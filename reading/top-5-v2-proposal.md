# 新版五篇主读名单 · 待用户确认

日期：**2026-10-30**。状态：**候选方案，尚未确认为当前主读名单**。

[返回首页](../README.md) · [第一版及年代复核](top-5.md) · [本版筛选证据](../resources/top5-v2-proposal.json)

## 1. 建议名单

**DiFT → MFTCoder → DMO → Agent-FLAN → DFT。** 三篇发表于 2025—2026 年，两篇发表于 2024 年；其中四篇主会论文、一篇 Findings。顺序按共同精读的内容依赖安排，不是数值化的学术质量排行榜。

选择围绕共同起点下的 NL2SQL-noCoT 与长程 Agent 混合 SFT 双侧回退展开：先看近邻现象，再区分损失汇聚、数据混合、监督结构与 token 更新权重。没有预先认定其中某一项就是当前业务的原因。

| 顺序 | 论文／正式发表 | 本轮核心阅读问题 | 作者代码与关注度 |
| --- | --- | --- | --- |
| 1 | [DiFT](https://proceedings.iclr.cc/paper_files/paper/2026/hash/b304a04c6a4ac168aa692923814ace17-Abstract-Conference.html)，ICLR 2026 | 多任务 SFT 何时协同、何时冲突？混训、单训、base 的比较是否与我们一致？ | 作者实现及 Stars 未核实；先作为问题与证据论文 |
| 2 | [MFTCoder](https://doi.org/10.1145/3637528.3671609)，KDD 2024 | 任务配比如何通过有效 token、样本和任务级 loss 汇聚变成实际训练目标？ | [作者仓库](https://github.com/codefuse-ai/MFTCoder)：**711 Stars**；已见核心 loss／trainer |
| 3 | [DMO](https://proceedings.mlr.press/v267/li25bh.html)，ICML 2025 | 如何定义与选择 SFT 各数据域的混合权重？ | [作者仓库](https://github.com/realYuanLi/Data-Mixing-Optimization)：**0 Stars**；已见混合、SFT 启动及权重求解代码 |
| 4 | [Agent-FLAN](https://aclanthology.org/2024.findings-acl.557/)，Findings of ACL 2024 | Agent 轨迹中的格式、推理、工具选择和参数理解应如何拆开描述？ | [作者仓库](https://github.com/InternLM/Agent-FLAN)：**363 Stars**；数据／模型发布，主训练源码未核实 |
| 5 | [DFT](https://proceedings.iclr.cc/paper_files/paper/2026/hash/2e53c02ea028cbf603f4b6b47fef3d97-Abstract-Conference.html)，ICLR 2026 | 改变示范 token 的更新权重，会怎样影响目标学习与泛化？ | [作者仓库](https://github.com/yongliang-wu/DFT)：**928 Stars**；已见核心 trainer／loss |

Stars 由 GitHub API 于 **2026-10-30** 复查；“已见代码”表示静态核查，不表示已经运行复现。DiFT 的未知值不计作 0。论文／代码完整核查记录见各篇笔记。

## 2. 每篇的入选理由与边界

### 1）DiFT：优先对齐混训退化的比较对象

完整标题：**Differential Fine-Tuning Large Language Models Towards Better Diverse Reasoning Abilities**。

本轮最先读它的问题设定、Table 1／2，再读参数区别更新的方法。原文一个 Mistral-7B 代码＋逻辑组合在数值上两侧均低于各自单训与共同 base，构成相邻证据。但部分下降很小，Table 1 的箭头与数值有矛盾；它也不是 SQL＋Agent 设置。不能把“有一个相邻组合”扩展成普遍机制。[笔记与原表对照](../papers/a-01-dift.md)

### 2）MFTCoder：保留一个可以读到具体 loss 的基础入口

完整标题：**MFTCoder: Boosting Code LLMs with Multitask Fine-Tuning**。

先用当前代码理解配比怎样与 token、样本及任务级归一化共同定义训练目标，再读 DMO 如何选择权重，二者互补。保留它的理由是这个具体作用，而非单纯因为引用／Stars 较高。当前仓库包含后续 CoBa，需区分原论文方法与后来实现。仅凭轨迹长短不能断定某任务主导梯度，还取决于 mask、归一化和批次组成。[笔记及固定版本代码](../papers/r-01-mftcoder.md)

### 3）DMO：把“混合比例”变成可定义的问题

完整标题：**Data Mixing Optimization for Supervised Fine-Tuning of Large Language Models**。

它直接研究 SFT 数据混合，并基于验证损失建模求权重；原文还比较原始比例、等 token、等条数与优化比例。对我们“SQL 条数多而输出短，Agent 条数少而轨迹长”的差异，这比单看数据集数量更有启发。其平均验证损失目标不等于 SQL 执行正确率与 Agent 任务成功率同时恢复。[笔记](../papers/a-02-data-mixing-optimization.md)

0 Stars 是明确短板，意味着不能从社区关注度获得额外信心。本次入选依据是问题直接、ICML 正式发表以及已有实质实现，不据此将仓库称为成熟工具。

### 4）Agent-FLAN：保留 Agent 监督结构的视角

完整标题：**Agent-FLAN: Designing Data and Methods of Effective Agent Tuning for Large Language Models**。

它帮助我们避免把长程 Agent 轨迹简单视为“更长的普通答案”：格式、推理、工具选择与参数理解的监督性质不同。阅读用途是把业务问题描述清楚，不能据此断言当前退化就是格式冲突；其模型、轨迹与评测均不等同当前业务设置。[笔记](../papers/b-02-agent-flan.md)

### 5）DFT：补充近期目标函数视角

完整标题：**On the Generalization of SFT: A Reinforcement Learning Perspective with Reward Rectification**；方法名 **Dynamic Fine-Tuning**，与第一篇 Differential Fine-Tuning 不同。

通过停止梯度的当前 token 概率重新加权交叉熵，提供一个与数据配比、参数更新范围不同的视角。必须连同原文事实知识负例阅读；不能从推理任务收益推出 SQL＋Agent 混训同样有效。[笔记](../papers/c-01-dft.md)

## 3. 相比第一版改了什么

- **新增 DiFT、DMO**：分别补充近期混训冲突证据与 SFT 数据混合优化。
- **保留 MFTCoder、Agent-FLAN、DFT**：分别承担损失实现、Agent 监督结构、近期 token 更新目标三个互补用途。
- **DMT 与 AgentTuning 移为背景材料**：继续保存在资料池；在只有五篇的限制下，当前优先阅读更直接的近期证据与方法。

PSFT 本轮不占五篇名额。它与 DFT 的公式和作用并不相同，具有能力保持价值；但正式论文仍将混合数据验证留作未来工作。对当前任务，优先保留 MFTCoder 来补足实际混合目标的定义，避免五篇中两个名额都主要用于目标函数／更新方式。[PSFT 笔记及局限](../papers/c-02-psft.md)

## 4. 引用指标如何保留

沿用 **2026-10-29** 已核实的 OpenAlex 快照：MFTCoder 的[正式／预印本合并条目](https://api.openalex.org/works/W4401863706)为 **23**，Agent-FLAN 的[合并条目](https://api.openalex.org/works/W4402683785)为 **13**。DiFT、DMO、DFT 的跨版本引用数尚未核全，不写成整篇零引用，也不将这份名单描述为高被引前五。

本轮优先关系为：**问题及比较对象匹配 → 结果证据与局限 → 正式发表及实质实现 → 考虑公开时间的引用和 Stars**。没有为了新年份预设名额，也没有为这些指标编造精确分数。

本文件作为待确认提案保存。用户确认后，再统一更新主读入口与后续逐篇学习顺序。
