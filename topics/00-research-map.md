# 调研总图：异质监督共同 SFT 的双侧回退

**首轮整理：2026-10-29。** 本文回答“大问题是什么—已有成果覆盖什么—哪些解释仍不成立—本案例处于什么位置”。逐篇证据、章节和代码见[论文索引](../papers/README.md)，来源深度见[检索与核查说明](../resources/search-and-verification.md)。

## 1. 从业务观察出发，保留两个参照

共同起点为用户提供的 Qwen-3.8-27B。NL2SQL-noCoT 与长程 Agent 轨迹分别单训时表现可接受；共同 SFT 后，同一个 checkpoint 在两个目标任务上均低于各自单训，也均低于共同起点。当前尚未附分数和训练配置；“可接受”没有被改写为“单训都超过 base”。完整定义见[问题文档](../problem.md)。

设两项面向业务的评测分数均为越高越好，用户观察可写为：

$$
S_{\rm SQL}(M_{\rm Mix}) < S_{\rm SQL}(M_{\rm SQL}),\qquad
S_{\rm Agent}(M_{\rm Mix}) < S_{\rm Agent}(M_{\rm Agent}),
$$

$$
S_{\rm SQL}(M_{\rm Mix}) < S_{\rm SQL}(M_0),\qquad
S_{\rm Agent}(M_{\rm Mix}) < S_{\rm Agent}(M_0).
$$

**我们的分析：**在这两个分数构成的平面内，当前混训结果被已有模型 M0 同时支配。因此，现象不能仅用“提升 SQL 必然牺牲 Agent”的取舍语言描述；至少存在一个已知单模型参照同时更好。但这并不证明各任务单训的最优表现能在同一个新模型内同时实现，也不涉及运行成本、统计显著性或未测任务。

| 对照 | 回答的问题 | 与本案例的关系 |
| --- | --- | --- |
| 混训与各自单训 | 共同训练是否保留任务专门适配的收益 | 直接对应第一层 |
| 微调模型与共同起点 | 训练后哪些已测表现回退 | 直接对应第二层 |
| 目标任务与未训练的保留任务 | 新能力获取是否伤害其他能力 | 相关，但不能替代两个参与训练目标的对照 |

## 2. 已有成果：五个不同的解释层次

下表是本仓库的整理框架，方法之间不是互斥关系，也不是性能排名。

| 层次 | 需要说清的对象 | 主要入口 | 已有证据的边界 |
| --- | --- | --- | --- |
| 数据组成与有效权重 | 抽到什么样本、哪些 token 监督、按什么单位平均 | [DMO](../papers/a-02-data-mixing-optimization.md)、[MFTCoder](../papers/r-01-mftcoder.md)、[Nemotron 3 Super](../papers/c-06-nemotron3-super.md) | 优化混合损失不自动等于两个业务成功率都恢复 |
| 学习动态 | 不同任务达到良好状态的速度、过拟合时点 | [CoBa](../papers/a-05-coba.md)、[mSFT](../papers/a-06-msft.md)、[DynamixSFT](../papers/a-03-dynamixsft.md) | 学习进度失衡与更新方向冲突不是同一个问题 |
| 示范与更新目标 | 低概率示范、参考策略、局部策略漂移 | [DFT](../papers/c-01-dft.md)、[PSFT](../papers/c-02-psft.md)、[iw-SFT](../papers/c-03-iw-sft.md) | 新知识学习、任务适配和外部保留可能需要不同信号 |
| 参数共享与更新空间 | 哪些参数共享，如何限制或调整更新 | [DiFT](../papers/a-01-dift.md)、[HFT](../papers/c-08-hft.md)、[LoTA](../papers/c-07-lota.md)、[PCGrad](../papers/r-02-pcgrad.md) | 参数关联或局部梯度性质不等于当前双降的因果解释 |
| 交互轨迹与输出协议 | 角色、工具调用、错误恢复、终止与行为格式 | [Agent-FLAN](../papers/b-02-agent-flan.md)、[AgentRefine](../papers/b-05-agentrefine.md)、[Agent-R](../papers/b-06-agent-r.md)、[终端监督长度](../papers/b-10-terminal-supervision-horizon.md) | 同类 Agent 监督的改进不等于异质任务兼容性已解决 |

**术语区别：**DiFT 是 Differential Fine-Tuning；DFT 是 Dynamic Fine-Tuning。两者目标、论文和代码不同。

