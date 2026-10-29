# AgentGym：跨环境轨迹监督及 SQL 任务的边界

- 编号：B04
- 正式题名：AgentGym: Evaluating and Training Large Language Model-based Agents across Diverse Environments。
- 作者与年份：Zhiheng Xi 等；预印本 2024，正式发表 2025。
- 发表状态：[ACL 2025 主会长文](https://aclanthology.org/2025.acl-long.1355/)。预印本及仓库仍使用 Evolving 开头的旧题名，不能因此登记成两篇。
- 阅读深度：正式论文 §4—6、表2—3；作者 README、行为克隆实现核查。
- 关联专题：[Agent 轨迹与能力保持](../topics/02-agent-trajectories-and-retention.md)
- 核查日期：2026-10-29

## 问题、做法与有证据的结论

AgentGym 将多个环境整合为统一交互接口，收集并筛选轨迹。§5.1 的行为克隆同时使用 AgentTraj 和通用数据；表3分别报告 SFT 与后续自改进模型，不能把后者收益归入离线 SFT。[原文 §4—6、表2—3](https://aclanthology.org/2025.acl-long.1355.pdf)

正式版本包含 BIRD-SQL 环境，但这只是其跨环境任务集合之一。表2与附录中的轮次统计说明不同环境的交互深度不同；“统一轨迹格式”不意味着每个任务都具有同等长程性质。[原文表2、附录表7](https://aclanthology.org/2025.acl-long.1355.pdf)

## 局限与本仓库解读

收录它是为了理解多源轨迹的训练表达与来源，不把本仓库扩展成环境平台清单。包含 BIRD 不等于研究过 NL2SQL-noCoT 与长程轨迹混合后的双侧回退，也不能替代对两类监督的单独定义。

其后续探索和强化学习结果仅作为论文边界记录；本专题关注离线轨迹与行为克隆阶段。

## 代码与公开资产

- 官方来源：[论文指定仓库 WooooDyy/AgentGym](https://github.com/WooooDyy/AgentGym)、[作者项目页](https://agentgym.github.io/)。
- 已读[行为克隆实现](https://github.com/WooooDyy/AgentGym/blob/main/agentenv/agentenv/trainer/bc_trainer.py)：包含数据读取、角色到标签的转换、输入损失屏蔽与长度截断；[多任务入口](https://github.com/WooooDyy/AgentGym/blob/main/agentenv/examples/behavioral_cloning/train_behavioral_clone_multi_task.sh)在目录中存在。
- 这是实质性代码发布；只确认实现范围，未证明当前依赖可直接运行，亦未将仓库的新 RL 扩展混入原论文结论。

## 来源

1. [正式发表记录及 PDF](https://aclanthology.org/2025.acl-long.1355/)
2. [旧题名预印本](https://arxiv.org/abs/2406.04151)
3. [作者 README](https://github.com/WooooDyy/AgentGym/blob/main/README.md)
4. [行为克隆源文件](https://github.com/WooooDyy/AgentGym/blob/main/agentenv/agentenv/trainer/bc_trainer.py)

