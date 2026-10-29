# R04 · GradNorm：梯度幅度与训练速度平衡

- **论文：**GradNorm: Gradient Normalization for Adaptive Loss Balancing in Deep Multitask Networks，**ICML 2018**。[正式原文页](https://proceedings.mlr.press/v80/chen18a.html)
- **代码：**本轮未核实作者原实现；已核查的 [LibMTL GradNorm](https://github.com/median-research-group/LibMTL/blob/main/LibMTL/weighting/GradNorm.py) 为第三方参考实现。
- **阅读深度：**正式摘要与参考实现；**P2／经典背景**。

## 方法与边界

GradNorm 调整多任务损失权重，使梯度幅度与任务的相对训练速度相协调。论文在回归、分类等多任务设置报告优于若干固定权重与自适应基线的结果。参考实现公开了可训练 loss scale 与梯度范数目标。

**我们的判断：**它主要提供幅度与速度平衡的视角，需与 PCGrad 的方向冲突区分。原论文不是 LLM 长程轨迹 SFT 研究；损失或梯度平衡也不自动保证本项目两个评测的基线保持。

核查日期：2026-10-29。
