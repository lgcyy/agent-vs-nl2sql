# PG-SFT：Agent 能力获取与保留的近期线索（摘要级）

- 编号：B09
- 完整题名：PG-SFT: Balancing Capability Acquisition and Retention in Offline Agent Fine-Tuning。
- 作者与年份：Ronghua Li、Zi Liang、Zhishan Li、Shinan Liu，2026。
- 发表状态：[arXiv:2610.00949](https://arxiv.org/abs/2610.00949)，首次提交 2026-10-01 UTC；作者实验室列为 Preprint / In Submission，未核实正式录用。
- 阅读深度：**仅原始摘要与作者发表列表**；PDF/HTML 抓取失败，本轮重试 HTML 仍失败。未采信聚合站的全文分析。
- 关联专题：[Agent 轨迹与能力保持](../topics/02-agent-trajectories-and-retention.md)
- 核查日期：2026-10-29

## 摘要可支持什么

作者研究离线 Agent 轨迹 SFT 中的新能力获取与已有能力保持。摘要描述标准 SFT 提高目标基准、降低若干非目标基准；提出按轮次信息增益调整监督强度，并报告在所测任务中改善获取—保留权衡。[原始摘要](https://arxiv.org/abs/2610.00949)

以上仅是作者摘要报告。本轮不填写模型规模、数据集、具体数值、统计显著性、实现开销或消融结果，因为这些需要全文证据。

## 与混训双降的关系

它与业务问题共享“Agent SFT 后能力退化”的主题，但摘要中的关系是一项训练目标改善、外部能力下降。当前问题则是两项都参与训练的目标，在同一混训 checkpoint 上均低于各自单训及共同 base；不能将两者归为相同现象。

不能根据本文标题或摘要宣称其已解决本仓库问题，也不能根据本轮未读到某项实验断言全文一定没有该实验。其角色是高相关、待精读线索。

## 代码与核查缺口

- 作者来源：[NAISS Lab](https://naisslab.github.io/)。
- 尚未从可核查的作者来源找到对应官方代码仓库，repo_url 为空；“未找到”不等于“未发布”。
- 不纳入“论文与完整训练代码均已核实”的集合。原始发布日期以 arXiv 的 10 月 1 日为准，不沿用聚合站的 9 月 30 日。

## 来源

1. [arXiv 原始摘要与元数据](https://arxiv.org/abs/2610.00949)
2. [作者实验室发表列表](https://naisslab.github.io/)

