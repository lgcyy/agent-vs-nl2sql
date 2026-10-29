# A07 · Task Mixture Effects：小规模指令混合的统计分析

- **论文**：Empirical Analysis of Task Mixture Effects in Small-scale Instruction Tuning: A Statistical Approach，Jung & Jung。
- **发表**：Findings of ACL 2026，2026-07；[正式论文页](https://aclanthology.org/2026.findings-acl.643/)。
- **优先级 / 范围**：P1；直接研究指令混合，但限于小规模、直接作答数据。
- **核查**：2026-10-29；已读设置、主结果和 Limitations；未复现。

## 原文证据

研究自动提取的五类任务与 51 种混合，使用统计模型描述组成效应。§4 / Table 2 展示任务组合的收益或损害具有领域及模型依赖性。Limitations 明确限定直接作答数据、7–8B 模型与 LLM judge 评测，不能假定结论直接迁移到 CoT 等推理密集设置。[全文](https://aclanthology.org/2026.findings-acl.643.pdf)

## 局限与本案例关系（调研判断）

适合作为“任务交互必须明确条件和不确定性”的写作参照。它与 NL2SQL-noCoT 只有监督形式上的部分接近；其小规模数据和主观评价不能代替 SQL 执行或环境交互结果。尤其不能直接搬用论文中最佳任务比例，也不能把其相互作用系数视作当前任务对的性质。

## 代码核查

检查正式论文及 GitHub 名称检索后，**作者仓库未核实**；不把 PDF 中引用的 Stanford Alpaca 仓库当作本论文实现。Stars：未核实。

