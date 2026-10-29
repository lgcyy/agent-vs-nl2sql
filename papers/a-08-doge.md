# A08 · DoGE：面向目标域泛化的数据重加权

- **论文**：DOGE: Domain Reweighting with Generalization Estimation，Fan, Pagliardini & Jaggi。
- **发表**：ICML 2024，PMLR 235:12895–12915；[正式论文页](https://proceedings.mlr.press/v235/fan24e.html)。正文阅读 [arXiv v2](https://arxiv.org/html/2310.15393v2)（2024-02-05）；与最终会议版未逐段比对。
- **优先级 / 范围**：P2；**预训练证据，不能直接当作 SFT 结论**。
- **核查**：2026-10-29；已读 §2–4、主结果与代码实现；未复现。

## 原文证据

DoGE 用代理模型估计训练域梯度对目标域损失的贡献，再把学习到的平均比例用于更大的模型。§3.1 / Table 1 的平均 PPL 最好，但仅七域中的五域优于均匀基线；Table 2 报告下游评测。§4 / Appendix E 中，所比较的阶段变化比例没有显示优于全程平均比例的总体收益。[正文](https://arxiv.org/html/2310.15393v2)

## 局限与本案例关系（调研判断）

它提供“训练数据属于哪个域”和“优化哪个目标域”应分开描述的概念。平均损失改善不代表逐域保持，动态方法也不天然优于静态方法。预训练代理模型的梯度与比例迁移，不足以说明已有 27B 模型上 SQL 和长程轨迹的联合 SFT 应呈现同样关系。

## 代码核查

[Olivia-fsm/DoGE](https://github.com/Olivia-fsm/DoGE) 的仓库描述对应论文；[作者主页](https://olivia-fsm.github.io/)确认该 GitHub Pages 账号为 Simin Fan，并列出论文。代码包含代理训练、域外目标设置与最终模型训练脚本。已读 [src/trainer.py](https://github.com/Olivia-fsm/DoGE/blob/master/src/trainer.py)，含 DoGETrainer、分域梯度及权重更新，不是占位仓库。

GitHub API：**21 stars**，2026-10-29。训练与数据设置面向原论文预训练流程，未验证适配现代 SFT 框架。

