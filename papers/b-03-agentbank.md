# AgentBank：混合比例、技能迁移与能力保持的条件性

- 编号：B03
- 完整题名：AgentBank: Towards Generalized LLM Agents via Fine-Tuning on 50000+ Interaction Trajectories。
- 作者与年份：Yifan Song 等，2024。
- 发表状态：[Findings of EMNLP 2024](https://aclanthology.org/2024.findings-emnlp.116/)。
- 阅读深度：原文 §7、表6—7及图4—5；论文指定数据页核查。
- 关联专题：[Agent 轨迹与能力保持](../topics/02-agent-trajectories-and-retention.md)
- 核查日期：2026-10-29

## 问题、做法与有证据的结论

AgentBank 汇集多任务交互轨迹，并将其与通用指令、代码数据混合训练。§7.2 发现少量通用数据有益，比例继续增大可降低域外 Agent 表现；纯 AgentBank 也能提升域外 Agent 得分，作者明确指出这不同于 AgentTuning 的观察。[原文 §7.2、图4](https://aclanthology.org/2024.findings-emnlp.116.pdf)

表6同时显示去掉 ShareGPT 后通用得分下降，而保留混合的 SAMOYED 也未在每项指标超过 base。表7的推理文本消融主要影响 held-out；图5的技能迁移存在负值。[原文表6—7、图5](https://aclanthology.org/2024.findings-emnlp.116.pdf)

## 局限与本仓库解读

需要分开看“域外 Agent 泛化”和“通用能力保留”。同一数据设置可以改善前者，同时仍损失后者；把它们合成一个 generalization 标签会遮蔽结果。

这篇与混训主题较近，但没有当前 SQL-noCoT 与长轨迹各自单训、混训和共同 base 的完整同题对照。其 CoT 消融发生在 Agent 轨迹内部，不能推出 SQL-noCoT 无需推理，也不能证明 noCoT 数据造成双降。

## 代码与公开资产

- 论文首页明确给出[作者数据集 Solaris99/AgentBank](https://huggingface.co/datasets/Solaris99/AgentBank)，来源链已核实。
- 本轮没有从论文或该数据页核实到专门的官方 GitHub 训练仓库；repo_url 保留为空，不用同名第三方项目补位。
- 数据页当前行数与论文统计不是同一快照；正文沿用论文统计，资产页仅用于确认可获取性。未下载或执行数据。

## 来源

1. [正式发表记录](https://aclanthology.org/2024.findings-emnlp.116/)
2. [正式论文 PDF：§7.2—7.4、表6—7、图4—5](https://aclanthology.org/2024.findings-emnlp.116.pdf)
3. [论文指定的作者数据集](https://huggingface.co/datasets/Solaris99/AgentBank)

