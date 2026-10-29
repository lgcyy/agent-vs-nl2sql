# 01 · 多任务 SFT、数据混合与训练组织

核查日期：**2026-10-29**。本专题记录文献证据与边界，不构成当前业务模型的训练方案。完整条目、阅读深度和仓库元数据见 [A 线来源登记](../resources/a-source-register.json)。

## 1. 大问题：异质监督下，为什么联合学习没有保持两侧能力？

用户已确认的现象是：从共同 SFT 起点 Qwen-3.8-27B（名称按用户提供）出发，NL2SQL-noCoT 与类似 SkillsBench 的长程 agent 轨迹分别单训表现可接受；同一个混训 checkpoint 在两侧均低于各自单训，并且均低于共同起点。单训可接受不等于已确认单训均胜 base；现有观察也不包含预算一致、统计显著性或跨配置稳定性的保证。

这里有两个比较关系：相对对应单训模型的双侧下降，以及相对共同起点的双侧回退。文献只回答其中一层，或只改善平均分时，不能据此宣布已覆盖整个问题。noCoT 仅表示无显式 CoT 监督，长程 agent 轨迹也不必然含显式 CoT；本专题不把两者简化成“无推理与有推理”的对立。

本轮围绕以下问题阅读：**已有研究如何描述多任务 SFT 中的交互、如何组织混合监督，以及这些证据还不能解释什么？**

## 2. 已有成果与方法：文献实际上在处理不同对象

