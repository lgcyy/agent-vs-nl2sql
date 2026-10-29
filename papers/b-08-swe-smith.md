# SWE-smith：可验证任务与轨迹的规模、质量及行为差异

- 编号：B08
- 完整题名：SWE-smith: Scaling Data for Software Engineering Agents。
- 作者与年份：John Yang、Kilian Lieret 等，2025。
- 发表状态：[NeurIPS 2025 Datasets and Benchmarks Track](https://papers.nips.cc/paper_files/paper/2025/hash/8b86cf5ace600c48fd188efbb8dedec8-Abstract-Datasets_and_Benchmarks_Track.html)；作者仓库标为 Spotlight。
- 阅读深度：正式论文 §3—4、表3—5、附录F；README、训练说明与关键源文件核查。
- 关联专题：[Agent 轨迹与能力保持](../topics/02-agent-trajectories-and-retention.md)
- 核查日期：2026-10-29

## 问题、做法与有证据的结论

作者合成能被测试验证的软件任务，采集成功轨迹并微调 SWE-agent 模型。表3明确采用 pass@1，排除验证器或多次尝试系统的直接混比。§4.1、表4—5显示不同任务与描述生成方式产生不同训练效果。[正式论文 §3—4、表3—5](https://papers.nips.cc/paper_files/paper/2025/file/8b86cf5ace600c48fd188efbb8dedec8-Paper-Datasets_and_Benchmarks_Track.pdf)

任务难度与下游训练收益并非简单同向；附录F.4、图18比较成功筛选和随机取轨迹。行为分析仍发现重复动作与失败相关，不能把“通过测试的训练样本”解释为完整的过程质量保证。[正式论文 §4.1、图7、图18](https://papers.nips.cc/paper_files/paper/2025/file/8b86cf5ace600c48fd188efbb8dedec8-Paper-Datasets_and_Benchmarks_Track.pdf)

## 局限与本仓库解读

需要区分任务可执行、终局验证、轨迹可学性与评测时的行为效率。它们有关联，但任何一个指标都不能自动替代其他指标。

该工作研究软件 Agent 数据构造与 SFT，没有 SQL-noCoT 混合对照或全面能力保持结论。其价值是提醒“多、难、长、成功”都不是可以互换的质量标签；不能据此确认本仓库混训退化来自哪一种质量问题。

## 代码与公开资产

- 官方资产：[SWE-bench/SWE-smith](https://github.com/SWE-bench/SWE-smith)、[作者网站](https://swesmith.com)。
- 已读[训练说明](https://github.com/SWE-bench/SWE-smith/blob/main/docs/guides/train_swe_agent.md)、[轨迹转换实现](https://github.com/SWE-bench/SWE-smith/blob/main/swesmith/train/traj_mgr/collect_trajs.py)、[训练调用实现](https://github.com/SWE-bench/SWE-smith/blob/main/swesmith/train/run/ft_torchtune.py)。代码覆盖环境/任务、轨迹转换和训练调用，依赖 SWE-agent 与外部训练库。
- 当前仓库和数据资产持续演进；不能用当前 52k/250+ 等 README 数字替换论文版本的 50k/128 仓库统计。没有运行这些实现。

## 来源

1. [正式发表记录](https://papers.nips.cc/paper_files/paper/2025/hash/8b86cf5ace600c48fd188efbb8dedec8-Abstract-Datasets_and_Benchmarks_Track.html)
2. [正式论文 PDF](https://papers.nips.cc/paper_files/paper/2025/file/8b86cf5ace600c48fd188efbb8dedec8-Paper-Datasets_and_Benchmarks_Track.pdf)
3. [作者训练文档](https://github.com/SWE-bench/SWE-smith/blob/main/docs/guides/train_swe_agent.md)

