# LS-Mixture：长短 CoT 配对混合

**完整标题**：Long-Short Chain-of-Thought Mixture Supervised Fine-Tuning Eliciting Efficient Reasoning in Large Language Models  
**作者**：Bin Yu 等。**状态**：arXiv 预印本，v1，2025-05-06；检索到投稿记录不等于正式录用，未核实会议发表。**核查日**：2026-10-29。  
**定位**：同题不同长度推理表达；与异构任务混训仅部分相邻。

## 问题、方法与原文证据

§2 将长 CoT 改写为保留结构的短 CoT，再以风格提示进行混合 SFT；推理时另有 balanced 提示。主要设置涉及 Qwen2.5-32B-Instruct 与 s1K。§4.1／Table 2 检查改写，§4.2／Table 3 比较长、短、混合监督，§4.3／Table 4 检查推理提示。应把监督与推理时的控制同时纳入理解，而非把结果全部归因于“混合”二字。

## 局限及与当前问题的关系

**本仓库推断**：这篇能支持“同一能力可有不同表达长度，混合并非原则上不可行”的讨论，但它的短样本仍含 CoT。成对的同题长短解答与 SQL-noCoT／长 Agent 两类异构数据在内容、格式、信息量及交互结构上有根本差别。

论文对样本量／epoch 的调整不能直接证明总监督 token 或训练计算量匹配。它也没有提供当前两个任务均低于共同 base 的案例；不应据此替用户决定长短比例。

## 代码核验

论文直接链接 [项目仓库](https://github.com/ZGCA-AI4Edu/LS-Mixture)。已读 README、根目录与 [package.json](https://github.com/ZGCA-AI4Edu/LS-Mixture/blob/main/package.json)：这是 Vue／Vite 项目页面，脚本为网页开发与构建；README 还有 under-development／TODO 内容。**截至核查仅确认项目网站和方法展示，未见训练实现**，不能写作“代码已完整开源”。核读 package blob：`eb7108f7fa0971b7ba4223d0497922304d302e20`。API 快照：**10 stars**，2026-10-29。

## 一手来源

- [arXiv 记录](https://arxiv.org/abs/2505.03469)；[所读 v1 全文](https://arxiv.org/html/2505.03469v1)
- [作者项目页](https://zgca-ai4edu.github.io/LS-Mixture/)；[README](https://github.com/ZGCA-AI4Edu/LS-Mixture/blob/main/README.md)

