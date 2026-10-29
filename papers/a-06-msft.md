# A06 · mSFT：混合数据的异步过拟合

- **论文**：mSFT: Addressing Dataset Mixtures Overfitting Heterogeneously in Multi-task SFT，Koh et al.
- **状态**：[arXiv:2603.21606](https://arxiv.org/abs/2603.21606)，首发 2026-03-23；本轮阅读 v7（2026-10-26）。作者 comments 标注 **AACL 2026 Oral**；本轮未核实正式会场论文集，保留“作者接收声明”与“正式出版”的区别。
- **优先级 / 范围**：P0；直接研究多任务 SFT 学习动态。
- **核查**：2026-10-29；已读 §2–4 关键段落与主表，并静态检查仓库流水线；未复现。

## 原文证据

§2 / Figure 2 呈现各子数据集达到最好验证表现的时点不同。§3 的 mSFT 逐次识别最早过拟合的数据集，回退 checkpoint 后继续余下混合；Figure 3 表明移除任务会改变其他任务的最佳时点。§4 / Tables 2–3 比较六个模型与多种 SFT 基线，主要报告跨十个 benchmark 的表现。[v7 全文](https://arxiv.org/pdf/2603.21606v7)

## 局限与本案例关系（调研判断）

它把“混合后每个子任务的学习历程”作为独立问题，和仅讨论静态比例形成互补。本文的训练任务与模型范围不等于用户的 27B 模型和长程轨迹；其现象不能证明当前 SQL 与 agent 的双侧回退源于过拟合。新版本结论也不能自动认为已被较早代码版本完全覆盖。

## 代码核查

[reiss-koh/msft](https://github.com/reiss-koh/msft) 由[作者投稿页面](https://huggingface.co/papers/2603.21606)链接，README 回链同名论文。已见混合数据、训练评测、过拟合分析及 checkpoint/数据集更新模块；已读 [run_mSFT.sh](https://github.com/reiss-koh/msft/blob/main/sft/run_mSFT.sh)，包含分阶段训练、分析及恢复逻辑，不是占位仓库。

GitHub API：**14 stars**，2026-10-29；仓库最近推送为 2026-03-24，早于论文 v7，版本对齐未验证。

