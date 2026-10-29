# 检索与核查说明

**资料核查日：2026-10-29。** 这是围绕已对齐业务问题的一轮主题调研，不声称覆盖所有文献，也不把摘要检索数量当作全文阅读数量。范围以[问题定义](../problem.md)为准。

## 检索范围

以 2024—2026 年工作为重点，并保留必要的多任务优化基础文献。来源包括正式会议论文集、arXiv、作者或机构技术报告、作者关联 GitHub、官方框架文档与模型／数据发布页。技术结论以一手来源为依据，聚合站只用作发现入口。

检索按以下关键词族交叉展开，随后沿正式题名、作者、原文参考文献和论文代码链接核查。这里记录检索策略，不是逐条带时间戳的完整查询日志。

| 线索 | 关键词与重点 |
| --- | --- |
| 多任务 SFT | multi-task SFT、negative transfer、task interference、data composition、data mixing optimization |
| 学习动态 | heterogeneous overfitting、convergence balance、dynamic mixture、checkpoint |
| Agent 离线监督 | agent tuning、interaction trajectories、behavior cloning、generalization、retention |
| 监督与更新目标 | probability weighting、proximal SFT、importance weighting、loss normalization |
| 监督形式 | thinking/non-thinking fusion、long/short CoT、terminal trajectory horizon、assistant masking |
| 参数更新 | differential fine-tuning、half fine-tuning、sparse adaptation、LoRA forgetting |
| 代码追踪 | 论文／作者页 → 作者仓库 → README → 目录 → 相关损失、数据或训练入口 |

预训练数据混合、连续微调、RL 与 Agent 运行平台保留为相邻资料，并标明阶段差异，不与当前 mixed SFT 合并解释。

## 如何记录阅读深度

每项条目保留原始链接、发表状态、核读版本、阅读深度、代码归属和与当前问题的距离。

- **关键正文／表格：**已读与判断相关的方法、结果和限制，并记录章节、图表或页码；不宣称逐字通读全文。
- **摘要／初筛：**只确认摘要或作者发布信息，不能据此补出模型、预算、消融或实现细节。
- **代码静态检查：**区分只读 README／目录与已读核心源码；未把这两类写成运行复现。
- **未核实：**公开仓库、正式会议或某项实现尚未获得可靠来源；未找到不等于不存在。

P0／P1／P2／P3 是调研阅读优先级，不是论文质量分数。不同专题的原始标签保留在来源登记中；总图另外给出跨专题建议阅读顺序。

## 发表状态与版本

正式会议身份优先由 ACL Anthology、PMLR、NeurIPS、ICLR proceedings 等确认。作者在 arXiv comments 或仓库写出的接收信息单列；投稿状态不能写成录用。主会、Findings、workshop、技术报告分别注明。

本轮特别保留：mSFT v7 的 AACL 2026 Oral 作者声明；终端监督长度论文的 ICLR 2027 投稿状态；PSFT 正式 PDF 附录中关于 MoE 范围的内部不一致。技术内容来自哪个版本、发表身份如何核实，允许采用不同来源，但分别说明。

## 仓库核查

作者代码必须有论文、作者主页或机构发布的归属链。第三方实现单独标明。检查项目是否包含训练、数据构造、loss 或评测源码，不把项目站点、图片、数据链接和空占位仓库算作完整训练实现。

GitHub stars、最近推送和归档状态为 2026-10-29 的 API 快照；星数不代表方法质量、相关性或可复现性。0 是真实查询值时才记作 0，缺失值保留为空。最近推送也不保证全部功能仍受维护。部分关键源码保存了 commit 或 blob 标识，其余分支链接带核查日期，后续内容可能变化。

## 统计与去重

[资料总目录](paper-catalog.json)合并 A／B／C 三线与基础补充登记。统计单位是独立资料条目，而非检索命中数。论文的 arXiv 与正式会议版本合并；同一代码仓库关联多篇论文时，论文各记一项，仓库按 canonical 名称去重。模型发布说明可以作为独立资源记录，但不因此计作另一篇论文。

原始分线登记保留细节，总目录和人类可读索引是本轮汇总视图。更新时需要同步汇总视图。完整范围与计数见[首页](../README.md)。

## 证据尚缺的部分

当前未读取业务训练数据、精确配置或原始评分文件；问题现象来自用户确认。此次未运行训练、复现论文结果或测量各框架对当前模型的兼容性。PG-SFT 全文抓取未成功，保留原始摘要级标记；具体受限项在各笔记中说明。

本轮“未找到完全同题证据”只描述已覆盖材料，不支持“首次发现”“业界无解”等新颖性结论。后续文献加入时，应修正现有判断及来源边界。
