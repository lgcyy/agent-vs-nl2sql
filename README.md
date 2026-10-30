# agent-vs-nl2sql

> NL2SQL-noCoT 与长程 Agent 轨迹混合 SFT 的双侧性能回退：问题、已有解释与相关研究。

本仓库集中保存中文专题综述、论文阅读笔记和代码资源。目标是解决当前业务中的混训双侧回退，逐步形成有证据支持、可推广的研究结论与潜在论文贡献。当前阶段以调研资料积累和问题对齐为主。

## 当前核心问题

以用户提供的 **Qwen-3.8-27B** 为共同 SFT 起点，NL2SQL-noCoT 数据与长程 Agent 轨迹分别单训时，各自表现可接受；当前同一个混训 checkpoint 在两侧均低于对应单训模型，且两侧均低于共同起点。

现象来自用户确认。“单训可接受”不扩写成“单训都超过 base”；noCoT 不表示任务无需推理，长程轨迹不自动等于长 CoT。完整比较关系和证据状态见[问题定义与研究目标](problem.md)。

## 第一轮调研 · 2026-10-29

| 交付 | 数量与范围 |
| --- | --- |
| 论文与技术资料 | **51 项**：50 项论文／报告，1 项模型发布说明 |
| 独立阅读笔记 | **34 篇**；阅读深度逐项标注，另有 17 项初筛记录 |
| 综合综述 | **1 篇总图 + 3 篇专题** |
| 主要代码／平台项目 | **37 个**；保留作者归属、资产范围、Stars 和核查日期 |
| 覆盖主线 | 多任务混合、Agent 轨迹、监督／能力保持，并补充必要基础工作 |

本轮以 2024—2026 年一手资料为重点，补充经典多任务方法；完成关键正文阅读与静态代码检查，尚未运行论文复现或验证当前业务模型。

## 从这里开始

**当前共同精读：DiFT → MFTCoder → DMO → Agent-FLAN → DFT。** 用户于 **2026-10-30** 确认并开始第一篇。[五篇名单与取舍](reading/top-5-v2-proposal.md)保留选入理由、指标范围和局限。

**正在学习 DiFT：[第一轮：问题、参数选择与原始证据](reading/01-dift.md) · [第二轮：效果与消融的证据强度](reading/01-dift-evidence.md)。** 第二轮重算六组混训结果，区分平均改善、双目标改善及达到单训；保留官方补充源码的静态核查，尚未运行论文复现。早期名单及年代偏差复核保存在[第一版记录](reading/top-5.md)，完整资料池继续作为查阅背景。

| 入口 | 主要内容 |
| --- | --- |
| [五篇共同精读](reading/top-5-v2-proposal.md) | 已确认名单、筛选理由与指标范围 |
| [第一篇：DiFT 精读](reading/01-dift.md) | 参数行选择的教学例子、原表比较及官方源码核查 |
| [DiFT 第二轮：效果与消融](reading/01-dift-evidence.md) | 六组混训重算、随机对照、位置选择与方向冲突的区别 |
| [调研总图](topics/00-research-map.md) | 大问题、已有成果、分歧、研究空间与建议阅读顺序 |
| [多任务 SFT 与混合](topics/01-multitask-sft-and-mixtures.md) | DiFT、DMO、CoBa、mSFT、DynamixSFT 等 |
| [Agent 轨迹与保持](topics/02-agent-trajectories-and-retention.md) | Agent-FLAN、AgentBank、SWE-Gym、SWE-smith 等 |
| [监督与能力保持](topics/03-supervision-and-capability-preservation.md) | Nemotron、Qwen3、DFT、PSFT、iw-SFT、LoTA 等 |
| [全部论文笔记与初筛目录](papers/README.md) | 发表身份、阅读深度、代码范围及逐篇分析 |
| [代码与资源地图](resources/repository-catalog.md) | 37 个主要项目，区分训练实现、数据／模型、项目站点和第三方实现 |
| [训练实现细节](resources/training-infrastructure.md) | mask、采样、loss 汇聚及关键源码入口 |
| [检索与核查说明](resources/search-and-verification.md) | 来源标准、版本、统计口径和未核实项 |

## 这一轮形成的认识

**样本配比没有完整定义训练目标。** 数据抽取、实际监督 token、conversation／任务级归一化和参数更新共同定义“混合”。Nemotron 与 MFTCoder 提供了相邻工业和代码证据；这仍不是当前双降的原因认定。[总图 §2](topics/00-research-map.md)

**已有文献的比较对象必须与当前问题对齐。** “目标任务改善、域外任务下降”“混训低于单训”“两个训练目标共同低于 base”是不同关系。很多方法覆盖其中一部分，平均收益也不保证两个业务目标同时恢复。[问题定义](problem.md)、[Agent 专题](topics/02-agent-trajectories-and-retention.md)

**目前有明确的相关研究基础，也有尚未闭合的证据。** 当前已读资料不足以确认本业务的机制，亦不足以宣称新颖性。研究空间围绕监督单位、联合适配与保持、机制边界展开，具体见[总图 §4](topics/00-research-map.md)。

## 整理约定

- 关键技术结论链接论文原文、正式论文集、作者代码或官方文档，并尽量定位章节／表格。
- 作者结论、用户观察和本仓库分析分开表述；保留反例与适用边界。
- 主会、Findings、workshop、预印本、作者接收声明分别记录。
- 未核实不等于不存在；有仓库不等于有训练实现，有源码不等于已复现。
- Stars、维护状态和版本注明核查日，不作为质量排名。
- 更新资料时同步笔记、索引和机器可读目录；同一论文不同版本不重复计数。

[专题索引](topics/README.md) · [资源索引](resources/README.md) · [资料总目录 JSON](resources/paper-catalog.json) · [仓库元数据 JSON](resources/repository-metadata.json) · [论文模板](templates/paper.md) · [专题模板](templates/topic.md)
