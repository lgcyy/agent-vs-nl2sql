# Agent-R：从成功终点到可学习的错误恢复轨迹

- 编号：B06
- 完整题名：Agent-R: Training Language Model Agents to Reflect via Iterative Self-Training。
- 作者与年份：Siyu Yuan 等，2025。
- 发表状态：[arXiv:2501.11425](https://arxiv.org/abs/2501.11425)，本轮核查版本 v3（2025-03-24）；未核实正式会议发表。
- 阅读深度：原文 §3—4、表2与图3；README、轨迹构造关键源文件核查。
- 关联专题：[Agent 轨迹与能力保持](../topics/02-agent-trajectories-and-retention.md)
- 核查日期：2026-10-29

## 问题、做法与有证据的结论

作者针对行为克隆模型发生错误后难以恢复，使用搜索产生正确与错误路径，再构造及时修正的轨迹，迭代进行监督学习。主要模型为 Llama-3.1-8B-Instruct；训练还混入 ShareGPT。[原文 §3、§4.2](https://arxiv.org/pdf/2501.11425)

表2与图3比较 WebShop、SciWorld、TextCraft 上的最终表现和不同轨迹构造方式，支持其设置下修正轨迹的价值；WebShop/SciWorld 采用最终奖励，TextCraft 采用成功率，不能把三个数字都称为成功率。[原文 §4.2—4.4、表2、图3](https://arxiv.org/pdf/2501.11425)

## 局限与本仓库解读

该工作不是“收集所有失败轨迹直接模仿”。错误历史可以充当上下文，修正动作才是学习目标之一；这种区分比只问轨迹是否最终成功更精确。

它研究迭代构造数据后的 Agent 改进，没有建立 SQL-noCoT 与长轨迹混训双降对照，也没有用广泛外部任务证明已有能力全面保留。本文借鉴的是轨迹质量的定义，不能把方法直接称为当前问题的解决方案。

## 代码与公开资产

- 论文链接的 bytedance/Agent-R 现指向 [ByteDance-Seed/Agent-R](https://github.com/ByteDance-Seed/Agent-R)，作者来源链可核实。GitHub 页面显示已归档，活跃度应单独更新。
- [README](https://github.com/ByteDance-Seed/Agent-R/blob/main/README.md)说明环境、数据生成及外部 XTuner 训练依赖。
- 已读 [path_collection.py](https://github.com/ByteDance-Seed/Agent-R/blob/main/path_collection.py)：包含路径修正、配对、对话转换，以及错误 assistant 片段的 loss 标记。仓库还有搜索采集与评测脚本；未运行流程，不能宣称可直接复现。

## 来源

1. [预印本记录与版本](https://arxiv.org/abs/2501.11425)
2. [论文 PDF：§3—4、表2、图3](https://arxiv.org/pdf/2501.11425)
3. [作者仓库与关键实现](https://github.com/ByteDance-Seed/Agent-R/blob/main/path_collection.py)

