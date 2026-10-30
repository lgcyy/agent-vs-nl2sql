# 第二篇精读 · MFTCoder：混合数据怎样变成训练目标

日期：**2026-10-30**。当前阅读顺序：**MFTCoder → DMO → Agent-FLAN → DFT**。DiFT 已完成两轮阅读，用户决定暂不优先考虑该方法，材料转为背景参考。

[当前阅读名单](top-5-v2-proposal.md) · [论文卡片](../papers/r-01-mftcoder.md) · [数字与源码核查记录](../resources/mftcoder-reading-evidence.json)

## 1. 这一篇要理解的问题

当前业务的数据形态是 NL2SQL-noCoT 条数多、输出短，Agent 轨迹条数少、交互长。在没有看到具体训练实现前，这些描述还不能确定哪一类数据在训练目标中获得较大权重。

本轮学习 MFTCoder 的核心数据平衡思路：**先按任务分别计算有效监督 token 的平均损失，再对任务作平均或显式加权。** 需要区分任务数据怎样抽取、哪些 token 参与监督，以及损失如何汇聚。它们共同决定实际训练目标。

**版本边界：**正式发表为 [KDD 2024](https://doi.org/10.1145/3637528.3671609)。下面的方法公式和主表数值核读自作者公开的 [arXiv:2311.02303v1](https://arxiv.org/html/2311.02303v1)，§3.5、§4.1–4.3、§6；不声称会议终版和 v1 已逐项核对一致。源码固定在作者仓库提交 `8ba13f44e2036f1412a343bc343dc6cda7383d73`，该代码包含后来的功能，不能全部归入 v1 的实验证据。

## 2. 一个能看出差别的教学例子

**以下数字是本仓库自造示例，不是业务统计或论文实验。** 假设一次损失汇聚中同时包含两类任务，没有 packing 或切分，一条原始记录对应一行输入序列：

| 任务 | 条数 | 每条有效监督 token | 合计有效监督 token |
| --- | ---: | ---: | ---: |
| SQL | 10 | 50 | 500 |
| Agent | 1 | 2,000 | 2,000 |

这里强调的是**有效监督 token**。一条轨迹的总文本可能包含用户消息、工具观察、历史上下文；这些内容是否直接作为预测目标进入 loss，要看实际 mask。被屏蔽为预测目标不意味着输入内容不会影响模型预测。

### 方式一：整批按 token 平均

令两类任务各自的 token 平均损失为 ℓ_SQL、ℓ_Agent，则：

$$
L_{\rm token}=\frac{500}{2500}\ell_{\rm SQL}
+\frac{2000}{2500}\ell_{\rm Agent}
=0.2\ell_{\rm SQL}+0.8\ell_{\rm Agent}.
$$

虽然 SQL 条数更多，但这个汇聚目标给 Agent 的任务均值系数更大。这里说的是数学系数，不是已经测得的梯度范数贡献或最终能力变化。

### 方式二：先每条平均，再平均所有条目

令每条序列先按自己的有效监督 token 数归一化；再对十一条序列平均。此时 SQL 组占条目平均的 10/11，Agent 占 1/11，约为 **90.9% / 9.1%**。

它降低了长序列因 token 数更多而获得的组内权重，但样本多的任务仍有更大系数。这一方式的任务内部单位是序列均值，与方式一的任务 token 均值不总是同一个量。

### 方式三：先每个任务平均，再平均任务

采用原文 Eq.1 所表达的任务归一化：

$$
L_{\rm task}=\tfrac12\ell_{\rm SQL}+\tfrac12\ell_{\rm Agent}.
$$

无论本次每个任务有多少条、多少有效 token，两类任务的 token 均值在这个局部汇聚中的系数均为 1/2。任务内部仍按 token 平均，所以同任务的长短样本没有因此变成等权。

三个目标可以对照如下：

| 汇聚方式 | SQL 组系数 | Agent 组系数 |
| --- | ---: | ---: |
| 整批有效 token 平均 | 20% | 80% |
| 原始条目先平均，再条目平均（本例无 packing） | 90.9% | 9.1% |
| 任务内 token 平均，再任务等权 | 50% | 50% |

**同样的数据能定义不同的优化目标。** 但这不证明 50/50 对业务最优，也不证明任一任务的梯度一定更大。梯度还取决于各监督位置的模型误差与导数方向。

## 3. 原文究竟验证了哪一层

v1 §3.5 区分数据量、任务难度和收敛速度三类平衡，并描述 focal 与 FAMO 启发的扩展。但 §4.1 明确说主要比较使用 **Data-Balance Loss（Eq.1）**。因此，本轮首先学习任务内部归一化，不能把三种平衡都当作主表已验证的组合。

原文把普通 SFT-Mixed 描述为把多任务合并为一个任务；MFT 保留任务分组。**依据这一描述作数学改写**，当所有数据被视为一个组时，Eq.1 退化成整批 token 平均；保留多个组时，先各组 token 平均、再任务平均。这解释了“都用 Eq.1”与“结果不同”并不矛盾。

Table 2（v1 PDF 第 10 页）给出：

| CodeLlama-13B-Python 系列 | HumanEval pass@1 | MBPP pass@1 |
| --- | ---: | ---: |
| 对应任务各自单训 | 59.76% | 54.2% |
| 普通混训 SFT-Mixed | 57.93% | 53.6% |
| MFT-5Tasks | 60.37% | 56.0% |

单训这一行由两个模型各自的目标成绩组成。按原表重算，MFT 相对 Mixed 提高 **2.44 / 2.40 个百分点**，相对单训提高 **0.61 / 1.80 个百分点**。这是正面相邻证据；没有直接证明 SQL＋长程 Agent 的双侧恢复。

**比较条件：**主实验底座、可训练位置及初始化一致，使用 QLoRA 和 pack；Mixed/MFT 使用相同的五类代码训练数据。各模型按验证 loss 独立早停，不能称为相同训练步数或相同累计 token 预算；主表没有给多次运行误差范围。其他语言／代码翻译子项仍有 Mixed 或单训领先，不能从两个表列推出全面支配。

论文的 Text-to-SQL 结果属于未在这五类训练数据中直接训练的迁移任务。它没有训练 SQL＋Agent 混合数据；不能把出现 Spider／BIRD 名称当作已覆盖当前问题。

## 4. 对应到固定版本代码

主要入口是 [loss_func_mft](https://github.com/codefuse-ai/MFTCoder/blob/8ba13f44e2036f1412a343bc343dc6cda7383d73/mftcoder_accelerate/src/utils/loss_utils.py)。下面描述其交叉熵分量，假设参与平均的相关分母非零。

对当前调用的一行输入 i，定义有效监督 token 数 N_i、token loss 总和 S_i。对任务 k，I_k 表示这个调用中属于该任务的输入行，K_B 表示当前出现的任务集合。

| 分支 | 静态核查的计算顺序 | 是否读取 task_weights |
| --- | --- | --- |
| case1 | 全部有效 token 的 loss 总和／有效 token 总数 | 否 |
| case2 | 每行先计算 S_i/N_i，再对行平均 | 否 |
| case3 | 每任务内先按 token 平均，再按出现任务的权重汇聚 | 是 |
| case4 | 每任务内先对行均值平均，再按出现任务的权重汇聚 | 是 |
| coba | 使用 case3 的 reduction 分支，由训练器更新任务权重 | 是 |

case3 的实际局部公式为：

$$
L_B=\sum_{k\in K_B}\frac{w_k}{\sum_{r\in K_B}w_r}
\frac{\sum_{i\in I_k}S_i}{\sum_{i\in I_k}N_i}.
$$

注意分母只含**当前调用出现的任务**，并不自动保留全数据所有任务的权重。这个批次实现与整份数据集的任务平均目标之间，还隔着采样、批次组织和分布式汇聚。

### 三个容易误解的实现细节

第一，**sample 在这里首先是张量的一行**。启用 packing 后，一行可能拼入多条记录；case2／case4 并没有恢复原始记录边界，不能直接叫“每条原始 SQL／每条原始轨迹等权”。依据：[编码与 packing](https://github.com/codefuse-ai/MFTCoder/blob/8ba13f44e2036f1412a343bc343dc6cda7383d73/mftcoder_accelerate/src/data/preprocess_data.py)。

第二，**实例 weight、采样权重与 task_weights 不应混为一谈**。本次核读的 collator 虽读取实例 weight，却未将其加入返回 batch；训练器传给 loss 的是 task_weights。case1／case2 不使用后者。默认 SFT 的问答／对话编码还会区分 prompt 与 answer 的监督 mask，collator 将 mask 与 next-token label 对齐；不能只按原始文本长度计数。依据：[collator 与参数解析](https://github.com/codefuse-ai/MFTCoder/blob/8ba13f44e2036f1412a343bc343dc6cda7383d73/mftcoder_accelerate/src/pefts/mft_accelerate.py)、[训练器](https://github.com/codefuse-ai/MFTCoder/blob/8ba13f44e2036f1412a343bc343dc6cda7383d73/mftcoder_accelerate/src/pefts/mft_trainer.py)。

第三，**局部任务等权不等于全训练过程等权**。假设某个 microbatch 只有 SQL，那么 w_SQL 同时出现在分子和权重分母，会相消。若十个等系数参与优化的 microbatch 中九个只有 SQL、一个只有 Agent，仅把两任务权重都设成 1，并不会使整体目标自动五五开。这是由公式推导的例子，不是实际训练观测。

本版本先在当前 rank／microbatch 中计算归一化 loss，再调用 accelerator.backward；loss 函数没有在归一化前跨 microbatch 或跨 rank 汇总这些分母。因此，不能仅凭函数内的等权公式就宣称已实现全局目标。这里没有对某个分布式后端作运行验证。

## 5. 任务平衡之后还剩什么

v1 §6 明确讨论了任务划分与收敛不同步的问题。任务权重相同，仍可能一个任务先过拟合，另一个尚未学好；让它们同步也不自动消除共享参数上的更新冲突。

从我们的角度，任务等权是一种可明确描述的目标选择。它既不保证数据质量，也不保证两侧业务指标共同恢复。用户现有训练可能已经采用类似归一化，本轮没有读取业务配置，所以不把这个差异诊断为已找到的原因。

后续 CoBa 是另一篇工作与仓库后续实现；本轮将其列为版本边界，不把其动态权重收益倒归原始 MFTCoder。沿已确认名单继续阅读 DMO 时，可以承接的问题是：**在知道任务均值如何定义之后，各任务的权重又该如何选择。**

## 原始来源与本轮交付

- [KDD 正式发表条目](https://doi.org/10.1145/3637528.3671609)：正式身份；全文技术阅读采用作者 v1。
- [作者预印本 v1](https://arxiv.org/html/2311.02303v1)：§3.3、§3.5、§4.1–4.3、§6；Eq.1 与 Table 2 结合 PDF 截图核读。
- [固定提交的作者仓库](https://github.com/codefuse-ai/MFTCoder/tree/8ba13f44e2036f1412a343bc343dc6cda7383d73)：loss、trainer、collator、preprocess 静态审阅；未执行源代码。
- [核查记录](../resources/mftcoder-reading-evidence.json)：原始表值、独立算术、源码 blob 标识及教学例子。

本轮没有运行训练或作业务归因。10 条 SQL／1 条 Agent 的示例、权重反例及公式改写均为本仓库教学分析。
