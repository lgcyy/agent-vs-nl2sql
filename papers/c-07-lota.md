# LoTA：Lottery Ticket Adaptation

**完整标题**：Lottery Ticket Adaptation: Mitigating Destructive Interference in LLMs  
**作者**：Ashwinee Panda、Berivan Isik、Xiangyu Qi、Sanmi Koyejo、Tsachy Weissman、Prateek Mittal。  
**状态**：arXiv 预印本；首发 2024-06-24，所读 v2 为 2024-06-25。可见 ICLR 2025 投稿记录，但未核实录用，故不标 ICLR 2025 已发表。**核查日**：2026-10-29。

## 问题、方法与原文证据

§3／Algorithm 1 先校准微调，以较大参数改变量生成稀疏 mask，重置回起点后训练子网络；LoTTO 进一步在连续任务中分配不重叠 mask。§5.2／Table 3 是单任务结果，§5.3／Table 5 是顺序学习，§5.4／Table 6 是模型合并。

**指标须分清**：Table 3 含 SQL，但该列使用 **ROUGE-1**，不能把接近 99 的数字解读为 SQL 执行正确率。顺序学习与合并的收益，也不是 SQL、Agent 同批混训的直接证据。

## 局限及与当前问题的关系

**本仓库推断**：LoTA 将干扰放到“哪些参数允许改变”这一层，与 loss 加权或数据比例是不同杠杆。稀疏更新有可能减少参数争用，但保留坐标不等于保留所有函数行为；校准过程也有额外成本和任务依赖。

当前业务若需要一个混训 checkpoint，不能直接用多模型合并结果替代该要求；若关注 SQL，应优先理解原文指标能否支持语义正确性的结论，而不是仅按任务名字匹配文献。

## 代码核验

论文链接 [作者仓库](https://github.com/kiddyboots216/lottery-ticket-adaptation)。已读 README、[apply_mask_in_backward.py](https://github.com/kiddyboots216/lottery-ticket-adaptation/blob/main/rlaif/apply_mask_in_backward.py)与 [continual_learning.sh](https://github.com/kiddyboots216/lottery-ticket-adaptation/blob/main/rlaif/scripts/continual_learning.sh)，可见梯度 mask、校准／保存 mask／稀疏训练命令。**有实现与运行脚本，README 标注 work in progress，未运行复现**。脚本 blob：`57d711276b615502ab2dd24056acd4b6e9bb7982`。API 快照：**40 stars**，2026-10-29。

## 一手来源

- [arXiv 记录](https://arxiv.org/abs/2406.16797)；[所读 v2 全文](https://arxiv.org/html/2406.16797v2)
- [OpenReview 投稿记录](https://openreview.net/forum?id=qOqCXEXsX4)
- [README](https://github.com/kiddyboots216/lottery-ticket-adaptation/blob/main/README.md)

