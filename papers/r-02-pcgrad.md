# R02 · PCGrad：梯度方向冲突的经典参照

- **论文：**Gradient Surgery for Multi-Task Learning，**NeurIPS 2020**。[正式论文页](https://papers.neurips.cc/paper_files/paper/2020/hash/3fe78a8acf5fda99de95303940a2420c-Abstract.html)
- **作者代码：**[tianheyu927/PCGrad](https://github.com/tianheyu927/PCGrad)，374 stars（2026-10-29 API）。
- **阅读深度：**原文引言及问题定义、作者实现；**P2／经典背景**。

## 方法与证据

原文将负余弦的任务梯度视作方向冲突，并结合梯度幅度差异和曲率讨论其不利影响。PCGrad 对相冲突的梯度做投影处理，作者在多任务监督及强化学习设置报告收益。[原文](https://arxiv.org/abs/2001.06782)

## 代码与边界

实际作者文件为 [PCGrad_tf.py](https://github.com/tianheyu927/PCGrad/blob/c5fbd7c856526373828074f06875230f7f3ee79e/PCGrad_tf.py)，是 TensorFlow 1 风格优化器封装；不能写成现成的 PyTorch LLM trainer。PyTorch 参考实现可见 [LibMTL](https://github.com/median-research-group/LibMTL/blob/main/LibMTL/weighting/PCGrad.py)，属于其他维护者的实现。

**我们的判断：**该工作提供“冲突”的可区分概念；两个任务分数下降不等于已经观测到梯度冲突，更不等于已证明它是退化原因。迁移到长程 LLM SFT 还涉及粒度、计算成本和其他优化条件。

核查日期：2026-10-29。
