# 长程终端轨迹：监督长度与可靠性的非单调关系

- 编号：B10
- 完整题名：Selecting Long-Horizon Trajectories for Reliable and Efficient Terminal-Agent Training。
- 作者与年份：Cuong Dang、Hoang Anh Just、Ruoxi Jia，2026。
- 发表状态：[arXiv:2610.05831](https://arxiv.org/abs/2610.05831)，2026-10-05；作者注明 Submitted to ICLR 2027，**不表示已录用**。
- 阅读深度：原文 §3—6、表2、附录A；作者主页核查，代码未核实。
- 关联专题：[Agent 轨迹与能力保持](../topics/02-agent-trajectories-and-retention.md)
- 核查日期：2026-10-29

## 问题、做法与有证据的结论

本文固定 Qwen3-8B 与 Nemotron 终端轨迹，比较不同 token 监督长度；在 Terminus-2 接口下用 Terminal-Bench 的重复尝试衡量覆盖与稳定性。12K→16K 时，表2的至少成功一次任务数为 29→26，但部分重复成功指标不同向，不能概括为“16K 所有方面更差”。[原文 §3.3—4.2、表2](https://arxiv.org/pdf/2610.05831)

§6先学习短前缀，再选择长续段；作者把似然解释为相对已学策略的兼容性，并非内在正确性分数。附录A.3区分理论量与经验诊断量。[原文 §2、§6、附录A.3](https://arxiv.org/pdf/2610.05831)

## 局限与本仓库解读

这里改变的是同类终端轨迹的监督范围，不是两种任务数据的混合比例。不同长度也改变计算量，不能把所报收益当作所有训练预算约束下的结论。

与当前问题的关联是“长”包含 token 数、行动步数、错误恢复历史等不同含义；它没有证明长轨迹等同显式 CoT，也没有 SQL-noCoT 混训对照。其单类轨迹内部的非单调性，只能提供邻近证据。

## 代码与公开资产

- [第一作者主页](https://captaincuong.github.io/)确认该预印本并链接 arXiv。
- 本轮检查的 PDF 与作者主页没有给出对应 GitHub 代码入口，官方代码标为“尚未核实”。
- 论文使用现有训练库和公开语料，不等于已经发布本文的选择及训练实现；不能用通用训练框架的仓库填充其代码字段。

## 来源

1. [arXiv 状态与版本](https://arxiv.org/abs/2610.05831)
2. [原始 PDF：§3—6、表2、附录A](https://arxiv.org/pdf/2610.05831)
3. [作者主页](https://captaincuong.github.io/)
