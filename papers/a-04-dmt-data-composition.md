# A04 · DMT 与 SFT 数据组成：任务、数据量和训练顺序

- **论文**：How Abilities in Large Language Models are Affected by Supervised Fine-tuning Data Composition，Dong et al.
- **发表**：ACL 2024 主会长文，2024-08；[正式论文页](https://aclanthology.org/2024.acl-long.12/)。[预印本](https://arxiv.org/abs/2310.05492)首发 2023-10-09。
- **优先级 / 范围**：P0；多能力 SFT 的较早直接证据。
- **核查**：2026-10-29；已读研究问题、Table 1、Figure 6 及相关讨论，未复现。

## 原文证据

研究数学、代码与一般对话的数据组成。§3 显示混合效应随数据量而变；Table 1 比较混合、顺序与双阶段训练。DMT 先学习专门任务，再在一般数据中保留少量专门数据。§4.2 / Figure 6 去除 ShareGPT 内数学与代码样本后，部分高资源冲突减弱，提示数据源内部组成会影响解释。[全文](https://aclanthology.org/2024.acl-long.12.pdf)

## 局限与本案例关系（调研判断）

可据此明确：“数据集名称”不等于纯净任务域，按外部标签划分的两类数据可能仍存在内部重叠或输出分布差异。本文不是长程 agent 研究；其中一般对话与专门任务的划分，也不能直接映射为 SQL 与 agent。其阶段顺序和小比例数值只属于原文设置，本仓库不把它们转成当前训练方案。

## 代码核查

已检查正式论文及相关检索，**本论文作者实现仓库未核实**。全文列出的 CodeAlpaca、GSM8K-ScRel、评测 harness 是数据或评测依赖，不能因此声称 DMT 官方训练实现已开源。Stars：未核实。

