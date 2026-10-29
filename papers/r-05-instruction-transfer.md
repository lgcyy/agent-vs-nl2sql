# R05 · 指令微调后的迁移能力与当前任务表现

- **论文：**Multi-Task Transfer Matters During Instruction-Tuning，**Findings ACL 2024**。[正式论文页](https://aclanthology.org/2024.findings-acl.883/)
- **作者仓库：**[davidandym/Multitask-Transfer-Instruction-Tuning](https://github.com/davidandym/Multitask-Transfer-Instruction-Tuning)，原文脚注给出链接；1 star（2026-10-29 API）。
- **阅读深度：**正式摘要、原文入口与代码目录；正文尚待精读。**P2**。

## 已核实内容

作者研究指令微调之后，对未见任务继续微调的迁移表现及其与 in-context learning 的关系，报告训练任务优化与后续泛化之间的联系。[原文](https://aclanthology.org/2024.findings-acl.883.pdf)

## 代码核查

当前默认分支递归目录仅见 README 与 LICENSE；作者链接存在，但未见实质训练代码。不得将它标成“代码完整、可复现”。

## 与本问题的关系

**我们的判断：**这篇工作的价值是提醒我们分别记录“参与训练任务的表现”“未参与训练能力的保持”和“将来适应新任务的能力”。本项目确认的是两个参与训练目标同时回退；该文的迁移结果不能直接替代这组证据。

核查日期：2026-10-29。
