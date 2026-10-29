# 监督表达、长度权重与能力保持：异构 SFT 为何可能两侧都下降

更新：2026-10-29。范围：15 项一手来源筛选，9 篇重点笔记；书目信息和代码核验见 [C 线来源登记](../resources/c-source-register.json)。本专题只整理研究与证据，不提出新的实验方案或训练实现。

## 一、大问题：一个模型学习多种输出约定时，究竟在优化什么

当前现象首先是一个**多种监督目标进入同一 checkpoint 后，目标能力与已有能力如何兼顾**的问题。数据集的名称只能描述其中一部分：SQL 与 Agent 既有任务差异，也可能有输出格式、交互结构、长度、示范来源和损失位置的差异。把它们合并为“两个领域混训”会遗漏这些结构；把它们简化成“有 CoT 和无 CoT 冲突”又会提前指定原因。

本文使用以下区分，它们是概念定义，不是对用户数据的测量结果。

| 维度 | 应表达的对象 | 不能据此推出的结论 |
|---|---|---|
| 任务内容 | 查询构造、规划、工具操作等能力 | SQL 只有记忆；Agent 全是推理 |
| 外显监督 | 最终 SQL、文字推理、工具调用、最终答复 | 没写 CoT 就不需要推理 |
| 交互结构 | 单轮或多轮，环境观察与模型行动 | 总轨迹越长，模型连续思考就越长 |
| 优化对象 | loss mask 后实际参与训练的 token | 输入上下文长度就是监督长度 |
| 能力保持 | 相对共同 SFT 起点的行为变化 | 参数变化小就等于业务目标已学会 |

