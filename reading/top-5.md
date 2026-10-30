# 五篇共同精读：从资料池收敛到学习主线

初选：**2026-10-29**；年代偏差复核：**2026-10-30**。初选综合问题匹配、正式发表、引用积累和作者项目关注度，从已有 51 项资料中收敛为以下 **5 篇**。这是一版偏基础学习与成熟度的组合，不代表当前问题的前沿相关性前五。

[返回首页](../README.md) · [完整资料目录](../papers/README.md) · [筛选证据 JSON](../resources/top5-selection-evidence.json)

## 0. 年代偏差复核：为何四篇来自 2024 年？

**主要是筛选取舍偏向成熟度和入门覆盖，不能解释为 2025—2026 年的研究匹配度不足。** 初选已经找到 DiFT、DMO、PSFT 等近期正式会议论文，但在收敛时，引用是否查全、项目关注度以及学习路径的互补性实际获得了较大权重。把这些条件当成阅读价值的替代，会压低近期直接相关工作的优先级。

- 累计引用和 Stars 的积累时间不同；跨版本引用未核全也不能成为论文价值较低的证据。初选虽然记录了这些限制，实际取舍仍未充分消除偏差。
- AgentTuning 与 Agent-FLAN 分别承担混训背景和监督结构的入门作用；同时占据两个名额，减少了近期方法进入主读的空间。
- 顶会身份不能解释近期论文落选：DiFT、PSFT 已正式发表于 ICLR 2026，DMO 已正式发表于 ICML 2025。

| 近期工作 | 对当前问题的实际价值 | 复核判断与边界 |
| --- | --- | --- |
| [DiFT，ICLR 2026](../papers/a-01-dift.md) | 直接比较单训、混训、共同起点；Table 1 的一个组合在数值上出现双侧同时低于单训与 base | 应优先精读其问题与证据；作者实现及引用未核全，不应直接阻止入选。该表有箭头与数值不一致，部分降幅很小，且任务不是 SQL＋Agent。 |
| [DMO，ICML 2025](../papers/a-02-data-mixing-optimization.md) | 直接优化多域 SFT 混合权重，有实质训练与权重求解实现 | 低 Stars 不足以否定它作为研究阅读材料的价值；平均验证损失目标仍不等于两个业务任务同时恢复。 |
| [PSFT，ICLR 2026](../papers/c-02-psft.md) | 通过限制更新讨论目标学习与原有能力保持 | 值得作为近期能力保持方法；正式全文 Appendix A 将混合数据有效性留作未来工作，因此直接匹配弱于 DiFT／DMO。 |

筛选优先关系应更明确：**问题与比较对象匹配 → 结果证据及局限 → 正式发表与实质实现 → 同等公开时长下的引用／关注度**。引用和 Stars 保留为辅助信号，不设置排除近期相关工作的硬门槛，也不为了年份机械凑名额。

