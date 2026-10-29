# AgentRefine：纠错轨迹、损失范围与泛化退化

- 编号：B05
- 完整题名：AgentRefine: Enhancing Agent Generalization through Refinement Tuning。
- 作者与年份：Dayuan Fu 等，2025。
- 发表状态：[ICLR 2025 正式会议论文](https://proceedings.iclr.cc/paper_files/paper/2025/hash/a3cc50126338b175e56bb3cad134db0b-Abstract-Conference.html)。
- 阅读深度：正式论文 §4、表1—2；作者仓库 README 与目录核查。
- 关联专题：[Agent 轨迹与能力保持](../topics/02-agent-trajectories-and-retention.md)
- 核查日期：2026-10-29

## 问题、做法与有证据的结论

作者针对 Agent 调优后对新环境适应不足，合成含错误与纠正的交互轨迹，并区分错误步骤和纠正步骤的监督。表2显示，在该设置下让错误轮次也参与损失会降低多项结果。[原文 §4.2、表2](https://proceedings.iclr.cc/paper_files/paper/2025/file/a3cc50126338b175e56bb3cad134db0b-Paper-Conference.pdf)

表1并不支持“所有底座能力都保持”：Llama-3-70B-Instruct 的 SciWorld success 为 42.2，AgentRefine 为 17.7。该论文相对若干 Agent-tuning 基线改善泛化，与全面超过未微调底座是不同命题。[原文表1](https://proceedings.iclr.cc/paper_files/paper/2025/file/a3cc50126338b175e56bb3cad134db0b-Paper-Conference.pdf)

## 局限与本仓库解读

轨迹的最终成功不等于每个中间 token 都是应该模仿的目标；这是阅读该论文时需要保留的监督层次，而不是已经确认的本仓库故障原因。

它提供的是 Agent 调优后某些任务低于起点的相关实例，不能替代“两种各自可用的数据混合，两个训练目标共同低于 base”的证据。错误恢复、跨环境泛化和原有能力保持也不能共用一个成功指标。

## 代码与公开资产

- [作者项目页](https://agentrefine.github.io/)链接 [Fu-Dayuan/AgentRefine](https://github.com/Fu-Dayuan/AgentRefine)，作者来源链已核实。
- 本轮[根目录核查](https://api.github.com/repos/Fu-Dayuan/AgentRefine/contents)仅见 README 和 picture；README 提供作者训练数据链接，没有发布可核查的完整合成、训练主实现。
- 因此资产状态为“作者数据发布，训练源码缺口待核实”，不能由仓库标题中的 Code 推断实现齐全。

## 来源

1. [正式发表记录](https://proceedings.iclr.cc/paper_files/paper/2025/hash/a3cc50126338b175e56bb3cad134db0b-Abstract-Conference.html)
2. [正式论文 PDF：§4.2、表1—2](https://proceedings.iclr.cc/paper_files/paper/2025/file/a3cc50126338b175e56bb3cad134db0b-Paper-Conference.pdf)
3. [作者仓库](https://github.com/Fu-Dayuan/AgentRefine)
