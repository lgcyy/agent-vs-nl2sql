# A05 · CoBa：不同任务的收敛速度与损失平衡

- **论文**：CoBa: Convergence Balancer for Multitask Finetuning of Large Language Models，Gong et al.
- **发表**：EMNLP 2024 主会，2024-11；[正式论文页](https://aclanthology.org/2024.emnlp-main.459/)。
- **优先级 / 范围**：P0；直接研究多任务 LLM 微调的收敛平衡。
- **核查**：2026-10-29；已读方法、主结果、§7 限制和实现，未复现。

## 原文证据

CoBa 根据任务验证损失的相对收敛、绝对收敛及发散信号动态调整任务权重。§3 解释方法；§4 / Tables 2–3 给出多任务结果与模型规模比较。**§7 明确：收敛平衡不保证存在任务冲突时所有任务都达到最优表现。**[全文](https://aclanthology.org/2024.emnlp-main.459.pdf)

## 局限与本案例关系（调研判断）

这一区分很重要：多个任务在同一时刻训练不足或训练过度，与共享更新在能力上产生不兼容，并非同一个命题。CoBa 为前一问题提供参考，但当前业务现象尚不能被归因为其中任何一种。验证损失的进展也不能替代 SQL 和交互式 agent 的实际成功指标。

## 代码核查

论文首页直接链接 [codefuse-ai/MFTCoder](https://github.com/codefuse-ai/MFTCoder)，归属已验证。已读 [loss_utils.py](https://github.com/codefuse-ai/MFTCoder/blob/main/mftcoder_accelerate/src/utils/loss_utils.py)：存在 CoBaStatus 与按任务加权的 loss 分支；目录还含 [coba_train_config.json](https://github.com/codefuse-ai/MFTCoder/blob/main/mftcoder_accelerate/src/configs/coba_train_config.json)。这是框架内实现，并非独立占位仓库。

GitHub API：**711 stars**，2026-10-29。该计数属于整个 MFTCoder 仓库，不能解释为 CoBa 单篇论文关注度。MFTCoder 论文与框架条目由仓库其他调研线单独整理。

