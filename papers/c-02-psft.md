# PSFT：Proximal Supervised Fine-Tuning

**作者**：Wenhong Zhu 等。**状态**：ICLR 2026 Conference；首发 2025-08-25，arXiv v2 为 2026-04-12；本卡同时核读正式会议 PDF。**核查日**：2026-10-29。  
**定位**：通过局部更新限制保留能力；高优先级，混合数据有效性尚未建立。

## 问题、方法与原文证据

PSFT 在离线示范上使用新旧 token 概率比的 clipped surrogate objective；旧策略会更新，SFT warm-up 可选（§3、Eq. 6）。它没有在线采样奖励优化环节，不能因借用了 PPO 形式而称为在线 RL。Table 1 与训练动态展示目标任务学习和域外能力保持；Appendix C.7 明确实验使用 full fine-tuning、token-mean 聚合。

**范围与版本问题**：Appendix A（正式 PDF 第 15 页）明确把 mixed data 留作未来工作；该段同时称 MoE 未测，但 C.3／Table 8（第 16–17 页）已经加入 Qwen3-30B-A3B-2507-Instruct MoE。应记录此内部不一致，不能重复“完全没有 MoE 评测”。

## 局限及与当前问题的关系

**本仓库推断**：PSFT 更贴近“学习目标任务时，减少已有能力受损”的问题，值得与数据混合方法区分讨论。但限制相邻策略的 token 概率变化，不等于对最初 base 的所有行为施加永久硬约束。论文比较还涉及学习率、更新批次和 checkpoint 选择；不能把全部差异只归因于公式。

当前混训的两个目标任务都退步，包含了“新任务没学好”和“原能力失去”的可能组合。PSFT 的单域学习／域外保持证据不能直接证明其能协调两个异构监督来源。

## 代码核验

[仓库](https://github.com/zwhong714/PSFT)与作者身份的链条为：会议论文作者邮箱前缀 `zwhong714` → [同名作者主页](https://github.com/zwhong714)的姓名、机构与置顶 PSFT。已读 README，并检查 [psft_ray_trainer.py](https://github.com/zwhong714/PSFT/blob/main/verl/recipe/psft/psft_ray_trainer.py)的示范数据、old log-prob 与 actor 更新，以及 [core_algos.py](https://github.com/zwhong714/PSFT/blob/main/verl/verl/trainer/ppo/core_algos.py)的 ratio clipping。**有训练实现，未运行复现**；前者 blob 为 `bcd00dcff2f5e33845baf72bace72b1854e0405f`。API 快照：**40 stars**，2026-10-29。

## 一手来源

- [ICLR 2026 正式论文集](https://proceedings.iclr.cc/paper_files/paper/2026/hash/da75d2bbf862b86f10241d0887613b41-Abstract-Conference.html)；[正式 PDF](https://proceedings.iclr.cc/paper_files/paper/2026/file/da75d2bbf862b86f10241d0887613b41-Paper-Conference.pdf)
- [arXiv 记录](https://arxiv.org/abs/2508.17784)；[v2 全文](https://arxiv.org/html/2508.17784v2)
- [README](https://github.com/zwhong714/PSFT/blob/main/README.md)

