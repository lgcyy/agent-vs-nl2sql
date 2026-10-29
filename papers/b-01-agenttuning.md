# AgentTuning：Agent 轨迹与通用指令混合训练的早期证据

- 编号：B01
- 作者与年份：Aohan Zeng 等；预印本 2023，正式发表 2024。
- 发表状态：[Findings of ACL 2024](https://aclanthology.org/2024.findings-acl.181/)，不是 ACL 主会长文。
- 阅读深度：原文重点章节与表格；作者仓库 README、目录和评测入口核查，未运行代码。
- 关联专题：[Agent 轨迹与能力保持](../topics/02-agent-trajectories-and-retention.md)
- 核查日期：2026-10-29

## 问题、做法与有证据的结论

作者希望提高 Agent 能力，同时保留通用语言能力。AgentInstruct 收集并按环境奖励筛选交互轨迹，再与 ShareGPT 混合微调 Llama-2-Chat；模型输出参与损失。[原文 §2.1—2.2](https://aclanthology.org/2024.findings-acl.181.pdf)

§3.4、表4将混训与 agent-only、general-only 比较：agent-only 的域内得分提高，但域外 Agent 与通用能力较弱；混训改善这一情况。7B/13B 上混训的域外收益与 general-only 接近，70B 的情况不同，不能只引用总体提升。[原文表3—4、§3.4](https://aclanthology.org/2024.findings-acl.181.pdf)

## 局限与本仓库解读

这是一组具体模型、数据和评估下的条件性结果。“加入通用指令有帮助”不等于任意两类业务监督都能成功混合。本文的数据库任务也不能直接替代 NL2SQL-noCoT 的任务定义。

与当前双降问题的关系：它提供“Agent 学习与外部能力保持”的近邻证据，以及成功混训的反例；没有建立“两个分别可用的训练目标混合后，同一 checkpoint 两侧均低于 base”的问题。应与 [AgentBank](b-03-agentbank.md) 的不同观察一起阅读，避免把 agent-only 的退化写成普遍规律。

## 代码与公开资产

- 作者项目：[THUDM/AgentTuning](https://github.com/THUDM/AgentTuning)，[作者项目页](https://thudm.github.io/AgentTuning/)；数据与 AgentLM 权重由 README 连接到作者 Hugging Face。
- [README](https://github.com/THUDM/AgentTuning/blob/main/README.md) 与 [仓库目录](https://api.github.com/repos/THUDM/AgentTuning/git/trees/main?recursive=1)主要提供部署、AgentBench.old、held-out 与通用评测入口。本轮未发现可直接对应 AgentLM 主混训过程的独立入口；嵌套环境中的训练文件不能当作论文主训练实现。
- 已读 [MMLU 评测实现](https://github.com/THUDM/AgentTuning/blob/main/eval_general/eval_mmlu_hf.py)，确认有模型加载与对话模板构造。README 中所写 evaluate_mmlu_hf.py 与实际文件名 eval_mmlu_hf.py 不一致；入口说明也需要对照目录。
- README 明确提醒最新 AgentBench 与论文所用版本可能不同。公开资产充足，不等于已核实完整训练复现链。

## 来源

1. [正式发表记录](https://aclanthology.org/2024.findings-acl.181/)
2. [正式论文 PDF：§2、§3.4、表3—4](https://aclanthology.org/2024.findings-acl.181.pdf)
3. [作者仓库 README](https://github.com/THUDM/AgentTuning/blob/main/README.md)