### 2.1 样本混合比例没有完整定义训练目标

MFTCoder 的实现明确包含不同损失汇聚方式。Nemotron 3 Super 的官方 SFT 文档也区分输出 token 平均与 conversation 平均；其技术报告讨论了长输入、短输出任务的退化与两阶段训练。这些是本轮与监督结构最贴近的材料。[MFTCoder 源码](https://github.com/codefuse-ai/MFTCoder/blob/8ba13f44e2036f1412a343bc343dc6cda7383d73/mftcoder_accelerate/src/utils/loss_utils.py)、[NVIDIA 官方 SFT 文档](https://docs.nvidia.com/nemotron/latest/nemotron/super3/sft.html)

下面是对两种目标的数学解释，不是用户训练配置。令第 $i$ 个 conversation 有 $T_i$ 个实际监督的输出 token，平均 token 损失为 $\bar{\ell}_i$：

$$
L_{\rm token}=\frac{\sum_iT_i\bar{\ell}_i}{\sum_iT_i},
\qquad
L_{\rm conversation}=\frac{1}{N}\sum_i\bar{\ell}_i.
$$

**我们的推导：**若一个示意 batch 中两类 conversation 数量相同、监督长度恒为 50 与 5,000，则第一种平均里，长输出一侧约占 99% 的监督 token 项；第二种平均里，两类 conversation 的标量系数总量各占一半。这些数字仅用于说明公式，与当前数据无关。它们不是实际梯度范数、学习收益或因果影响的百分比。

由此，至少要区分样本条数、输入长度、监督输出长度、轮次和任务权重。工具观察可能很长却不参与 loss；同一轨迹可能被切成多个训练样本。不能从“Agent 轨迹更长”直接推出它在所有训练器里都支配优化。相关源码入口见[训练基础设施](../resources/training-infrastructure.md)。

### 2.2 收敛平衡、梯度幅度和梯度方向不能互相替代

CoBa 自身在 §7 明确不保证冲突任务均达到最优；mSFT 关注不同子数据集的过拟合时点。[CoBa 正文](https://aclanthology.org/2024.emnlp-main.459.pdf)、[mSFT v7](https://arxiv.org/pdf/2603.21606v7)

**我们的分析：**任务损失下降较慢、梯度较大、梯度方向相冲突，是三个不同观测命题。GradNorm、PCGrad、CAGrad 可作为基础入口，但其经典设置不构成当前 LLM 混训的直接验证。损失重新加权也可能改变方向相加的结果，却不因此自动消除所有任务之间的冲突。[基础笔记](../papers/r-04-gradnorm.md)

### 2.3 轨迹成功不等于每个中间 token 都应被同等模仿

SWE-Gym、SWE-smith 提供可验证任务和轨迹处理；AgentRefine、Agent-R 则进一步区分错误和修正内容。具体方法和比较对象见 [Agent 专题](02-agent-trajectories-and-retention.md)。

**我们的分析：**“高质量”至少可能指任务可执行、最终答案正确、中间行为合理、监督片段适合学生模型四种不同性质。短 SQL 结果监督与长程交互监督在这些性质上的差异值得描述，但不能把所有差异都称为已经证实的分布冲突。

## 3. 文献中的分歧、反例与需要保留的限制

| 容易形成的结论 | 一手资料给出的约束 | 本仓库的处理 |
| --- | --- | --- |
| 加通用数据越多，Agent 泛化越好 | AgentBank §7.2 的比例效应非单调，且其 agent-only 结果与 AgentTuning 的观察不同 | 保留数据与设置的条件性，不抽象成统一配比规则 |
| 新 SFT 目标能普遍兼顾学习与保持 | DFT 包含新知识学习的负例；PSFT 将混合数据保留为未解决范围 | 方法与反例一起读，不按摘要选“万能替换 loss” |
| 混合 thinking / non-thinking 已证明无干扰 | Qwen3 是多阶段后训练；长短 CoT 混合也不等于有／无 CoT 混合 | 区分阶段、监督形式和实际比较对象 |
| SQL 相关论文已直接解决当前 SQL 成功率 | LoTA 所列 SQL 指标为 ROUGE-1；AgentGym 的 BIRD 是交互式轨迹 | 保留指标及接口，不能只凭“SQL”关键词归为同题证据 |
| 仓库链接意味着训练代码齐全 | Agent-FLAN、AgentRefine、LS-Mixture 等公开资产范围不同 | 将作者身份、资产类型、核心源码和运行验证分开记录 |

对应原文及表格定位见 [AgentBank](../papers/b-03-agentbank.md)、[DFT](../papers/c-01-dft.md)、[PSFT](../papers/c-02-psft.md)、[Qwen3](../papers/c-04-qwen3-thinking-fusion.md)、[LS-Mixture](../papers/c-05-ls-mixture.md)、[LoTA](../papers/c-07-lota.md)。这里归纳的是本轮证据边界，并不否定这些工作在各自问题上的价值。

## 4. 从相邻研究到当前问题还有什么距离

**本轮覆盖结论：**已核读资料尚未完整覆盖“NL2SQL-noCoT + 长程 Agent 交互轨迹 + 共同起点 + 同一混训 checkpoint 相对两份单训和起点均双侧下降”的全部关系。此次是有主题筛选的首轮调研，并非穷尽检索；不能据此宣称首次发现或业界没有解决方法。

以下是由文献整理得到的研究空间判断，尚不是本项目发现：

1. **监督单位与目标任务的对应。** 一份训练样本、一个 conversation、一个决策、一个 token 和一次业务任务可能不一一对应。现有方法常在其中一个单位上优化，而业务目标在另一个单位上判断成败。
2. **联合适配与起点保持的双重要求。** 超过差的混训基线不足以回答问题；仅接近起点也未必保留单训的业务价值。这个区别比“平均 benchmark 提高”更贴近实际诉求。
3. **机制适用边界。** 格式偏移、学习动态、损失权重、参数干扰可能给出不同解释。把多种差异统称“异质性”有助于描述对象，但尚未提供可推广解释。
4. **长程表现与训练代理目标的差距。** 更低的 token loss、更多成功示范或更长监督，不足以逻辑推出整段交互的成功与可靠性。已有论文提供局部证据，统一论证仍需要更精确的对象。
5. **可推广贡献与任务对新颖性的区别。** 研究价值可来自解释条件、形成可验证的原则或提出有边界的方法；把已有技术应用到一对新任务，不自动构成充分的论文贡献。

当前较稳妥的研究表述是：

> 理解最终结果式监督与长程交互式监督在共同 SFT 中的相互作用，解释何时联合适配会使两个目标表现共同回退，以及怎样兼顾任务学习和起点表现保持。

这句话是研究方向，不是对某个机制或解决方案的预先承诺。

## 5. 建议先读的材料与理由

下列顺序服务于问题理解，只规定阅读优先级。

1. **Nemotron 3 Super + MFTCoder**：把“混合”具体到监督范围与汇聚方式；前者是工业报告，后者有可检查的多任务损失实现。
2. **DiFT + DMO**：分别理解任务交互与显式 SFT 混合优化，避免把所有问题归为一个静态比例。
3. **CoBa + mSFT**：认识共同 checkpoint 背后不同任务的学习进度；mSFT 会议身份保留作者声明标记。
4. **Agent-FLAN + AgentBank**：把 Agent 数据拆到格式、能力与数据组成，并保留相互不同的观察。
5. **DFT + PSFT**：理解修改更新目标的方向，同时读新知识学习负例和 mixed-data 限制。
6. **SWE-Gym + SWE-smith**：理解可验证轨迹与训练资产如何组织，区分纯 SFT、验证器和其他收益。

预训练混合的 DoGE、RegMix、Olmix，以及参数更新的 HFT、LoTA 和 LoRA 学习／遗忘分析，作为第二层背景。近期 PG-SFT 与终端监督长度论文单列跟踪，其中 PG-SFT 目前只有原始摘要级证据。

## 6. 当前成果的使用边界

本轮完成来源身份核查、关键章节阅读、作者代码链条及静态实现检查。代码入口存在不代表安装成功、论文结果可复现或适用于当前模型。本仓库当前保存调研材料；数学解释、证据归类和研究空间判断均不冒充本项目的新实验结果。

[多任务 SFT 专题](01-multitask-sft-and-mixtures.md) · [Agent 轨迹专题](02-agent-trajectories-and-retention.md) · [监督与能力保持专题](03-supervision-and-capability-preservation.md) · [代码地图](../resources/repository-catalog.md)