论文的首次公开日期与正式会议年份应并列理解。例如 MFTCoder 于 [2023-11-04 首次公开](https://arxiv.org/abs/2311.02303)，DMT 于 [2023-10-09 首次公开](https://arxiv.org/abs/2310.05492)；已入选的 DFT 于 [2025-08-07 首次公开](https://arxiv.org/abs/2508.05629)，正式发表于 ICLR 2026。因此“没有标成 2025 的入选论文”不等于没有 2025 年提出的工作。

本次针对用户的筛选理由追问补充复核，保留下面的第一版名单及当时取舍。研究问题的下一步对齐应优先回到 **DiFT 的原始比较证据**；如果保持五篇且把当前问题匹配放在首位，优先考虑 **DiFT 替换 AgentTuning**，由 Agent-FLAN 保留 Agent 监督结构视角。本轮记录这项修正建议，不无声改写原名单。详见[补充证据](../resources/top5-selection-evidence.json)中的 `recency_audit`。

## 1. 第一版名单

| 阅读顺序 | 论文 | 正式发表 | OpenAlex 收录引用数 | 作者项目 Stars／实际公开范围 |
| --- | --- | --- | --- | --- |
| 1 | [MFTCoder](../papers/r-01-mftcoder.md) | [KDD 2024](https://doi.org/10.1145/3637528.3671609) | [23](https://api.openalex.org/works/W4401863706)；正式与预印本合并条目 | [711](https://github.com/codefuse-ai/MFTCoder)；已见核心 loss 与 trainer |
| 2 | [SFT Data Composition / DMT](../papers/a-04-dmt-data-composition.md) | [ACL 2024 主会长文](https://aclanthology.org/2024.acl-long.12/) | [55](https://api.openalex.org/works/W4402667025)；正式条目 | 作者训练仓库未核实；作为问题与证据论文 |
| 3 | [AgentTuning](../papers/b-01-agenttuning.md) | [Findings of ACL 2024](https://aclanthology.org/2024.findings-acl.181/) | [40](https://api.openalex.org/works/W4402670718)；正式与预印本合并条目 | [1,509](https://github.com/THUDM/AgentTuning)；部署／评测／模型／数据，主混训入口未核实 |
| 4 | [Agent-FLAN](../papers/b-02-agent-flan.md) | [Findings of ACL 2024](https://aclanthology.org/2024.findings-acl.557/) | [13](https://api.openalex.org/works/W4402683785)；正式与预印本合并条目 | [363](https://github.com/InternLM/Agent-FLAN)；数据／模型发布，未见主训练源码 |
| 5 | [DFT：Dynamic Fine-Tuning](../papers/c-01-dft.md) | [ICLR 2026 主会](https://proceedings.iclr.cc/paper_files/paper/2026/hash/2e53c02ea028cbf603f4b6b47fef3d97-Abstract-Conference.html) | **跨版本引用未核全** | [928](https://github.com/yongliang-wu/DFT)；已见核心 trainer／loss |

引用和 Stars 均查询于 **2026-10-29**。这五篇包括 **3 篇主会论文、2 篇 Findings**，不把 Findings 写成 ACL 主会。引用列是 OpenAlex 收录结果，不是 Google Scholar 数字或全部索引的真实总引用。

DMT 还有一个单独的[预印本条目，计数为 5](https://api.openalex.org/works/W4387560076)；未核对施引文献重叠，不能把 55 与 5 相加。DFT 当前解析到的[预印本条目](https://api.openalex.org/works/W6929518238)为 0，但尚未核全正式版本，故不将其写成整篇论文零引用。不能以这类不完整计数否定新论文，也不能把它包装成高被引工作。

## 2. 筛选标准与实际取舍

**问题匹配是首要条件。** 优先研究多任务 SFT、Agent 监督和能力保持；预训练混合、RL 或运行平台即使受到较多关注，也不直接占据这轮主读名额。

**正式发表、引用和代码分开判断。** 会议身份由原始 proceedings 核实；引用采用同一书目索引并保留版本范围；代码除了看 Stars，还看是否有目标函数、数据处理和训练入口。较早论文具有更长引用积累期，不能与新论文只比总数。

**五篇需要互补。** 本轮保留“多任务实现 → 混合现象 → Agent 混训对照 → 监督结构 → 新 SFT 目标”的学习路径。同一工程的相关论文只选一篇主读，其他材料仍保存在原资料池。

在这些条件下，本轮没有得到“五篇都高星、都高被引、都直接匹配且都完整开源”的集合。最终保留一篇代码未核实但直接问题证据较强的 DMT，以及一篇引用覆盖未核全但已有实质实现的近期 DFT。公开范围在主表直接标出。

## 3. 每篇只带走一个核心问题

### 第一篇：MFTCoder——共同训练到底如何定义各任务的贡献？

原文区分单任务 SFT、简单合并数据的 SFT-Mixed 和显式多任务方法；讨论数据不均衡、任务难度和收敛差异。对应 §3、§4.1。[所读预印本](https://arxiv.org/pdf/2311.02303)

**选择理由：**它同时具备直接多任务问题、KDD 正式发表、可核查引用和实际 loss／trainer，适合先把训练概念与源码对应起来。它的代码任务不能等同当前 SQL-noCoT 加长程交互。

### 第二篇：Data Composition / DMT——数据共同出现，何时协同，何时产生冲突？

论文研究数学、代码和通用对话的组成，比较数据量、模型规模与不同训练顺序；§3、Table 1 和 §4.2 是阅读重点。[正式论文](https://aclanthology.org/2024.acl-long.12.pdf)

**选择理由：**直接讨论 SFT 的多能力组合，正式版的引用证据也较明确。学习重点是作者怎样把混训现象拆成可讨论的问题，而不是把其双阶段配方直接套到当前业务。

### 第三篇：AgentTuning——Agent 轨迹与通用指令怎样一起训练？

阅读 §2.2 的混合方式和 §3.4 的 agent-only、general-only 与混训对照。[正式论文](https://aclanthology.org/2024.findings-acl.181.pdf)

**选择理由：**已有可核查引用和相对较高项目关注度，也提供了 Agent 与一般能力兼顾的具体实例。其域外能力保留与“两个参与训练目标同时低于共同起点”仍是不同问题。

### 第四篇：Agent-FLAN——一条 Agent 轨迹里究竟监督了哪些能力？

阅读 §3–4 中格式、推理、工具选择与参数理解的拆分，再看 §5.2 的通用表现对照。[正式论文](https://aclanthology.org/2024.findings-acl.557.pdf)

**选择理由：**它能帮助我们进一步描述 SQL 结果监督与 Agent 交互监督的差异。363 stars 和缺少主训练源码是实际限制；其监督结构视角仍值得保留。不能由格式差异直接认定当前双降机制。

### 第五篇：DFT——对示范 token 的更新权重是否都应相同？

阅读 §3.3 的目标函数及代码中的停止梯度，再一起读 §4.5 的事实知识负例。[所读 v3](https://arxiv.org/html/2508.05629v3)

**选择理由：**保留近期 ICLR 方法，且可以对应实际 trainer。它提供目标函数视角，但不直接验证 SQL＋Agent 混训。DFT 是 Dynamic Fine-Tuning，与 DiFT（Differential Fine-Tuning）不同。

## 4. 第一版未选其他论文的原因（复核前取舍）

下表记录 2026-10-29 初选理由。DiFT 与 DMO 的降级理由已在上方年代复核中修正，不应继续只因引用未核全或低 Stars 排后。

| 工作 | 本轮取舍 |
| --- | --- |
| CoBa | 与 MFTCoder 共用工程。前者更聚焦动态收敛，后者适合作为基础入口且当前合并条目的引用证据更强。本轮只占一个主读名额，不代表两篇方法等同。 |
| DiFT | 问题匹配很强且已正式发表于 ICLR 2026，但作者代码和统一引用计数未核实。本次更重视影响力及可核查性，由 DMT 承担直接问题证据席位；不据此判定 DiFT 质量低。 |
| DMO、PSFT | 正式发表和实质代码已核实，但项目关注度及引用版本覆盖弱于本次保留对象，留在资料池。 |
| Nemotron 3 Super、SWE-Gym | 前者是工业技术报告，后者更侧重软件轨迹与验证器；保留工程参考价值，不扩大本轮五篇主读范围。 |

这些取舍依据已有卡片和本次计数快照，详见[筛选证据](../resources/top5-selection-evidence.json)。后续阅读若发现某篇对问题帮助有限，可在同样标准下替换，不把此名单当成固定的学术排行榜。

## 5. 第一次共同阅读：先理解 MFTCoder 的比较对象

暂时只抓住一个问题：**共同使用两类数据，是否已经完整定义了多任务学习目标？**

原文 §4.1 的三个比较对象分别是各任务单训、将数据合并的 SFT-Mixed，以及显式多任务方法。阅读这些对照时，先辨认它们改变了什么，再讨论某个分数是否提高。

用一个一般表达帮助理解：

$$
L(\theta)=\lambda_{\rm SQL}L_{\rm SQL}(\theta)+
\lambda_{\rm Agent}L_{\rm Agent}(\theta).
$$

**这是本仓库的说明，不是对用户训练配置的还原。** 其中每个任务损失是否先按有效 token 或样本归一化，任务权重如何得到，都会影响这条式子的具体含义。仅知道样本比例，还不足以给出完整目标。

当前 MFTCoder 的[核心实现](https://github.com/codefuse-ai/MFTCoder/blob/8ba13f44e2036f1412a343bc343dc6cda7383d73/mftcoder_accelerate/src/utils/loss_utils.py)提供 token、样本和任务级汇聚分支；这些分支的准确批次范围见[R01 笔记](../papers/r-01-mftcoder.md)。仓库还包含后续 CoBa，读取当前代码时需与原始论文贡献分开。

第一遍阅读依次回答：

1. 作者为什么认为数据不均衡、任务难度、收敛速度是不同问题？
2. 各对照的任务、起点、数据和汇聚方式分别是什么？
3. 主结果究竟支持平均改善、逐任务改善，还是保持起点表现？
4. 哪些证据属于代码任务，迁移到长程交互时还缺少什么？

本轮先完成文献学习与概念对齐，不把论文配方提前认定为业务解法。
