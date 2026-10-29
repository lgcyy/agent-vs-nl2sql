# LoRA Learns Less and Forgets Less

**作者**：Dan Biderman 等。**状态**：TMLR，2024 年 8 月录用，Featured Certification；首发 2024-05-15，本卡核读最终 arXiv v2（2024-09-20）。**核查日**：2026-10-29。  
**定位**：区分目标任务学习与能力保持，避免把 PEFT 的小更新直接等同于更好泛化。

## 问题、方法与原文证据

论文比较 full fine-tuning 与不同 LoRA 设置的目标学习和遗忘。最终版 §4.3／Figure 3 呈现依赖数据的权衡；不能概括为所有设置都在同一 Pareto 前沿。§4.4、Appendix C／Table S13 的 Tülu-v2-mix 结果中，部分低秩设置可接近 full fine-tuning 的目标表现，同时长期训练的保持性较好。§4.7 讨论 rank、模块和缩放的敏感性。

## 局限及与当前问题的关系

**本仓库推断**：评价“保留已有能力”必须同时看目标任务学到了多少。一个几乎不改变模型的方案可能不遗忘，也可能没有获得所需业务能力；一个训练得更充分的方案则可能付出保持代价。只按遗忘大小对方法排序会混淆这两种情况。

文中较早模型、数据与训练设置并非当前 27B 模型的 SQL／Agent 混训。它支持把参数更新范围视为变量，不支持预先宣布 LoRA、全参或更低 rank 一定解决双降。

## 代码核验

最终版首页链接 [作者仓库](https://github.com/danbider/lora-tradeoffs)。已读 README 与完整根目录：主要为 README、assets 与 checkpoint／配方说明链接，**该仓库未见训练源码**。README 链接 [作者模型集合](https://huggingface.co/LoRA-TMLR-2024)；不能把 checkpoint 发布写成训练代码完整开放。README blob：`dba704fd9cd8d69a8602ff370c4fa8cae68ae818`。API 快照：**22 stars**，2026-10-29。

## 一手来源

- [arXiv 记录：含 TMLR 接收与版本说明](https://arxiv.org/abs/2405.09673)；[最终 v2 全文](https://arxiv.org/html/2405.09673v2)
- [TMLR／OpenReview 记录](https://openreview.net/forum?id=aloEru2qCG)
- [README](https://github.com/danbider/lora-tradeoffs/blob/main/README.md)