| 研究对象 | 已有工作 | 已核实的内容及定位 |
|---|---|---|
| 任务相关参数与共享更新 | [DiFT，ICLR 2026](../papers/a-01-dift.md) | 参数行差异与多推理任务干扰；[§3–5 / Tables 1–2](https://proceedings.iclr.cc/paper_files/paper/2026/file/b304a04c6a4ac168aa692923814ace17-Paper-Conference.pdf)。 |
| 静态混合权重的目标函数 | [DMO，ICML 2025](../papers/a-02-data-mixing-optimization.md) | 显式建立多域 SFT 损失与数据量关系；[§4–5](https://arxiv.org/pdf/2508.11953)。 |
| 训练状态变化时的采样 | [DynamixSFT，Findings ACL 2026](../papers/a-03-dynamixsft.md) | 用数据集级前瞻信号调整混合；[§4–7](https://aclanthology.org/2026.findings-acl.1972.pdf)。 |
| 数据组成与阶段顺序 | [DMT / Data Composition，ACL 2024](../papers/a-04-dmt-data-composition.md) | 分开讨论数据量、组合与顺序；[§3–4](https://aclanthology.org/2024.acl-long.12.pdf)。 |
| 多任务到达良好状态的不同速度 | [CoBa，EMNLP 2024](../papers/a-05-coba.md)；[mSFT，2026](../papers/a-06-msft.md) | 分别关注收敛平衡与异步过拟合；[CoBa §3 / §7](https://aclanthology.org/2024.emnlp-main.459.pdf)、[mSFT §2–4](https://arxiv.org/pdf/2603.21606v7)。 |
| 任务组合效应的条件性 | [Task Mixture Effects，Findings ACL 2026](../papers/a-07-task-mixture-effects.md) | 小规模直接作答指令的统计分析；[§4 / Limitations](https://aclanthology.org/2026.findings-acl.643.pdf)。 |
| 预训练域对目标损失的贡献 | [DoGE，ICML 2024](../papers/a-08-doge.md) | 代理模型估计域权重；[§2–4](https://arxiv.org/html/2310.15393v2)。 |
| 通过代理运行预测混合结果 | [RegMix，ICLR 2025](../papers/a-09-regmix.md)；[Olmix，ICML 2026](../papers/a-10-olmix.md) | 离线回归搜索与域更新后的比例复用；[RegMix §3–4](https://arxiv.org/pdf/2407.01492)、[Olmix §3–6](https://arxiv.org/pdf/2602.12237)。两者属于预训练参照。 |

上述分类是本仓库的整理方式。不同论文的训练数据、模型起点、目标和汇总指标不同，因此“方法 A 比 B 好”的原文结论只能连同设置一起读取。它们不构成对当前业务模型的直接算法排名。

## 3. 局限与未解：哪些推断目前跨不过去？

### 3.1 平均收益不等于每一域都保持

DMO Table 3 和 DoGE Table 1 都能看到平均目标较优与部分单域取舍同时存在。[DMO 主表](https://arxiv.org/pdf/2508.11953)、[DoGE 主表](https://arxiv.org/html/2310.15393v2)

本案例要求理解同一 checkpoint 的两侧表现；“总体更好”“十个 benchmark 平均更高”“验证 PPL 更低”与这一目标不等价。调研记录应保留原始比较对象，避免把平均收益改写成逐域恢复。

### 3.2 收敛不一致、参数干扰与格式变化是不同层面的解释

CoBa 自身已声明收敛平衡不保证冲突任务都达到最优；DiFT 附录 B.6 的格式问题来自特定连续训练个案。[CoBa §7](https://aclanthology.org/2024.emnlp-main.459.pdf)、[DiFT 附录 B.6](https://proceedings.iclr.cc/paper_files/paper/2026/file/b304a04c6a4ac168aa692923814ace17-Paper-Conference.pdf)

因此，现阶段不能仅凭双侧分数下降就确定哪一种解释成立，也不能把连续训练中的遗忘现象直接搬到同时混训。不同解释可能并存，但这种可能性仍是推断。

### 3.3 预训练的成功不能自动迁移到 SFT

DoGE、RegMix、Olmix 的正文研究对象主要是预训练数据域和代理模型；当前问题的起点是已有能力的 checkpoint，且两类监督在任务和序列结构上不同。[DoGE 设置](https://arxiv.org/html/2310.15393v2)、[RegMix 设置](https://arxiv.org/pdf/2407.01492)、[Olmix 设置](https://arxiv.org/pdf/2602.12237)

两类研究可以共享“目标域”“比例”“数据约束”等概念，但现有证据不能保证比例排序、代理估计或最优预算在训练阶段之间保持不变。

### 3.4 当前文献覆盖尚未闭合到具体业务现象

在本轮 16 项筛选中，未找到同时覆盖“NL2SQL-noCoT + 长程交互轨迹 + 同一起点 + 同一混训 checkpoint 双侧低于对应单训且低于起点”，并解释其原因的直接证据。这是**本轮覆盖结论**，不是宣称此前无人研究，更不是新颖性认定。

Task Mixture Effects 尤其明确将结论限定于直接作答与较小模型，其作者也未把结论推广到推理密集监督。[原文 Limitations](https://aclanthology.org/2026.findings-acl.643.pdf)

## 4. 对本案例的启发（以下均为调研推断）

1. **研究问题应保留双重参照。** 相对单训的下降描述联合学习未能保留什么；相对共同起点的回退描述原有能力是否受损。两者可放在同一个问题下，但应保持可区分。
2. **“异质监督”是待解释对象，不是已经找到的原因。** 当前两类数据同时在任务、长度和交互结构上不同。仅把这些差异重新命名，不能解释为何两侧一起下降。
3. **文献可帮助组织竞争性解释。** 目标与权重、学习动态、参数共享、输出协议分别提供观察现象的不同视角；当前材料还不能把任何一个视角提升为本案例机制。
4. **潜在贡献需要比业务修复多一层边界说明。** 业务恢复说明具体系统有用；可推广贡献还需说明结论在什么对象、条件和评价口径下成立。论文主张应在文献覆盖补齐后形成，不能从“当前未查到完全相同任务对”直接推出新颖性。
5. **长期维护应保留数据与文献版本。** 这是从各论文不同设置及 Olmix 的域更新问题得到的整理建议。记录同一个方法名对应的版本、数据域定义、代码状态，可以防止后续讨论在不知情时替换研究对象。

## 5. 继续保留的初筛条目

以下已核对主来源，但未形成与前十篇同等深度的正文笔记。它们已登记在 JSON，不能计作全文精读。

| 条目 | 发表身份 | 保留原因与阅读边界 |
|---|---|---|
| [DoReMi](https://proceedings.neurips.cc/paper_files/paper/2023/hash/dcba6be91359358c2355cd920da3fcbd-Abstract-Conference.html) | NeurIPS 2023 主会 | 域重加权的经典入口；预训练边界，已核对官方摘要与作者 README。 |
| [Skill-It](https://proceedings.neurips.cc/paper_files/paper/2023/hash/70b8505ac79e3e131756f793cd80eb8d-Abstract-Conference.html) | NeurIPS 2023 主会 | 技能依赖与数据顺序；已读摘要及 README。其 skills 不应自动等同本案例的 SkillsBench。 |
| [Data Mixing Laws](https://proceedings.iclr.cc/paper_files/paper/2025/hash/cc84bfabe6389d8883fc2071c848f62a-Abstract-Conference.html) | ICLR 2025 | 预测混合损失的相关路线；正文和核心代码未深读。 |
| [AutoScale](https://openreview.net/pdf?id=rujwIvjooA) | COLM 2025，论文页头与作者仓库声明 | 尺度依赖边界；正式论坛读取受浏览验证页阻挡，已有公开论文搜索内容及作者 README。 |
| [Demystifying Instruction Mixing](https://aclanthology.org/2024.acl-srw.15/) | ACL 2024 **Student Research Workshop** | 直接研究指令类型组合；不能标成 ACL 主会。仅摘要级。 |
| [Rethinking Data Mixing from the Perspective of Large Language Models](https://aclanthology.org/2026.acl-short.28/) | ACL 2026 主会短文 | 讨论域定义与训练动态；仅官方摘要级，不能当作已验证的 SFT 机制。 |

## 6. 代码与证据覆盖

本轮共筛选 **16 项**，形成 **10 篇主笔记**。其中 9 篇已由正式 proceedings 核实会议身份；mSFT 保留 arXiv v7 的 AACL 2026 Oral 作者声明，尚未核实正式论文集。

十篇主笔记中，DMO、CoBa、mSFT、DoGE、RegMix、Olmix 已核实关联代码并检查目录或核心文件；其余四篇作者仓库未核实。另有四项初筛工作已核对作者 README。GitHub stars 只作核查时点的元数据，不用于替代相关性或复现成熟度判断。

已有代码也有不同边界：mSFT 仓库推送日期早于当前论文版本；RegMix 主 README 保留 evaluation 的 WIP 标签，但实际已有评测入口；Olmix 明示开源迁移中的 WIP；Skill-It README 明示技能图学习代码未提供。[mSFT](https://github.com/reiss-koh/msft)、[RegMix](https://github.com/sail-sg/regmix)、[Olmix](https://github.com/allenai/olmix)、[Skill-It](https://github.com/HazyResearch/skill-it)

本轮完成的是来源核查、关键正文阅读和静态代码检查，未运行训练、复现论文结果或建立统一性能排行榜。

