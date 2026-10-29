# R03 · CAGrad：平均目标与单任务局部改进

- **论文：**Conflict-Averse Gradient Descent for Multi-task Learning，**NeurIPS 2021**。[正式论文页](https://proceedings.neurips.cc/paper/2021/hash/9d27fdf2477ffbff837d73ef7ae23db9-Abstract.html)
- **作者代码：**[Cranial-XIX/CAGrad](https://github.com/Cranial-XIX/CAGrad)，161 stars（2026-10-29 API）。
- **阅读深度：**原文摘要与目标定义、README、核心演示实现；**P2／经典背景**。

## 问题与做法

CAGrad 在优化任务平均损失时，用各任务中较差的局部改进约束更新方向。论文给出相应条件下的收敛分析，并在多任务监督及强化学习任务中比较梯度处理方法。[原文](https://arxiv.org/abs/2110.14048)

## 代码与局限

[toy.py](https://github.com/Cranial-XIX/CAGrad/blob/dc3d48152b6196945cfd56144879b9d42353b095/toy.py) 含 `cagrad`、`pcgrad` 和多目标优化演示；仓库还提供视觉多任务和 MetaWorld 相关目录。它不是面向当代 LLM 的即用型训练工程。

**我们的判断：**“局部更新照顾各任务”与“两个任务的最终评测都不低于初始模型”是不同命题；前者的理论或经验结果不能直接保证后者。这个边界对解读多任务论文的保证范围有用。

核查日期：2026-10-29。
