# A10 · Olmix：混合方法的设计选择与数据域更新

- **论文**：Olmix: A Framework for Data Mixing Throughout LM Development，Chen et al.
- **发表**：ICML 2026，2026-07，PMLR 306:16193–16242；[正式论文页](https://proceedings.mlr.press/v306/chen26dd.html)。正文阅读 [arXiv v1](https://arxiv.org/abs/2602.12237)（2026-02-12）。
- **优先级 / 范围**：P1 资料基础设施价值；**实验主要为预训练，不是 SFT 验证**。
- **核查**：2026-10-29；已读 §3–6 关键结果、限制及仓库核心模块，未复现。

## 原文证据

论文梳理离线混合优化的配置选择，并研究数据域新增、拆分或修订后如何复用已有比例。§3 / Figures 4、6、7 分别讨论代理运行数量、回归器及有限数据约束；§4–5 研究比例复用。§6 说明其分析限于离线优化，部分复用还需判断受影响的数据域。[全文](https://arxiv.org/pdf/2602.12237)

## 局限与本案例关系（调研判断）

它的主要价值是提醒调研仓库保存“当时的域定义和数据版本”，因为同名数据源更新后，旧比例结论可能已不对应新对象。论文标题的 Throughout LM Development 不能被扩大解读为已经验证所有训练阶段。对当前双侧 SFT 回退的解释仍需来自更贴近任务、监督形式和起点的证据。

## 代码核查

[allenai/olmix](https://github.com/allenai/olmix) 的 README 对应论文作者及标题，且[作者机构项目介绍](https://allenai.org/blog/olmix)关联项目。已检查比例生成、拟合和启动模块；[olmix/fit/core.py](https://github.com/allenai/olmix/blob/main/olmix/fit/core.py)实现回归拟合和比例建议接口。

仓库明确标记 WIP、内部基础设施向开源迁移中；已有实质代码，端到端 launch 仍涉及 Beaker 环境和结果导出。GitHub API：**42 stars**，2026-10-29。

