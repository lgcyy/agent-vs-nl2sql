# HFT：Half Fine-Tuning for Large Language Models

**作者**：Tingfeng Hui、Zhenyu Zhang、Shuohuan Wang、Weiran Xu、Yu Sun、Hua Wu。  
**状态**：ACL 2025 Long Paper，pp. 12791–12819；arXiv 初稿为 2024 年，本卡以 ACL 正式版为准。**核查日**：2026-10-29。  
**定位**：通过冻结部分参数减少连续微调的遗忘。

## 问题、方法与原文证据

HFT 对参数类别施加约一半冻结、另一半更新的约束；§4.1 涉及 SFT／DPO，§4.2 的 TRACE 结果使用 OP、BWT 指标，Table 4 比较选择粒度。它不是简单固定“每隔一层冻结”。

**关键实现语义**：Appendix Algorithm 1 位于 PDF 第 14 页、印刷页 12804，按 fine-tuning **stage** 选择参数；不能写成“每个 epoch 重新随机选一半”。算法对 FFN／SAN／LN 等类别处理，embedding 与 lm_head 被单独排除在该冻结循环外。

## 局限及与当前问题的关系

**本仓库推断**：HFT 提供“限制可更新子空间”的简单参照。减少遗忘不自动说明异构任务间更兼容；冻结也可能限制新 schema 或新交互协议的学习能力。其比例与分类策略应视为论文中的方法定义，不能直接当作任意模型的最优常数。

顺序微调的 backward transfer 和同时混合两个任务、两侧又均低于 base 是不同证据对象。当前问题可借用能力保持视角，但不能把负 BWT 的改善直接换算成混训双升。

## 代码核验

已核对 ACL 论文页面、PDF 与作者相关公开页面，**尚未确认作者官方实现仓库**。因此不提供猜测的同名仓库，不列未经核实的 stars，也不把文中伪代码当作已经发布的训练代码。Algorithm 1 足以说明设计，但不足以核实具体训练系统的实现。

## 一手来源

- [ACL Anthology 正式页面](https://aclanthology.org/2025.acl-long.626/)
- [ACL 正式 PDF，尤其 Algorithm 1、Tables 3–4](https://aclanthology.org/2025.acl-long.626.pdf)
- [作者 Zhenyu Zhang 主页](https://zhenyu.ac.cn/)

