# 论文与技术资料索引

首轮核查日期：**2026-10-29**。共 **51 项资料**，其中 **34 篇形成阅读笔记**，另 **17 项保留初筛记录**。资料包含会议论文、预印本、技术报告及 1 项模型发布说明；不把全部条目统称为已精读论文。

[先读调研总图](../topics/00-research-map.md) · [问题定义](../problem.md) · [机器可读总目录](../resources/paper-catalog.json) · [代码地图](../resources/repository-catalog.md)

## 阅读方式

每篇卡片保留研究对象、原文证据、适用限制、与当前问题的关系以及代码公开范围。优先级是阅读顺序，不是质量评分。正式发表、作者接收声明、投稿和预印本分别记录；以下短标题的完整书目信息见卡片或 JSON。

34 篇笔记中有些是明确标注的摘要／基础实现级阅读，尤其 PG-SFT 全文未取得；不能把笔记数当作全文通读数。所有代码只作静态核查。

## A. 多任务 SFT 与数据混合

| 笔记 | 发表身份 | 阅读深度 | 代码范围 |
| --- | --- | --- | --- |
| [A01 · DiFT：多推理任务的差异化更新](a-01-dift.md) · P0 | ICLR 2026 | 关键正文／表格；详见卡片 | 作者仓库未核实 |
| [A02 · Data Mixing Optimization](a-02-data-mixing-optimization.md) · P0 | ICML 2025 (PMLR 267) | 关键正文／表格；详见卡片 | [采样、训练封装与权重求解](https://github.com/realYuanLi/Data-Mixing-Optimization) |
| [A03 · DynamixSFT](a-03-dynamixsft.md) · P0 | Findings of ACL 2026 | 关键正文／表格；详见卡片 | 作者仓库未核实 |
| [A04 · Data Composition / DMT](a-04-dmt-data-composition.md) · P0 | ACL 2024 Main Conference (Long Papers) | 关键正文／表格；详见卡片 | 作者仓库未核实 |
| [A05 · CoBa](a-05-coba.md) · P0 | EMNLP 2024 Main Conference | 关键正文／表格；详见卡片 | [MFTCoder 内实现](https://github.com/codefuse-ai/MFTCoder) |
| [A06 · mSFT](a-06-msft.md) · P0 | arXiv v7；作者称 AACL 2026 Oral，正式论文集未核实 | 关键正文／表格；详见卡片 | [训练／回退流水线；版本待对齐](https://github.com/reiss-koh/msft) |
| [A07 · Task Mixture Effects](a-07-task-mixture-effects.md) · P1 | Findings of ACL 2026 | 关键正文／表格；详见卡片 | 作者仓库未核实 |
| [A08 · DoGE](a-08-doge.md) · P2 | ICML 2024 (PMLR 235) | 关键正文／表格；详见卡片 | [作者训练实现](https://github.com/Olivia-fsm/DoGE) |
| [A09 · RegMix](a-09-regmix.md) · P2 | ICLR 2025 | 关键正文／表格；详见卡片 | [作者实现；评测 WIP 标记](https://github.com/sail-sg/regmix) |
| [A10 · Olmix](a-10-olmix.md) · P1 | ICML 2026 (PMLR 306) | 关键正文／表格；详见卡片 | [混合管线；开源迁移 WIP](https://github.com/allenai/olmix) |

## B. Agent 轨迹与能力保持

| 笔记 | 发表身份 | 阅读深度 | 代码范围 |
| --- | --- | --- | --- |
| [B01 · AgentTuning](b-01-agenttuning.md) · P1 | Findings of ACL 2024 | 关键正文／表格；详见卡片 | [部署／评测；主训练入口未核实](https://github.com/THUDM/AgentTuning) |
| [B02 · Agent-FLAN](b-02-agent-flan.md) · P1 | Findings of ACL 2024 | 关键正文／表格；详见卡片 | [模型／数据；未见主训练源码](https://github.com/InternLM/Agent-FLAN) |
| [B03 · AgentBank](b-03-agentbank.md) · P1 | Findings of EMNLP 2024 | 关键正文／表格；详见卡片 | 作者数据；GitHub 未核实 |
| [B04 · AgentGym](b-04-agentgym.md) · P2 | ACL 2025 Main Conference Long Papers | 关键正文／表格；详见卡片 | [行为克隆源码](https://github.com/WooooDyy/AgentGym) |
| [B05 · AgentRefine](b-05-agentrefine.md) · P1 | ICLR 2025 | 关键正文／表格；详见卡片 | [README／图片／数据](https://github.com/Fu-Dayuan/AgentRefine) |
| [B06 · Agent-R](b-06-agent-r.md) · P2 | arXiv | 关键正文／表格；详见卡片 | [采集／修正；归档](https://github.com/ByteDance-Seed/Agent-R) |
| [B07 · SWE-Gym](b-07-swe-gym.md) · P1 | ICML 2025 / PMLR 267 | 关键正文／表格；详见卡片 | [训练／验证器配置与脚本](https://github.com/SWE-Gym/SWE-Gym) |
| [B08 · SWE-smith](b-08-swe-smith.md) · P1 | NeurIPS 2025 Datasets and Benchmarks Track | 关键正文／表格；详见卡片 | [任务构造／轨迹／训练入口](https://github.com/SWE-bench/SWE-smith) |
| [B09 · PG-SFT](b-09-pg-sft.md) · P2 | arXiv | 原始摘要；全文未取得 | 作者代码未核实 |
| [B10 · Selecting Long-Horizon Trajectories](b-10-terminal-supervision-horizon.md) · P2 | 2026 预印本；投稿 ICLR 2027 | 关键正文／表格；详见卡片 | 作者代码未核实 |

## C. 监督表达、更新目标与参数保持

| 笔记 | 发表身份 | 阅读深度 | 代码范围 |
| --- | --- | --- | --- |
| [C01 · DFT：SFT 泛化与概率加权](c-01-dft.md) · P1 | ICLR 2026 Conference | 关键正文／表格；详见卡片 | [核心 trainer／loss](https://github.com/yongliang-wu/DFT) |
| [C02 · Proximal SFT](c-02-psft.md) · P1 | ICLR 2026 Conference | 关键正文／表格；详见卡片 | [trainer／ratio clipping](https://github.com/zwhong714/PSFT) |
| [C03 · iw-SFT](c-03-iw-sft.md) · P1 | arXiv 2025；正式会议未核实 | 关键正文／表格；详见卡片 | [IwSFTLoss](https://github.com/emmyqin/iw_sft) |
| [C04 · Qwen3 Technical Report](c-04-qwen3-thinking-fusion.md) · P1 | 官方技术报告／arXiv 2025 | 关键正文／表格；详见卡片 | [模型／推理；原始训练未开放核实](https://github.com/QwenLM/Qwen3) |
| [C05 · Long-Short CoT Mixture SFT](c-05-ls-mixture.md) · P2 | arXiv 2025；正式会议未核实 | 关键正文／表格；详见卡片 | [项目网站，非训练实现](https://github.com/ZGCA-AI4Edu/LS-Mixture) |
| [C06 · Nemotron 3 Super](c-06-nemotron3-super.md) · P1 | 官方技术报告／arXiv 2026 | 关键正文／表格；详见卡片 | [SFT recipe；生产完整性待核](https://github.com/NVIDIA-NeMo/Nemotron) |
| [C07 · Lottery Ticket Adaptation](c-07-lota.md) · P2 | arXiv 2024；正式会议未核实 | 关键正文／表格；详见卡片 | [mask／连续训练；WIP](https://github.com/kiddyboots216/lottery-ticket-adaptation) |
| [C08 · HFT](c-08-hft.md) · P2 | ACL 2025 Long Papers | 关键正文／表格；详见卡片 | 作者代码未核实 |
| [C09 · LoRA Learns Less and Forgets Less](c-09-lora-learning-forgetting.md) · P2 | Transactions on Machine Learning Research | 关键正文／表格；详见卡片 | [checkpoint／配方](https://github.com/danbider/lora-tradeoffs) |

## R. 多任务优化基础与工程补充

| 笔记 | 发表身份 | 阅读深度 | 代码范围 |
| --- | --- | --- | --- |
| [R01 · MFTCoder](r-01-mftcoder.md) · P1 | KDD 2024 | 关键正文／表格；详见卡片 | [多任务 loss 与 trainer](https://github.com/codefuse-ai/MFTCoder) |
| [R02 · PCGrad](r-02-pcgrad.md) · P2 | NeurIPS 2020 | 问题定义＋作者实现 | [作者 TensorFlow 1 实现](https://github.com/tianheyu927/PCGrad) |
| [R03 · CAGrad](r-03-cagrad.md) · P2 | NeurIPS 2021 | 摘要／目标＋示例实现 | [toy／视觉／MTRL](https://github.com/Cranial-XIX/CAGrad) |
| [R04 · GradNorm](r-04-gradnorm.md) · P2 | ICML 2018 | 正式摘要＋第三方实现 | [LibMTL 第三方实现](https://github.com/median-research-group/LibMTL) |
| [R05 · Multi-Task Transfer Matters During Instruction-Tuning](r-05-instruction-transfer.md) · P2 | Findings ACL 2024 | 正式摘要＋目录 | [README／LICENSE 占位](https://github.com/davidandym/Multitask-Transfer-Instruction-Tuning) |

## 初筛资料：17 项

以下未形成独立阅读卡。保留它们是为了标记相关路线、阶段边界和后续阅读入口，不据此补出未核实的方法或结果。

| ID / 资料 | 发表或发布身份 | 与本问题的关系 |
| --- | --- | --- |
| A11 · [DoReMi](https://proceedings.neurips.cc/paper_files/paper/2023/hash/dcba6be91359358c2355cd920da3fcbd-Abstract-Conference.html) | NeurIPS 2023 Main Conference | 经典预训练域重加权背景，非直接SFT证据 |
| A12 · [Skill-It!](https://proceedings.neurips.cc/paper_files/paper/2023/hash/70b8505ac79e3e131756f793cd80eb8d-Abstract-Conference.html) | NeurIPS 2023 Main Conference | 技能依赖/数据选择背景；skills不等同SkillsBench |
| A13 · [Data Mixing Laws](https://proceedings.iclr.cc/paper_files/paper/2025/hash/cc84bfabe6389d8883fc2071c848f62a-Abstract-Conference.html) | ICLR 2025 | 预训练损失预测与比例选择，非直接SFT证据 |
| A14 · [AutoScale](https://openreview.net/pdf?id=rujwIvjooA) | COLM 2025 | 预训练最优比例的尺度依赖边界 |
| A15 · [Demystifying Instruction Mixing](https://aclanthology.org/2024.acl-srw.15/) | ACL 2024 Student Research Workshop | 直接指令混合；与DMT研究问题相邻 |
| A16 · [Rethinking Data Mixing](https://aclanthology.org/2026.acl-short.28/) | ACL 2026 Main Conference (Short Papers) | 域定义及梯度动态；摘要设置为GPT-2预训练相关，SFT适用性未核实 |
| B11 · [AgentOhana](https://arxiv.org/abs/2402.15506) | arXiv / LLMAgents@ICLR 2024 workshop | 多源Agent轨迹统一格式和数据管线相邻；不直接覆盖业务双降，暂不单独深读 |
| B12 · [OpenHands LM 32B 发布](https://www.openhands.dev/blog/introducing-openhands-lm-32b-a-strong-open-coding-agent-model) | Official model release | SWE-Gym后续模型资产；与SWE-Gym去重，不作为新的混训保持证据 |
| B13 · [OpenHands](https://proceedings.iclr.cc/paper_files/paper/2025/hash/a4b6ad6b48850c0c331d1259fc66a69c-Abstract-Conference.html) | ICLR 2025 | 运行接口与评测基础设施；不等同离线SFT训练方法，留给通用基础设施整理 |
| B14 · [SWE-agent](https://papers.nips.cc/paper_files/paper/2024/hash/5a7c947568c1b1328ccc5230172e1e7c-Abstract-Conference.html) | NeurIPS 2024 Main Conference | 接口设计影响Agent表现；非离线轨迹SFT或两目标混训退化的主研究 |
| B15 · [Agent Q](https://arxiv.org/abs/2408.07199) | arXiv | 行为克隆局限的相邻背景；主方法涉及搜索与DPO，本轮不扩展RL/偏好优化综述 |
| C10 · [Next-Chunk Reasoning RL 与 Mixed SFT](https://aclanthology.org/2026.emnlp-main.921/) | EMNLP 2026 Main Conference | no-CoT+长CoT Mixed SFT 相邻；需区分 SFT 即时表现与后续 RLVR 结果 |
| C11 · [Rho-1：Not All Tokens Are What You Need](https://proceedings.neurips.cc/paper_files/paper/2024/hash/3322a9a72a1707de14badd5e552ff466-Abstract-Conference.html) | NeurIPS 2024 | 预训练中的 token 选择；不能直接外推成异构 SFT 解决方案 |
| C12 · [LIMA](https://papers.neurips.cc/paper_files/paper/2023/hash/ac662d74829e4407ce1d126477f4a03a-Abstract-Conference.html) | NeurIPS 2023 | 精选少量监督与已有能力表达的经典背景；不覆盖现代长 Agent/SQL 双降 |
| C13 · [SFT Memorizes, RL Generalizes](https://proceedings.mlr.press/v267/chu25c.html) | ICML 2025 / PMLR 267 | 文本/视觉 OOD 下 SFT 与 RL 的比较；标题不是所有 SFT 的普适定理 |
| C14 · [RFT Mitigates Forgetting](https://proceedings.mlr.press/v306/lai26h.html) | ICML 2026 / PMLR 306 | 多模态连续后训练中的 RFT 保持性；不直接覆盖本轮纯 SFT 同批混训 |
| C15 · [SFT-as-Context](https://arxiv.org/abs/2610.11132) | arXiv 2026；正式会议未核实 | 训练后将 SFT 输出作为 parent 模型上下文的组合；不满足同一混训 checkpoint 的核心要求 |

## 需要避免的混淆

- **DiFT 与 DFT** 是不同论文；**长短 CoT** 不等于 **有／无 CoT**，长程交互也不自动等于长 CoT。
- 预训练混合、连续微调、同时混训和后续 RL 分别记录；总体均值、域外保持、SQL 文本指标和执行成功率不能互换。
- 作者仓库未核实保留为空；项目网站和数据发布不会补记为训练代码。具体实现检查见代码地图及各卡。

分线来源登记：[A](../resources/a-source-register.json) · [B](../resources/b-source-register.json) · [C](../resources/c-source-register.json) · [补充](../resources/root-source-register.json)。完整检索口径见[核查说明](../resources/search-and-verification.md)。
