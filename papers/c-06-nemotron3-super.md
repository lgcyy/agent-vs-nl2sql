# Nemotron 3 Super：异构 SFT 与 conversation 归一化

**完整标题**：Nemotron 3 Super: Open, Efficient Mixture-of-Experts Hybrid Mamba-Transformer Model for Agentic Reasoning  
**作者**：NVIDIA。**状态**：官方技术报告／arXiv；官方 PDF 标注 2026-04-03，arXiv 首次提交 2026-04-14，两者日期分列。**核查日**：2026-10-29。  
**定位**：本线最接近 SQL 与 Agentic 同一训练混合物的工业证据；不是当前双降的因果诊断。

## 问题、方法与原文证据

官方报告 §3.1（PDF 第 18–19 页）记载：单阶段 SFT 的长输入、短输出任务退化，随后采用两阶段训练，从全局输出 token 均值切换到先对各 conversation 的输出 token 求均值、再对 conversation 求均值（Eqs. 1–2）。§3.1.1 包含 Agentic Programming（第 19 页）与 SQL（第 23 页）。

**证据边界**：第二阶段还改变长上下文数据、packing 长度及 batch size，因此不是仅改变归一化的隔离消融。全流程还含多类 RL；最终成绩不能全部归功于该 SFT 调整。报告的 SQL 描述也不足以断言所有 SQL 样本均无显式 CoT。

## 局限及与当前问题的关系

**本仓库推断**：它表明样本混合比例之外，还存在由监督长度和 reduction 方式定义的有效权重。长 Agent 轨迹是否获得更多权重，取决于实际被监督的 assistant token、工具消息的 mask 以及 loss 的聚合单位，而非序列总长度本身。

这是一份工业配方与训练观测，可支持“长度与归一化值得作为独立文献维度”。它没有建立用户混训双降与归一化之间的因果链，也没有证明 conversation 归一化在任何任务组合上都更好。

## 代码核验

报告首页链接 [NVIDIA-NeMo/Nemotron](https://github.com/NVIDIA-NeMo/Nemotron)。已读 [stage1_sft/README.md](https://github.com/NVIDIA-NeMo/Nemotron/blob/main/src/nemotron/recipes/super3/stage1_sft/README.md)、[train.py](https://github.com/NVIDIA-NeMo/Nemotron/blob/main/src/nemotron/recipes/super3/stage1_sft/train.py)与 [default.yaml](https://github.com/NVIDIA-NeMo/Nemotron/blob/main/src/nemotron/recipes/super3/stage1_sft/config/default.yaml)。存在调用 Megatron Bridge 的实际 SFT 入口；所读默认配置未明确呈现完整两阶段切换，**不能据此宣称已核实原生产训练可完整复现**。train.py blob：`0540ec284d8cc633a0086cc188c22ec55b7c1221`。API 快照：**2,235 stars**，2026-10-29。

## 一手来源

- [官方技术报告 PDF，§3.1、§3.1.1](https://research.nvidia.com/labs/nemotron/files/NVIDIA-Nemotron-3-Super-Technical-Report.pdf)
- [arXiv 记录](https://arxiv.org/abs/2604.12374)
- [官方 SFT 文档：两种 normalization 与 assistant mask](https://docs.nvidia.com/nemotron/latest/nemotron/super3/sft.html)

