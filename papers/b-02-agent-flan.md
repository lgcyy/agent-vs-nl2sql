# Agent-FLAN：格式、推理内容与工具使用监督的区别

- 编号：B02
- 作者与年份：Zehui Chen 等，2024。
- 发表状态：[Findings of ACL 2024](https://aclanthology.org/2024.findings-acl.557/)。
- 阅读深度：原文 §3—5、表4；作者 README 与递归目录核查，未运行代码。
- 关联专题：[Agent 轨迹与能力保持](../topics/02-agent-trajectories-and-retention.md)
- 核查日期：2026-10-29

## 问题、做法与有证据的结论

作者区分 Agent 数据中的格式生成、推理、工具选择和参数理解，观察这些部分的学习速度不同；其做法包括转换为对话形式、调整能力组成，以及加入不应调用工具的负例。[原文 §3—4、图2—4](https://aclanthology.org/2024.findings-acl.557.pdf)

§5.2、表4以 general-data 模型为对照，报告多项通用得分接近或略升；7B 的 MMLU 为 50.0→49.7，因此不能概括为“每项通用能力都提高”。该表支持有限范围的兼容性，不构成全面能力保持保证。[原文 §5.2、表4](https://aclanthology.org/2024.findings-acl.557.pdf)

## 局限与本仓库解读

作者观察到的损失与格式差异，为区分监督结构提供依据；它并未证明当前混训双降的原因就是格式干扰。SQL 直出、工具参数、自然语言推理和工具观察应分别描述，不能统称“短答案与长思维链”。

本研究属于 Agent 适配及副作用控制的近邻工作。其训练数据、Llama-2 系列起点和通用评测，与本仓库确认的 NL2SQL-noCoT＋长程 Agent 两训练目标比较不同。

## 代码与公开资产

- 论文直接链接 [InternLM/Agent-FLAN](https://github.com/InternLM/Agent-FLAN)，作者来源链已核实。
- [README](https://github.com/InternLM/Agent-FLAN/blob/main/README.md)提供模型、数据和所依赖项目入口。
- 本轮[递归目录核查](https://api.github.com/repos/InternLM/Agent-FLAN/git/trees/main?recursive=1)仅见 README、许可证及图片文档资产，没有主训练或数据构造源码。应标为“作者数据/模型发布仓库，训练实现未在该仓库核实”，不能仅凭论文中的 code 链接写成实现齐全。

## 来源

1. [正式发表记录](https://aclanthology.org/2024.findings-acl.557/)
2. [正式论文 PDF：§3—5、表4](https://aclanthology.org/2024.findings-acl.557.pdf)
3. [作者 README](https://github.com/InternLM/Agent-FLAN/blob/main/README.md)

