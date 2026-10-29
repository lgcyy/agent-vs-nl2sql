# A03 · DynamixSFT：训练过程中的动态数据集混合

- **论文**：DynamixSFT: Dynamic Mixture Optimization of Instruction Tuning Collections，Shin et al.
- **发表**：**Findings of ACL 2026**，2026-07；[正式论文页](https://aclanthology.org/2026.findings-acl.1972/)。首个预印本为 2025-08-16，不能只登记为 2025 预印本。
- **优先级 / 范围**：P0；指令微调数据集级动态采样。
- **核查**：2026-10-29；已读方法、主表、消融与限制；未复现。

## 原文证据

方法将数据集视作 bandit 的臂，以一步前瞻奖励更新采样概率，并用原始比例作为软锚点。§6 / Table 2 报告 TÜLU-2/3 混合上的十项评测平均收益；§7 / Table 3 检验比例先验。Limitations 明确其权重作用于整个数据集，而非逐样本，也未细分研究各具体领域机制。[全文](https://aclanthology.org/2026.findings-acl.1972.pdf)

## 局限与本案例关系（调研判断）

适合作为“数据价值随训练状态变化”的来源。这里 bandit 是采样控制方法，模型学习阶段仍是指令 SFT，不能把它归成用户暂不优先的策略模型 RL 训练。本文优化多源混合的总体表现，不证明所有任务单项均恢复，也不直接覆盖 SQL 与长程 agent 的两侧基线约束。

## 代码核查

检查了[作者主页](https://haebinshin.github.io/)、正式论文及 GitHub 名称检索；作者主页的该条目仅提供 Paper，**公开作者仓库未核实**。不将检索到的作者其他项目或第三方基线实现当作本论文官方代码。Stars：未核实。

