# DFT：On the Generalization of SFT

**完整标题**：On the Generalization of SFT: A Reinforcement Learning Perspective with Reward Rectification  
**作者**：Yongliang Wu 等。**状态**：ICLR 2026 Conference；首发 2025-08-07，本卡核读 arXiv v3（2026-02-27）。**核查日**：2026-10-29。  
**定位**：SFT 的 token 概率加权；高优先级，但必须连同反例阅读。这里的 **DFT 是 Dynamic Fine-Tuning**，不同于 A 线的 DiFT（Differential Fine-Tuning）。

## 问题、方法与原文证据

论文从策略梯度重写 SFT，提出用当前模型对示范 token 的概率、停止梯度后乘以交叉熵：`-stopgrad(p) * log(p)`（§3.3）。这降低低概率 token 的更新权重；不是直接对 `-p log(p)` 完整求导。数学、代码、多模态实验分别见 Tables 1、3、4。

**反例必须保留**：§4.5、PDF 第 9 页的 Natural Questions 结果为 base **31.24%**、SFT **36.62%**、DFT **30.14%**。作者将其联系到已有置信度妨碍新知识学习。该数字位于正文，**不是 Table 5**；Table 5 比较不同粒度的概率加权。

## 局限及与当前问题的关系

**本仓库推断**：这是一条修改目标函数的路线，不能从其推理收益推出 SQL 与 Agent 混训也必然获益。新 schema 标识符、罕见但必要的工具动作可能恰好具有低概率；概率不是业务重要性或轨迹质量的标签。反过来，也不能把 NL2SQL 一概归为事实记忆任务，或把 Agent 轨迹一概归为推理任务。

论文没有直接覆盖当前“同一混训 checkpoint 在两任务上均低于各自单训与共同 base”的设定。其梯度重写提供分析视角，不足以证明某次双降由原始 SFT 目标造成。

## 代码核验

[作者仓库](https://github.com/yongliang-wu/DFT)由论文首页直接链接。已读 README，并检查 [fsdp_dft_trainer.py](https://github.com/yongliang-wu/DFT/blob/master/verl/verl/trainer/fsdp_dft_trainer.py)：存在 softmax、目标 token 概率提取、`detach`、loss mask 与有效 token 归一化，属于**已见核心训练实现，未运行复现**。核读文件 blob：`c6961181b5d02ed44fc071547b8be60942eb91f0`。GitHub API 快照：**928 stars**，2026-10-29。

## 一手来源

- [ICLR 2026 正式论文集](https://proceedings.iclr.cc/paper_files/paper/2026/hash/2e53c02ea028cbf603f4b6b47fef3d97-Abstract-Conference.html)
- [arXiv 记录](https://arxiv.org/abs/2508.05629)；[所读 v3 全文](https://arxiv.org/html/2508.05629v3)；[v3 PDF，§4.5 位于第 9 页](https://arxiv.org/pdf/2508.05629v3)
- [README](https://github.com/yongliang-wu/DFT/blob/master/README.md)

