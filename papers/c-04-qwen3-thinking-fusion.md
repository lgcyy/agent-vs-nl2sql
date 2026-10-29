# Qwen3：Thinking Mode Fusion 的成功范围与代价

**论文**：Qwen3 Technical Report。**作者**：Qwen Team。**状态**：技术报告／arXiv；不标作已同行评审论文。所读版本：v1，2025-05-14。**核查日**：2026-10-29。  
**定位**：显式模式条件下的 thinking／non-thinking 融合。它不是对用户所称 Qwen-3.8-27B 训练历史的核实。

## 问题、方法与原文证据

§4 的主流程包含 Long-CoT cold-start SFT、Reasoning RL、Thinking Mode Fusion SFT、General RL。§4.3 用模式标记及一致的 think 区域格式融合两种输出风格；non-thinking 使用空 think 区域。

**融合有代价**：§4.7、Table 22 中，Qwen3-32B 的 thinking 模式从 Stage 2 → Stage 3 → Stage 4，AIME24 为 **83.8 → 81.9 → 81.4**，LiveCodeBench v5 为 **68.4 → 67.2 → 65.7**。因此最终多能力表现不能被引用为“纯 SFT 无损混合”的证明。

## 局限及与当前问题的关系

**本仓库推断**：报告最有用的启发是把输出约定显式化。SQL-only、解释加 SQL、工具调用与最终答复，在监督中可能是不同的条件分布；“任务是什么”与“应输出哪种形式”值得分开概念化。

这里的 non-thinking 描述外显输出模式，不证明模型不需要推理；长 Agent 轨迹包含行动、观察和多轮状态，也不能直接套作长 CoT。报告更未提供 SQL-noCoT 与长 Agent 轨迹的单训／混训／共同 base 对照。

## 代码核验

[官方仓库](https://github.com/QwenLM/Qwen3)由报告链接。已读 README、目录与 [LLaMA-Factory 训练文档](https://github.com/QwenLM/Qwen3/blob/main/docs/source/training/llama_factory.md)；该具体文档仍注明待更新到 Qwen3，正文示例为 Qwen2.5。公开模型、推理示例与第三方框架说明，**不能等同于已开放原始四阶段融合训练配方和数据**。核读文档 blob：`1eda7a106209d1fdcfb17d4d4bb1577730fc48bd`。API 快照：**27,694 stars**，2026-10-29。

## 一手来源

- [报告记录](https://arxiv.org/abs/2505.09388)；[所读全文，§4.3 与 Table 22](https://arxiv.org/html/2505.09388v1)
- [官方 README](https://github.com/QwenLM/Qwen3/blob/main/README.md)

