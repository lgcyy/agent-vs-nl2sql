# iw-SFT：Supervised Fine Tuning on Curated Data is Reinforcement Learning

**完整标题**：Supervised Fine Tuning on Curated Data is Reinforcement Learning (and can be improved)  
**作者**：Chongli Qin、Jost Tobias Springenberg。**状态**：arXiv 预印本；未核实正式会议录用。首发 2025-07-17，本卡核读 v2（2025-09-06）。**核查日**：2026-10-29。  
**定位**：精选示范数据、参考策略与 importance weighting。

## 问题、方法与原文证据

论文在筛选正例与分布支持等假设下，把 SFT 联系到稀疏奖励目标的下界；iw-SFT 通过参考／中间策略的重要性权重收紧该联系（§3.2–3.3、Appendix A）。这与 DFT 只使用当前目标 token 概率不同。§4、Table 1 包含 Qwen2.5-32B-Instruct 在 s1K 上的推理结果；表内部分对照引用 s1 既有结果，不能当作所有方法均同预算重跑。

## 局限及与当前问题的关系

**本仓库推断**：该工作帮助区分“示范被挑选过”与“示范来自当前策略”两件事。对于由不同教师、不同执行成功标准筛选的 SQL 与 Agent 数据，参考分布的语义未必相同；统一 importance weighting 可能改变有效任务比重。理论中的正例筛选假设也不自动等价于业务执行正确。

论文没有验证当前异构混训双降。保留 base 能力、拟合新 schema，以及拟合一条冗长但仅部分关键的轨迹，可能需要不同的学习信号；本卡不据单一数学推理结果推定通用收益。

## 代码核验

论文 Appendix H 直接链接 [LLM 仓库](https://github.com/emmyqin/iw_sft)与独立的 [control 仓库](https://github.com/emmyqin/iw_sft_control)。已读前者 README 和 [bounding_trainers.py](https://github.com/emmyqin/iw_sft/blob/main/bounding_trainers.py)，可见 `IwSFTLoss`、参考 log-prob、停止梯度的权重以及 token／时间聚合选项。**已见核心实现，未运行复现**。核读 blob：`814cb538e882b19bdab5845b8d95a18a12ad4cbc`。LLM 仓库 API 快照：**28 stars**，2026-10-29。

## 一手来源

- [arXiv 记录](https://arxiv.org/abs/2507.12856)；[所读 v2 全文](https://arxiv.org/html/2507.12856v2)
- [LLM 仓库 README](https://github.com/emmyqin/iw_sft/blob/main/README.md)

