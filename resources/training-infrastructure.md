# 训练实现与多任务参考库

核查日期：**2026-10-29**。星数为 GitHub API 快照，表示关注度，不表示对本项目的效果。已静态核查 README 与下列关键文件；未安装或执行，也未核实与用户当前模型检查点的兼容性。

[返回资源索引](README.md) · [问题定义](../problem.md)

## 高关注度的 SFT 基础设施

| 仓库 | Stars | 已核查入口 | 与调研的关系 |
| --- | ---: | --- | --- |
| [LlamaFactory](https://github.com/hiyouga/LlamaFactory) | 75,773 | [数据参数](https://github.com/hiyouga/LlamaFactory/blob/47d407d2aabbd52e11d4f34c7a06b9de3e2f0a77/src/llamafactory/hparams/data_args.py)、[监督处理](https://github.com/hiyouga/LlamaFactory/blob/47d407d2aabbd52e11d4f34c7a06b9de3e2f0a77/src/llamafactory/data/processor/supervised.py)、[trainer](https://github.com/hiyouga/LlamaFactory/blob/47d407d2aabbd52e11d4f34c7a06b9de3e2f0a77/src/llamafactory/train/sft/trainer.py) | 数据合并/交错、历史轮次 mask、截断与损失入口 |
| [TRL](https://github.com/huggingface/trl) | 19,628 | [SFTConfig](https://github.com/huggingface/trl/blob/008cf575b389e2839b632dc3333416eafee8d337/trl/trainer/sft_config.py)、[官方文档](https://huggingface.co/docs/trl/sft_trainer) | completion-only、assistant-only、packing 与 DFT 的含义 |
| [ms-swift](https://github.com/modelscope/ms-swift) | 15,985 | [Agent loss scale](https://github.com/modelscope/ms-swift/blob/b02a4f7ad0704d4b20a4ac94621c5ac442284b5f/swift/loss_scale/agent.py)、[causal LM loss](https://github.com/modelscope/ms-swift/blob/b02a4f7ad0704d4b20a4ac94621c5ac442284b5f/swift/loss/causal_lm.py) | Agent 片段权重与有效监督分母 |
| [Axolotl](https://github.com/axolotl-ai-cloud/axolotl) | 12,645 | [DFT loss](https://github.com/axolotl-ai-cloud/axolotl/blob/93575ed6353a531df8dfbee913f21eca77194e1e/src/axolotl/monkeypatch/loss/dft.py) | DFT 的框架实现，可对照论文与代码 |
| [Open Instruct](https://github.com/allenai/open-instruct) | 3,916 | [OLMo-core SFT](https://github.com/allenai/open-instruct/blob/64beb76c3efe2b600f39922ef83b5ea44669ea61/open_instruct/olmo_core_finetune.py) | 开放后训练工程的数据转换、文档边界与 SFT 入口 |

LlamaFactory 旧名链接在核查时返回迁移信息，本表使用 API 确认的当前名称。

## 已确认的实现区别

**LlamaFactory：**数据参数区分 concat 与多种 interleave 策略；监督处理按 train_on_prompt、mask_history 构造标签并处理长度预算，mask_history 相关分支只训练最后轮次。这是代码行为记录，不能据此判断用户配置。

**TRL：**官方文档区分 completion-only 与 assistant-only，后者依赖相应聊天模板标记；SFTConfig 区分 NLL 与 DFT。因此，同一个样本配比在不同预处理与 mask 下未必代表同样的监督。

**ms-swift：**Agent loss scale 模块含 Agent-FLAN、ReAct、Qwen 等映射类。causal LM loss 的普通分支对逐 token 损失乘以 `loss_scale`，再除以调用方提供的 `num_items_in_batch`；缺省时使用移位后未被 `-100` 屏蔽的标签数，而非权重之和。sequence-parallel 分支依赖 trainer 预先处理。

**Axolotl：**DFT 文件可见概率权重的 detach 与未屏蔽 token 分母。这说明实质实现已存在，不代表该方法已在当前业务场景验证。

## 多任务研究实现

| 项目 | Stars | 代码状态 |
| --- | ---: | --- |
| [MFTCoder](https://github.com/codefuse-ai/MFTCoder) | 711 | 作者代码，有任务权重、损失与训练入口，后续版本含 CoBa；见 [R01](../papers/r-01-mftcoder.md) |
| [LibMTL](https://github.com/median-research-group/LibMTL) | 2,597 | 实际含 GradNorm、PCGrad、CAGrad；是这些方法的第三方参考实现 |
| [PCGrad](https://github.com/tianheyu927/PCGrad) | 374 | 作者实现为 TensorFlow 1 风格优化器，不标成现代 LLM trainer |
| [CAGrad](https://github.com/Cranial-XIX/CAGrad) | 161 | 有 toy、视觉多任务与多任务 RL 实现，不标成即用型 LLM 工程 |

## 阅读这些实现的目的

**我们的分析：**“混合”需要拆成样本抽取、监督范围、token/样本/任务级汇聚和参数更新。只看混合比例或库名，无法确定优化目标。

代码身份、功能存在、运行成功与业务有效是不同证据层次。本轮仅完成前两类静态核查。原始快照及源码标识见 [root-repository-metadata.json](root-repository-metadata.json)。
