# R01 · MFTCoder：多任务 SFT 的任务权重与损失汇聚

- **状态：**KDD 2024；预印本首次发表于 2023。正式发表由 [ACM 页面](https://doi.org/10.1145/3637528.3671609) 核实。
- **原文：**[arXiv:2311.02303](https://arxiv.org/abs/2311.02303)；本轮技术阅读对应可获取的 v1 正文 §3、§4.1，注意与正式版本及后续代码区分。
- **作者代码：**[codefuse-ai/MFTCoder](https://github.com/codefuse-ai/MFTCoder)，论文原文直接链接；711 stars（2026-10-29 API）。
- **阅读深度：**关键正文、README、损失与训练入口；未运行代码。**优先级：P1**。

## 问题与做法

论文围绕多个代码任务的样本量、难度和收敛速度差异，区分单任务 SFT、简单合并数据的 SFT-Mixed 与显式多任务训练。§4.1 的比较覆盖五类代码任务；作者在其设置中报告多任务方法的优势。[原文](https://arxiv.org/pdf/2311.02303)

## 实现核查

[loss_utils.py](https://github.com/codefuse-ai/MFTCoder/blob/8ba13f44e2036f1412a343bc343dc6cda7383d73/mftcoder_accelerate/src/utils/loss_utils.py) 的 `loss_func_mft` 在**当前传入批次**内区分：`case1` 为有效 token 平均；`case2` 为各样本先按自己的有效 token 数归一化再平均；`case3/coba` 先在各任务内作 token 平均；`case4` 先在各任务内作样本平均。后两者再按任务权重汇聚，仅遍历当前批次出现的任务，并除以这些任务的权重之和。因此，批次任务组成也参与定义实际目标，不能直接读作固定的全局任务贡献。

同文件含 CoBa 状态逻辑。[MftTrainer](https://github.com/codefuse-ai/MFTCoder/blob/8ba13f44e2036f1412a343bc343dc6cda7383d73/mftcoder_accelerate/src/pefts/mft_trainer.py) 为实质训练入口。CoBa 是后续独立论文的贡献，不应全部归入原始 MFTCoder 结果。

## 与本问题的关系与局限

**我们的判断：**它是理解“数据合并”与“监督权重如何汇聚”差异的直接代码入口。样本条数比例不能独立决定梯度贡献，实际实现还涉及有效 token 与汇聚方式。其代码任务结果没有建立 NL2SQL-noCoT 加长程交互轨迹的适用性，也没有提供本项目双侧跌破 base 的因果解释。

核查日期：2026-10-29。
