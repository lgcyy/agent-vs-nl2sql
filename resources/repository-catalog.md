# 代码与资源地图

核查日期：**2026-10-29**。本轮索引 **37 个主要项目**，同时保留对应论文、作者归属链、实际资产范围和元数据。Stars 为当日 GitHub API 快照；不代表论文质量、可运行性或对当前业务的有效性。

[返回资源索引](README.md) · [训练实现细节](training-infrastructure.md) · [机器可读元数据](repository-metadata.json)

代码公开程度按已看到的文件描述：作者仓库不自动等于训练代码，训练入口存在不自动等于完整复现。本轮没有执行项目或安装训练依赖。

## 1. SFT 混合、更新目标与参数适配

| 项目 | Stars | 实际核查范围 | 状态与最近推送 |
| --- | ---: | --- | --- |
| [codefuse-ai/MFTCoder](https://github.com/codefuse-ai/MFTCoder) | 711 | [任务级 loss、trainer、CoBa 配置](../papers/a-05-coba.md) | 已读核心源码；CoBa 属后续工作；2024-12-30 |
| [realYuanLi/Data-Mixing-Optimization](https://github.com/realYuanLi/Data-Mixing-Optimization) | 0 | [混合采样、SFT 启动、校准、SLSQP 权重求解](../papers/a-02-data-mixing-optimization.md) | 已读核心权重求解；训练封装依赖 LLaMA-Factory；2025-11-11 |
| [reiss-koh/msft](https://github.com/reiss-koh/msft) | 14 | [分阶段训练、过拟合分析、checkpoint 回退](../papers/a-06-msft.md) | 已读流水线；代码推送早于论文 v7；2026-03-24 |
| [yongliang-wu/DFT](https://github.com/yongliang-wu/DFT) | 928 | [目标 token 概率加权与 detach 的 trainer](../papers/c-01-dft.md) | 已读核心源码；Dynamic Fine-Tuning，非 DiFT；2026-08-01 |
| [zwhong714/PSFT](https://github.com/zwhong714/PSFT) | 40 | [示范更新、旧概率、ratio clipping](../papers/c-02-psft.md) | 已读 trainer／loss；未验证当前混训场景；2025-09-09 |
| [emmyqin/iw_sft](https://github.com/emmyqin/iw_sft) | 28 | [IwSFTLoss、参考概率与聚合选项](../papers/c-03-iw-sft.md) | 已读核心源码；精选数据假设需保留；2025-07-18 |
| [NVIDIA-NeMo/Nemotron](https://github.com/NVIDIA-NeMo/Nemotron) | 2,235 | [Super SFT recipe、入口及 YAML](../papers/c-06-nemotron3-super.md) | 已读源码；完整生产两阶段切换未核实；2026-10-28 |
| [kiddyboots216/lottery-ticket-adaptation](https://github.com/kiddyboots216/lottery-ticket-adaptation) | 40 | [稀疏 mask 与连续训练脚本](../papers/c-07-lota.md) | 已读核心文件；README 标 WIP；2024-11-20 |

## 2. Agent 轨迹、训练资产与运行平台

| 项目 | Stars | 实际核查范围 | 状态与最近推送 |
| --- | ---: | --- | --- |
| [THUDM/AgentTuning](https://github.com/THUDM/AgentTuning) | 1,509 | [部署、评测、模型与数据入口](../papers/b-01-agenttuning.md) | 未核实主混训训练入口；2023-10-31 |
| [InternLM/Agent-FLAN](https://github.com/InternLM/Agent-FLAN) | 363 | [作者模型／数据发布、README 与文档图片](../papers/b-02-agent-flan.md) | 所查递归目录未见主训练源码；2024-03-22 |
| [WooooDyy/AgentGym](https://github.com/WooooDyy/AgentGym) | 853 | [跨环境数据与行为克隆 trainer](../papers/b-04-agentgym.md) | 已读角色标签、mask 和截断源码；2026-05-30 |
| [Fu-Dayuan/AgentRefine](https://github.com/Fu-Dayuan/AgentRefine) | 22 | [README、图片与作者数据链接](../papers/b-05-agentrefine.md) | 所查目录未见完整合成／训练实现；2025-11-22 |
| [ByteDance-Seed/Agent-R](https://github.com/ByteDance-Seed/Agent-R) | 175 | [轨迹采集、修正、评测脚本](../papers/b-06-agent-r.md) | 已读路径构造；依赖 XTuner；已归档；2025-10-20 |
| [SWE-Gym/SWE-Gym](https://github.com/SWE-Gym/SWE-Gym) | 754 | [策略／验证器训练脚本、配置与轨迹资产](../papers/b-07-swe-gym.md) | 已读训练配置；依赖特定 OpenHands fork；2025-07-29 |
| [SWE-bench/SWE-smith](https://github.com/SWE-bench/SWE-smith) | 818 | [任务合成、轨迹处理及训练调用](../papers/b-08-swe-smith.md) | 已读关键源码；依赖 SWE-agent／外部训练库；2026-10-26 |
| [OpenHands/OpenHands](https://github.com/OpenHands/OpenHands) | 92,179 | Agent 运行平台，论文关联与元数据 | 初筛基础设施；没有展开实现评审；2026-10-29 |
| [SWE-agent/SWE-agent](https://github.com/SWE-agent/SWE-agent) | 20,682 | Agent 接口与运行平台 | 初筛基础设施；没有展开实现评审；2026-10-26 |

## 3. 预训练混合与相邻背景

| 项目 | Stars | 实际核查范围 | 状态与最近推送 |
| --- | ---: | --- | --- |
| [Olivia-fsm/DoGE](https://github.com/Olivia-fsm/DoGE) | 21 | [域权重、训练与代理泛化相关实现](../papers/a-08-doge.md) | 已查作者代码；主要为预训练；2024-02-29 |
| [sail-sg/regmix](https://github.com/sail-sg/regmix) | 217 | [代理数据混合回归与评测入口](../papers/a-09-regmix.md) | 已查关键入口；README 评测仍标 WIP；2025-02-17 |
| [allenai/olmix](https://github.com/allenai/olmix) | 42 | [数据混合管线与域映射](../papers/a-10-olmix.md) | 已查关键入口；开源迁移 WIP；2026-05-26 |
| [sangmichaelxie/doremi](https://github.com/sangmichaelxie/doremi) | 361 | 预训练域重加权作者仓库 | 初筛 README；未深读核心源码；2023-12-26 |
| [HazyResearch/skill-it](https://github.com/HazyResearch/skill-it) | 49 | 技能选择与课程说明 | 初筛 README；技能图学习代码未提供；2023-10-31 |
| [yegcjs/mixinglaws](https://github.com/yegcjs/mixinglaws) | 113 | 数据混合损失拟合与运行说明 | 初筛 README；未深读核心源码；2025-07-15 |
| [feiyang-k/AutoScale](https://github.com/feiyang-k/AutoScale) | 14 | 尺度相关数据混合说明与示例 | 初筛 README；未审计核心实现；2025-08-08 |
| [microsoft/rho](https://github.com/microsoft/rho) | 474 | Rho-1 候选关联仓库、API 元数据 | 作者归属链与训练源码未核实；2024-04-18 |

## 4. 论文发布、模型资产与项目站点

| 项目 | Stars | 实际核查范围 | 状态与最近推送 |
| --- | ---: | --- | --- |
| [davidandym/Multitask-Transfer-Instruction-Tuning](https://github.com/davidandym/Multitask-Transfer-Instruction-Tuning) | 1 | [作者 README 与 LICENSE](../papers/r-05-instruction-transfer.md) | 递归目录为占位发布，未见训练实现；2024-05-28 |
| [QwenLM/Qwen3](https://github.com/QwenLM/Qwen3) | 27,694 | [模型、推理及训练框架说明](../papers/c-04-qwen3-thinking-fusion.md) | 不等于原始 thinking fusion 生产管线开放；2026-01-09 |
| [ZGCA-AI4Edu/LS-Mixture](https://github.com/ZGCA-AI4Edu/LS-Mixture) | 10 | [Vue／Vite 项目站点与方法展示](../papers/c-05-ls-mixture.md) | 未见训练实现；2025-05-10 |
| [danbider/lora-tradeoffs](https://github.com/danbider/lora-tradeoffs) | 22 | [checkpoint 与配方说明](../papers/c-09-lora-learning-forgetting.md) | 所读目录未见训练源码；2024-09-27 |

## 5. 多任务优化基础实现

| 项目 | Stars | 实际核查范围 | 状态与最近推送 |
| --- | ---: | --- | --- |
| [Cranial-XIX/CAGrad](https://github.com/Cranial-XIX/CAGrad) | 161 | [toy、视觉多任务、多任务 RL](../papers/r-03-cagrad.md) | 已读演示实现；非即用 LLM 工程；2023-11-09 |
| [median-research-group/LibMTL](https://github.com/median-research-group/LibMTL) | 2,597 | [GradNorm／PCGrad／CAGrad PyTorch 实现](../papers/r-04-gradnorm.md) | 已读相关类；第三方参考，不冒充原作者代码；2025-05-14 |
| [tianheyu927/PCGrad](https://github.com/tianheyu927/PCGrad) | 374 | [作者 TensorFlow 1 风格优化器](../papers/r-02-pcgrad.md) | 已读 PCGrad_tf.py；非现代 LLM trainer；2020-04-28 |

## 6. 通用 SFT 基础设施

| 项目 | Stars | 实际核查范围 | 状态与最近推送 |
| --- | ---: | --- | --- |
| [huggingface/trl](https://github.com/huggingface/trl) | 19,628 | [SFTConfig、assistant／completion mask、DFT](training-infrastructure.md) | 已读官方文档与配置源码；2026-10-29 |
| [modelscope/ms-swift](https://github.com/modelscope/ms-swift) | 15,985 | [Agent 片段 loss scale、causal LM loss](training-infrastructure.md) | 已读源码；分母不是 loss_scale 之和；2026-10-29 |
| [axolotl-ai-cloud/axolotl](https://github.com/axolotl-ai-cloud/axolotl) | 12,645 | [DFT loss 的 detach 与分母](training-infrastructure.md) | 已读核心文件；2026-10-29 |
| [allenai/open-instruct](https://github.com/allenai/open-instruct) | 3,916 | [开放后训练的数据转换与 SFT 入口](training-infrastructure.md) | 已读 OLMo-core fine-tune 入口；2026-10-29 |
| [hiyouga/LlamaFactory](https://github.com/hiyouga/LlamaFactory) | 75,773 | [采样参数、监督 mask、packing、SFT trainer](training-infrastructure.md) | 已读关键源码；使用 API 确认的迁移后名称；2026-10-29 |

## 归属和版本说明

- OpenHands 的论文旧链接为 All-Hands-AI/OpenHands，API 已确认当前 canonical 仓库为 OpenHands/OpenHands；LlamaFactory 也使用迁移后名称。
- MFTCoder 同时关联 MFTCoder 与 CoBa，按一个仓库统计；星数不能分别算作两篇论文各自的关注度。
- LibMTL 是经典算法的第三方参考实现。Rho-1 仅保存候选项目和元数据，作者归属链尚未补齐。
- AgentBank 已确认论文指定的 [Hugging Face 数据](https://huggingface.co/datasets/Solaris99/AgentBank)，没有用第三方同名 GitHub 填空。DiFT、DynamixSFT、DMT、HFT、PG-SFT 等未核实专门作者仓库的条目，保留在论文索引；“未核实”不等于“没有发布”。
- 元数据中的最近推送只是活动时间，不保证持续维护或全部功能可用。部分源码已记录 commit／blob；具体证据链见论文卡和 JSON。

## 如何使用这张地图

阅读训练目标时，优先查看 MFTCoder、DFT、PSFT、iw-SFT 和 Nemotron 的具体入口；阅读 Agent 轨迹处理时，优先查看 AgentGym、SWE-Gym、SWE-smith、Agent-R。通用框架用于核对 mask、采样和 loss 汇聚的实际含义，不能用框架支持某算法替代对论文方法的理解。不同项目未经统一运行比较，这里不作效果排行榜。
