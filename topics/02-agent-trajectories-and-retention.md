# Agent 离线轨迹 SFT、轨迹质量与已有能力保持

- 整理日期：2026-10-29
- 状态：第一轮已整理；广筛 15 项资料，形成 10 篇笔记，其中 9 篇核查了原文重点章节/表格，PG-SFT 为明确标注的摘要级线索。
- 范围：离线 Agent 监督、轨迹构造/质量、能力保留；不把平台性能、后续 RL 或验证器选择收益算作纯 SFT 结论。
- 完整来源与阅读深度：[B 线来源登记](../resources/b-source-register.json)

## 1. 当前业务观察与三种不同的问题

用户确认的观察是：Qwen-3.8-27B 为共同起点，NL2SQL-noCoT 与长程 Agent 轨迹分别 SFT 时可接受；同一个混训 checkpoint 在两侧都低于对应单训模型，并且两侧均低于共同 base。这是本仓库的问题背景，不是本轮文献推导出的结果。详见[问题定义](../problem.md)。

| 问题 | 训练与比较关系 | 本轮相关证据 | 与当前问题的距离 |
| --- | --- | --- | --- |
| Agent 目标提高，外部保留任务下降 | 新训练目标与未作为目标训练的能力分开看 | AgentTuning、AgentBank、PG-SFT | 相关，但外部保留任务不是第二个被共同训练的业务目标 |
| 单一 Agent 训练设定中的退化 | 只研究 Agent 类监督；某项结果低于起点，或更长监督低于较短监督 | AgentRefine 的部分 base 对照；终端监督长度研究 | 能说明 Agent SFT 本身存在条件性回退，不能归因于两类监督相遇 |
| 两项训练目标混合后共同低于 base | A-only、B-only 各自可用，A+B 的同一模型在 A、B 上都回退 | 用户确认的业务现象 | 本轮未核实到完整覆盖这一对照关系的已读论文；这不构成穷尽检索或创新性证明 |

表中前两行分别见 [AgentTuning 表4](../papers/b-01-agenttuning.md)、[AgentBank 表6](../papers/b-03-agentbank.md)、[PG-SFT 原始摘要](../papers/b-09-pg-sft.md)、[AgentRefine 表1](../papers/b-05-agentrefine.md)及[监督长度表2](../papers/b-10-terminal-supervision-horizon.md)。“单一 Agent 训练设定”可以包含多个 Agent 环境，不应误写成论文只训练了一个任务。

## 2. 文献覆盖了什么

| 文献 | 本轮阅读重点 | 对当前问题有价值的部分 | 不应跨越的证据边界 |
| --- | --- | --- | --- |
| [B01 AgentTuning](../papers/b-01-agenttuning.md) | Agent/通用数据混合与两类单训消融 | 混合训练可兼顾能力的早期实例 | 不能推出任意业务数据组合都有效 |
| [B02 Agent-FLAN](../papers/b-02-agent-flan.md) | 格式、推理、工具选择及参数监督 | 监督组成比“Agent 数据”标签更具体 | 格式假说尚不是本仓库双降原因 |
| [B03 AgentBank](../papers/b-03-agentbank.md) | 数据比例、CoT 消融、技能迁移 | 域外 Agent 泛化与通用能力保持可以不同向 | 不能把一个总体均值当作全面保持 |
| [B04 AgentGym](../papers/b-04-agentgym.md) | 多源轨迹及行为克隆表达 | 数据来源、角色、损失范围可核查 | 包含 BIRD-SQL 不等于同题混训研究 |
| [B05 AgentRefine](../papers/b-05-agentrefine.md) | 纠错内容、错误 token 监督、base 对照 | 成功轨迹内部仍有不同学习目标 | 优于其他 Agent-tuning 模型不等于超过 base |
| [B06 Agent-R](../papers/b-06-agent-r.md) | 搜索路径构造及及时修正监督 | 失败历史与模仿目标可以分开 | 迭代数据改进不是能力保持证明 |
| [B07 SWE-Gym](../papers/b-07-swe-gym.md) | 成功筛选、SFT、验证器各自收益 | 终局验证和行为指标的区分 | 不能把候选选择后的成绩归入纯 SFT |
| [B08 SWE-smith](../papers/b-08-swe-smith.md) | 任务生成、轨迹筛选、重复行为 | 难度、终局成功和训练收益不是同一概念 | 可验证软件任务不代表 SQL/Agent 兼容性 |
| [B09 PG-SFT](../papers/b-09-pg-sft.md) | 获取—保留权衡；摘要级 | 近期直接讨论 Agent SFT 的保留问题 | 全文未核查，模型/消融/代码不做推断 |
| [B10 终端监督长度](../papers/b-10-terminal-supervision-horizon.md) | 监督长度与重复任务可靠性 | 长度收益可以非单调，指标也可不同向 | 同类轨迹长度变化不是异质任务混合 |

## 3. 有价值的分歧与容易被掩盖的对照