这些维度对应不同的文献路线。Qwen3 讨论显式输出模式，Nemotron 3 Super 讨论损失聚合，DFT／iw-SFT／PSFT 改变更新权重，LoTA／HFT／LoRA 则改变参数更新的范围或结构。它们各自覆盖问题的一部分，不能仅因都使用“泛化”或“遗忘”一词而视为同一种解释。[Qwen3 §4](https://arxiv.org/html/2505.09388v1)；[Nemotron §3.1](https://research.nvidia.com/labs/nemotron/files/NVIDIA-Nemotron-3-Super-Technical-Report.pdf)。

## 二、已有成果：四个不同层次的干预

### 2.1 监督表达：把需要生成的形式作为条件

[Qwen3](../papers/c-04-qwen3-thinking-fusion.md)的意义在于同一模型可以通过明确模式表达两种输出习惯；但其完整流程包含 SFT 与 RL 的多阶段组合，融合阶段也存在推理指标代价。[LS-Mixture](../papers/c-05-ls-mixture.md)更接近纯 SFT 的长短表达混合，不过比较对象是同题长短 CoT，短样本并非无 CoT。[Qwen3 §4.3、Table 22](https://arxiv.org/html/2505.09388v1)；[LS-Mixture §2、Tables 3–4](https://arxiv.org/html/2505.03469v1)。

**本综述推断**：相关研究首先支持一个表达层面的区分：模型需要学习“如何解决”，也需要学习“以什么形式回答”。如果不同数据源对第二个问题给出不同约定，任务标签未必足以表达这种差别。然而，由格式差异推出不可兼容，仍然缺少一步；条件充分的单模型可以表达多种行为，关键是训练、输入条件与评价要求是否指向同一分布。

2026 年的 *Is Next-Chunk Reasoning RL Really Better than SFT?* 已出现在 EMNLP 正式论文集中，其摘要涉及 no-CoT 与长 CoT 的 Mixed SFT，以及后续 RLVR 的结果。这是优先跟进的相邻研究，但本次仅取得正式页面和摘要，未取得可核读全文，故不把摘要中的优势作为已核验的预算公平或直接解决当前问题的证据。[EMNLP 2026 正式记录](https://aclanthology.org/2026.emnlp-main.921/)。

### 2.2 长度与聚合：样本比例不等于目标权重

[Nemotron 3 Super](../papers/c-06-nemotron3-super.md)提供本线最接近实际异构混合的工业观测：其 SFT 数据包含 SQL 与 Agentic 类型，并明确区分全局 token 均值和 conversation 均值。但两阶段调整伴随其他变化，不能作为单独归一化的因果证明。[官方报告 §3.1–3.1.1](https://research.nvidia.com/labs/nemotron/files/NVIDIA-Nemotron-3-Super-Technical-Report.pdf)；[官方 SFT 文档](https://docs.nvidia.com/nemotron/latest/nemotron/super3/sft.html)。

以下是**本综述的数学说明，不是用户训练日志的结论**。对一个 batch 的 conversation $i$，令 $M_i$ 是实际被监督的 token 集合，$m_i=|M_i|>0$，$\ell_{it}$ 是该 token 的负对数似然，$\bar\ell_i=m_i^{-1}\sum_{t\in M_i}\ell_{it}$。两种目标为：

$$
L_{\mathrm{token}}=\frac{\sum_i m_i\bar\ell_i}{\sum_i m_i},
\qquad
L_{\mathrm{conversation}}=\frac{1}{B}\sum_i\bar\ell_i.
$$

前者给每个被监督 token 相同系数，因此长输出 conversation 的总系数更大；后者先在 conversation 内归一化。这里的 conversation 可以包含多个 assistant 回合，不能随意替换为“最后一个回答”。

假设按 conversation 以概率 $q_k$ 抽取任务 $k$，且采用足够大的全局 token 聚合，前者的总体目标近似为：

$$
L_{\mathrm{token}}\approx
\frac{\sum_k q_k\,\mathbb{E}[m\bar\ell\mid k]}
{\sum_j q_j\,\mathbb{E}[m\mid j]}
=\sum_k\alpha_k R_k^{\mathrm{token}},
\quad
\alpha_k=\frac{q_k\mathbb{E}[m\mid k]}{\sum_j q_j\mathbb{E}[m\mid j]},
\quad
R_k^{\mathrm{token}}=\frac{\mathbb{E}[m\bar\ell\mid k]}{\mathbb{E}[m\mid k]}.
$$

该写法特意保留长度与损失的相关性；不能在没有条件时把 $\mathbb{E}[m\bar\ell]$ 拆成两项期望相乘。conversation 均值对应的目标则为 $\sum_k q_k\mathbb{E}[\bar\ell\mid k]$。有限 batch 的随机分母、microbatch 平均、梯度累积、packing 与跨设备 reduction 会改变实现中的精确关系。

这个推导只说明**目标系数**，不等于梯度范数、有效学习量或最终性能。低系数的重要 token 仍可能产生显著更新，高系数的容易 token 也未必主导模型变化。尤其不能根据“Agent 总序列更长”就宣布它压制 SQL：工具观察是否 mask、历史 assistant 是否监督，以及每条数据如何组织，都仍未知。

### 2.3 损失目标：三种“加权”并不相同

| 路线 | 权重或约束来自哪里 | 原文证据的主要范围 | 对当前问题仍未建立的结论 |
|---|---|---|---|
| [DFT](../papers/c-01-dft.md) | 当前模型的目标 token 概率，停止梯度 | 推理、代码、多模态及明确的事实知识反例 | 低概率示范在 SQL／Agent 中是否可被安全降权 |
| [iw-SFT](../papers/c-03-iw-sft.md) | 当前与参考／中间策略的分布关系 | 精选数据下的目标联系及推理实验 | 异构教师、筛选机制与参考策略是否匹配 |
| [PSFT](../papers/c-02-psft.md) | 相对可更新旧策略的 clipped 概率比 | 目标学习与域外保持，部分扩展设置 | 混合数据能否同时避免两个目标退步 |

对应一手来源：[DFT §3、§4.5](https://arxiv.org/html/2508.05629v3)，[iw-SFT §3、Appendix A](https://arxiv.org/html/2507.12856v2)，[PSFT §3、Appendix A](https://proceedings.iclr.cc/paper_files/paper/2026/file/da75d2bbf862b86f10241d0887613b41-Paper-Conference.pdf)。

**本综述推断**：归一化决定“conversation 相互如何比较”，概率加权决定“其中哪些 token 获得多大更新”；两者并非替代关系。一个方法即使改善单任务推理，也可能让尚未学会的业务术语或少见动作更难进入模型。另一方面，置信度低也可能确实来自错误、冗余或不匹配的示范。仅靠概率不能区分这些情形。

PSFT 使用会更新的旧策略，因此其局部稳定性与“始终贴近共同 base”不是同一保证。DFT 的 NQ 反例也说明，加权目标的价值需按学习对象判断，而不是由论文总体平均收益决定。以上是对适用边界的推断，不是对当前数据质量或训练原因的断言。

### 2.4 参数更新：保持已有能力与学会新能力的共同边界

[LoTA](../papers/c-07-lota.md)通过校准得到稀疏更新区域，[HFT](../papers/c-08-hft.md)按阶段冻结部分参数，[LoRA 学习／遗忘研究](../papers/c-09-lora-learning-forgetting.md)比较低秩与全参更新的权衡。三者提供了参数层面的证据，但不能把“更少参数变化”自动翻译为“更好的业务解”。[LoTA §3、§5](https://arxiv.org/html/2406.16797v2)；[HFT §3–4、Algorithm 1](https://aclanthology.org/2025.acl-long.626.pdf)；[LoRA 最终版 §4.3–4.7](https://arxiv.org/html/2405.09673v2)。

**本综述推断**：能力保持至少有两条轴：旧行为保留多少，新目标获得多少。只看旧能力分数会偏爱“几乎没有学”的模型，只看新任务峰值则可能忽略保持代价。不同参数限制改变的可能既是相互干扰，也可能是可学容量、优化速度或训练的有效程度；现有结果不允许把这些因素统一称为已解决的“冲突”。

## 三、局限与尚未解决的问题

### 3.1 已有成功案例之间，证据对象并不相同

多阶段工业模型、同题长短解答、单领域 SFT 的域外保持、连续任务的 backward transfer，以及模型合并，回答的是不同问题。跨这些设置迁移结论需要额外假设。此次重点核读中，尚未见到直接匹配“共同起点、SQL-noCoT 与长 Agent 两路分别单训、再混训，且混训两侧都低于共同 base”的完整受控证据。这是**本次调研的覆盖边界**，不是宣称整个领域没有相关工作或已经证明论文新颖性。

同样，任务名称并不足以对齐指标。例如 LoTA 的 SQL 列是 ROUGE-1，不能直接用来判断执行语义；后续 RL 的峰值也不能替换 SFT checkpoint 的即时表现。不同指标和训练阶段之间的改善，没有可直接相加的共同单位。[LoTA Table 3](https://arxiv.org/html/2406.16797v2)；[Mixed SFT 论文摘要](https://aclanthology.org/2026.emnlp-main.921/)。

### 3.2 保持、选择性学习与监督充分性之间仍有张力

**本综述推断**：降低不可信示范的影响，有助于避免学坏；降低所有不熟悉示范的影响，却可能阻止必要的新知识进入。为每个 conversation 平等分配权重，改变了长短样本的相对贡献，但没有回答哪一条轨迹值得平等对待。冻结参数约束了变化范围，也没有证明剩余空间足以承载新协议。这三个张力分别发生在示范可信度、样本价值、可更新容量上，不能只用“更保守”概括最优方向。

因此，一套普适理论需要说明：何时希望重用已有能力，何时必须获得新行为，哪些外显步骤是必要监督，哪些只是某位教师的表达偏好。现有文献给出的是局部答案，而非已经适用于所有异构 SFT 的统一规则。

### 3.3 代码公开与证据成熟度需要单独记录

以下状态来自作者身份链、README 与相关文件的静态核查；没有运行训练，不等同于复现实验。准确路径、blob 与 stars 时间戳在各卡和 JSON 中。

| 工作 | 本次实际确认的公开材料 |
|---|---|
| DFT、iw-SFT、PSFT | 已见核心训练／损失实现 |
| LoTA | 已见 mask 实现和连续训练脚本；README 标 work in progress |
| Nemotron 3 Super | 已见 SFT recipe；未核实完整生产两阶段配方可直接复现 |
| Qwen3 | 模型、推理与框架说明；不等于原始融合管线开放 |
| LS-Mixture | Vue／Vite 项目网站与方法展示，未见训练代码 |
| LoRA Learns Less and Forgets Less | checkpoint 和配方说明；所读仓库未见训练源码 |
| HFT | 未确认作者官方仓库 |

另有版本陷阱：PSFT 限制段落与新增 MoE 附录互相不一致；HFT 需读正式算法而非把 stage 改写成 epoch；LoRA 应采用最终 v2，不能沿用早版的过度概括。这些差异会影响可据文献提出的主张，不能在汇总表中省略。[PSFT 正式 PDF，Appendices A、C.3](https://proceedings.iclr.cc/paper_files/paper/2026/file/da75d2bbf862b86f10241d0887613b41-Paper-Conference.pdf)；其余见对应卡。

## 四、与当前混训双降的关联：能说什么，不能提前说什么

令 $B$ 为用户指定的共同 SFT 起点，$S$ 为 SQL 单训模型，$A$ 为 Agent 单训模型，$M$ 为混训模型；$f_S$、$f_A$ 分别为两侧评价。当前已知关系是：

$$
f_S(M)<f_S(S),\quad f_S(M)<f_S(B),
\qquad
f_A(M)<f_A(A),\quad f_A(M)<f_A(B).
$$

“单训可接受”不推出 $f_S(S)>f_S(B)$ 或 $f_A(A)>f_A(B)$。这里的 base 是共同训练起点，不预设为原始预训练权重。用户称其为 Qwen-3.8-27B；本专题不依据 Qwen3 报告推定该 checkpoint 的具体训练历史。

**本综述推断**：比起只有一侧下降，双侧低于共同起点更强地要求区分以下解释层次，但现有信息尚不能在它们之间定因：

- **输出约定改变**：模型仍可能保有求解能力，却生成了不符合该侧评价的形式；也可能是求解本身退化。二者不是相同的能力损失。
- **有效目标改变**：相同的名义数据比例，在不同监督 mask、长度和 reduction 下形成不同的训练目标；但权重不同并不自动证明是性能原因。
- **共享参数上的学习与保持失衡**：目标任务未充分获得、已有行为受损、或两者并存；“灾难性遗忘”只是其中一类表述，不能取代诊断。
- **示范分布与评价分布不一致**：教师风格、交互环境和正确性筛选可能改变被拟合的目标。这部分与 Agent 轨迹质量专题相接，不能只靠长度解释。

由此可形成有推广价值的研究问题表述：**当监督在外显表达、交互结构和监督密度上异构时，单模型 SFT 如何同时获得目标行为并保持已有能力；现有概率加权、归一化与参数约束各在哪些条件下有效？** 这是一种从业务现象提炼问题的方式；是否构成新贡献，还取决于与更广文献的区别和后续证据。当前资料不支持提前把“SQL-noCoT 与 Agent 不可兼容”或某一种改法作为答案。

## 五、阅读次序与资料边界

建议先读 [Nemotron 3 Super](../papers/c-06-nemotron3-super.md)理解混合监督与长度聚合的现实语境，再读 [Qwen3](../papers/c-04-qwen3-thinking-fusion.md)和 [LS-Mixture](../papers/c-05-ls-mixture.md)澄清表达模式。随后并列阅读 [DFT](../papers/c-01-dft.md)、[PSFT](../papers/c-02-psft.md)、[iw-SFT](../papers/c-03-iw-sft.md)，最后用 [LoTA](../papers/c-07-lota.md)、[HFT](../papers/c-08-hft.md)与 [LoRA](../papers/c-09-lora-learning-forgetting.md)补充参数保持视角。

扩展登记只保留少量边界材料：Rho-1 属于预训练 token 选择，LIMA 是早期精选监督背景，SFT／RL 比较与 continual RFT 研究不直接回答本轮纯 SFT 混训问题；SFT-as-Context 使用推理时组合，不能替代一个混训 checkpoint 的要求。这些仅做初筛，不冒充全文精读。完整状态、优先级和未核实项见 [来源 JSON](../resources/c-source-register.json)。
