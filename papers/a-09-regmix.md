# A09 · RegMix：用回归预测数据比例的效果

- **论文**：RegMix: Data Mixture as Regression for Language Model Pre-training，Liu et al.
- **发表**：ICLR 2025；[官方 proceedings](https://proceedings.iclr.cc/paper_files/paper/2025/hash/5f67d864aae6115374fed7beddd119e0-Abstract-Conference.html)。预印本首发 2024-07-01。
- **优先级 / 范围**：P2；**预训练方法参照**。
- **核查**：2026-10-29；已读方法、Table 2 和代码说明/目录；未复现。

## 原文证据

方法从许多小代理模型的混合比例与验证损失拟合回归器，预测更优比例后训练大模型。§3 描述流程；§4 / Table 2 以未见比例上的排序相关性检验跨规模迁移，在其设置下支持代理排序的有效性。研究任务是语言模型预训练，并非已具备两种业务能力的模型联合 SFT。[全文](https://arxiv.org/pdf/2407.01492)

## 局限与本案例关系（调研判断）

适合帮助区分“预测比例的搜索方法”与“解释两域为何同时退化”。迁移依赖代理训练与目标模型之间的关联；已有 checkpoint 的训练历史、响应监督掩码和交互轨迹，使本案例不等同于从头预训练。原文的跨规模成功不构成所有模型和所有训练阶段都保持比例排序的保证。

## 代码核查

官方 proceedings 摘要直接链接 [sail-sg/regmix](https://github.com/sail-sg/regmix)，归属已验证。已确认比例生成、回归 notebook、代理/大模型训练脚本，以及数据模型发布说明。主 README 仍将 evaluation 标作 WIP，但目录中已有 [评测说明](https://github.com/sail-sg/regmix/blob/main/evaluation/README.md)及 harness 脚本；不能据 WIP 标签误写成“评测代码不存在”。

GitHub API：**217 stars**，2026-10-29。读取层级为 README、目录和评测说明；训练全流程未运行。

