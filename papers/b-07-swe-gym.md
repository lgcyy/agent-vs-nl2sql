# SWE-Gym：可验证环境中的轨迹 SFT 与验证器收益

- 编号：B07
- 完整题名：Training Software Engineering Agents and Verifiers with SWE-Gym。
- 作者与年份：Jiayi Pan、Xingyao Wang 等；预印本 2024，正式发表 2025。
- 发表状态：[ICML 2025，PMLR 267](https://proceedings.mlr.press/v267/pan25g.html)。
- 阅读深度：论文 §4、表3；作者 README、OpenHands 说明、训练配置核查。
- 关联专题：[Agent 轨迹与能力保持](../topics/02-agent-trajectories-and-retention.md)
- 核查日期：2026-10-29

## 问题、做法与有证据的结论

SWE-Gym 为软件工程任务提供可执行环境与测试，使用成功轨迹进行 rejection-sampling SFT，并单独训练验证器。表3以相同 OpenHands 接口比较 Qwen2.5-Coder-Instruct 及微调模型；32B 在 SWE-bench Verified 上为 7.0%→20.6%。[原文 §4.1—4.2、表3](https://arxiv.org/pdf/2412.21139)

摘要中的 32.0% Verified 成绩还结合验证器与候选选择，不能标为“纯 SFT 达到 32%”。表3同时报告空补丁、循环和回合数，说明成功率提高不代表每种行为问题都消失。[原文摘要、表3及验证器章节](https://arxiv.org/pdf/2412.21139)

## 局限与本仓库解读

测试提供的是终局可验证性，不自动保证轨迹中每一步都必要、正确或适合模仿；结果与模型、工具接口和候选选择共同相关。

本文支持“高质量环境与轨迹能够改进单个 Agent 目标”的命题，没有直接涉及 SQL-noCoT 数据混入，更没有证明其他业务能力保持。它与当前“双目标共同回退”的联系是轨迹证据质量与结果归因方式，而非同题解法。

## 代码与公开资产

- [作者仓库 SWE-Gym/SWE-Gym](https://github.com/SWE-Gym/SWE-Gym)由正式论文页连接，含数据、模型、训练和验证器资产。
- 已读 [OpenHands 说明](https://github.com/SWE-Gym/SWE-Gym/blob/main/docs/OpenHands.md)及[策略训练配置](https://github.com/SWE-Gym/SWE-Gym/blob/main/scripts/training/openhands/configs/policy/1116-sonnet-4o-491i-32k-qwen25_coder_32b_full-lr1e-4.yaml)：可核查模型、对话数据和输入损失屏蔽设置；目录含主训练脚本。
- 作者要求使用其特定 OpenHands fork；平台最新版与论文配置不能混用。此处只做静态源码核查，未下载环境、训练或执行评测。

## 来源

1. [ICML 正式发表记录](https://proceedings.mlr.press/v267/pan25g.html)
2. [论文 v2 PDF：§4、表3](https://arxiv.org/pdf/2412.21139)
3. [作者仓库及 OpenHands 说明](https://github.com/SWE-Gym/SWE-Gym/blob/main/docs/OpenHands.md)

