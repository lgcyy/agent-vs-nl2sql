# A02 · DMO：面向 SFT 的数据混合优化

- **论文**：Data Mixing Optimization for Supervised Fine-Tuning of Large Language Models，Li, Liu & Xing。
- **发表**：ICML 2025，PMLR 267:35419–35437；[正式论文页](https://proceedings.mlr.press/v267/li25bh.html)。
- **优先级 / 范围**：P0；直接研究多域 SFT 的混合权重。
- **核查**：2026-10-29；已读损失建模、主要结果表与代码核心文件，未复现。

## 原文证据

方法用有效迁移数据量与微调缩放关系拟合各域验证损失，再求混合权重。§4 / Figure 2 讨论最优比例随数据预算变化；§5.2 / Table 3 比较原始比例、等 token、等条数与优化比例。**平均 PPL 改善并不意味着每个域均优于原始比例**，表内存在单域取舍；§5.3 另评下游任务。[全文](https://arxiv.org/pdf/2508.11953)

## 局限与本案例关系（调研判断）

本文比预训练混合论文更贴近当前阶段，也明确区分了条数与 token 的混合定义。但损失拟合依赖模型、数据域和已有测量；平均验证损失、SQL 执行正确率与长程任务成功率没有自动等价关系。它支持“比例必须连同计量单位和目标一起解释”的调研框架，不能据此确认当前双侧回退仅由比例造成。

## 代码核查

[realYuanLi/Data-Mixing-Optimization](https://github.com/realYuanLi/Data-Mixing-Optimization) 的 README 明确关联论文及作者。仓库包含数据预处理、混合采样、SFT 启动脚本、参数校准和权重求解，不是占位仓库。已检查 [weight_optimizer.py](https://github.com/realYuanLi/Data-Mixing-Optimization/blob/main/domain_weights/weight_optimizer.py)，有实际目标函数与 SLSQP 求解；训练封装依赖 LLaMA-Factory，路径及调度环境仍需适配。

GitHub API：**0 stars**，核查日 2026-10-29；这是查询值，不是缺失值。代码来源：[README](https://github.com/realYuanLi/Data-Mixing-Optimization/blob/main/README.md)、[训练入口](https://github.com/realYuanLi/Data-Mixing-Optimization/blob/main/commands/train_command/train_single.sh)。