**通用数据有益是条件性结论。** AgentTuning 的 agent-only 消融较差；AgentBank §7.2 则报告纯轨迹训练也能改善域外 Agent 表现，同时较小通用数据比例有益、继续增加比例会变差。两篇的数据覆盖、规模与训练配置不同，不能据此选出一个普遍正确的口号。[AgentTuning §3.4](https://aclanthology.org/2024.findings-acl.181.pdf)、[AgentBank §7.2](https://aclanthology.org/2024.findings-emnlp.116.pdf)

**相对其他微调模型的优势，与相对 base 的保留是两件事。** AgentRefine 表1中部分模型/任务低于未微调起点；Agent-FLAN 表4也存在小幅下降项。因此“模型平均更好”“方法优于基线”“所有能力保持”需要不同证据，标题和摘要不能代替逐列对照。[AgentRefine 笔记](../papers/b-05-agentrefine.md)、[Agent-FLAN 笔记](../papers/b-02-agent-flan.md)

**轨迹质量至少有四层含义。** 任务本身可执行、终局结果可信、中间行为值得学习、监督片段与学生状态相适应，是不同的判断。SWE-Gym/SWE-smith 侧重可验证任务与成功筛选；AgentRefine/Agent-R 对错误与修正片段作进一步区分；终端长度论文讨论监督范围和相对策略的可学习性。这里的四层划分是本仓库的综合理解，不是某篇论文已经验证的统一理论。[B05—B08](../resources/b-source-register.json)、[B10](../papers/b-10-terminal-supervision-horizon.md)

**数据标签不能替代监督定义。** NL2SQL 是任务域，Agent 是交互/执行方式；noCoT 表示目标没有显式推理文本，不表示任务不需要推理。长轨迹可能包含动作、观察和工具输出，不能直接等同长 CoT。本仓库据此保留任务内容、角色结构、显式推理、工具交互、长度与实际损失范围等维度，而不提前指定哪个维度导致双降。Agent-FLAN 的能力拆分、AgentGym 的标签构造和 B10 的 token/decision horizon 区别提供了相邻背景。[B02](../papers/b-02-agent-flan.md)、[B04](../papers/b-04-agentgym.md)、[B10](../papers/b-10-terminal-supervision-horizon.md)

## 4. 代码来源与实现完整度

本轮只做公开来源和静态文件核查，没有运行训练、评测或依赖安装。星数与最近活动由统一资源元数据维护，不在专题中重复抄写。

| 资产范围 | 本轮核实的项目 | 需要保留的说明 |
| --- | --- | --- |
| 有实质训练/数据处理入口 | SWE-Gym、SWE-smith、AgentGym | 具体依赖与版本仍影响可运行性；源码存在不是本地复现完成 |
| 有轨迹生成/修正代码，训练依赖外部框架 | Agent-R | 仓库已归档；README 依赖 XTuner/AgentGym |
| 主要是部署、评测、模型/数据入口 | AgentTuning | 没有把嵌套环境代码误认为主混训实现 |
| 作者数据/模型发布，主训练源码未核实 | Agent-FLAN、AgentRefine | 官方 GitHub 的存在不能等同完整训练代码 |
| 作者数据确认，专门 GitHub 未核实 | AgentBank | 使用论文指定 Hugging Face 链接，不补入第三方同名仓库 |
| 对应官方实现未核实 | PG-SFT、终端监督长度论文 | 未找到不等于没有发布；不拿通用框架冒充论文实现 |

每项的 README、目录或关键文件证据均保存在[对应笔记](../resources/b-source-register.json)中。

## 5. 已筛查但没有展开的资料

- **AgentOhana**：与多源轨迹统一表达有关，保留为补充线索；作者页面名为 Code 的链接实际指向模型资产，尚未核实完整训练源码。
- **OpenHands LM 32B 发布说明**：是 SWE-Gym 后续模型资产与训练叙述，不能当作独立会议论文；避免与 SWE-Gym 重复计数。
- **OpenHands、SWE-agent**：主要支持运行接口、环境和行为归因；留给通用基础设施资源整理，不扩成本专题的主论文列表。
- **Agent Q**：结合搜索与 DPO，作为行为克隆局限的邻近文献登记；本轮不扩展为 RL/偏好优化综述，也未把社区实现认作作者代码。

上述五项的原始来源、发表状态与未深读原因见[来源登记 B11—B15](../resources/b-source-register.json)。

## 6. 当前可以支持的判断

本轮文献说明 Agent 轨迹 SFT 的效果依赖数据组成、监督表达、起点与任务，既有能力改善，也有退化和保留不足；现有成功混训工作同时说明“两类能力本质不能共存”不是可以由当前业务现象直接得出的结论。

对本仓库更精确的定位仍是：**在两个目标分别可用的前提下，理解异质监督共同 SFT 的双侧回退及其适用条件。** 当前相关工作为这个问题提供邻近观察和概念区分，尚不能替代同题证据，也不足以单独确认某种因果机制或论文创新性。
