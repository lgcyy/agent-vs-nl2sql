# A01 · DiFT：多推理任务的参数干扰

- **论文**：Differential Fine-Tuning Large Language Models Towards Better Diverse Reasoning Abilities，Yuan et al.
- **发表**：ICLR 2026 正式会议论文；以[官方 proceedings](https://proceedings.iclr.cc/paper_files/paper/2026/hash/b304a04c6a4ac168aa692923814ace17-Abstract-Conference.html)为准。
- **优先级 / 范围**：P0；直接涉及多任务 SFT，任务组合与本仓库不同。
- **核查**：2026-10-29 初读；2026-10-30 复核 Table 1／2 原始 PDF 截图，未做复现。

## 原文证据

问题是多种推理能力混合或连续 SFT 后难以保持单训表现。DiFT 根据初始模型与任务微调模型的激活差异识别参数行，再按任务组合区别更新。§3 / Table 1 呈现协同与干扰并存；§4 说明方法；§5 / Table 2 主要用目标任务平均分 ATA 汇总改进。[论文全文](https://proceedings.iclr.cc/paper_files/paper/2026/file/b304a04c6a4ac168aa692923814ace17-Paper-Conference.pdf)

附录 B.6 给出**连续数学→逻辑训练**的格式污染个案。它提示评分下降可能包含输出协议变化，但不是对所有退化的证明，也不是本案例混训原因的证据。同一全文的 §5.3 与 Table 2 标注说明了该个案的适用设置。

### 补充：一个数值上双侧低于单训与 base 的组合

正式 PDF 第 4 页 Table 1 的 **Mistral-7B、Mix-Code-Logic** 报告如下；保留原文指标名和数值，不把 xGLUE 列改写为 SQL 或 Agent 成功率。

| 比较对象 | 代码侧（原文 xGLUE 列） | 逻辑侧（LogiQA2 列） |
| --- | --- | --- |
| 共同 base | 1.2449 | 31.87 |
| 对应任务单训：Code-only／Logic-only | 1.7146 | 31.23 |
| 同一个 Mix-Code-Logic 模型 | 1.2342 | 31.17 |
| 加 DiFT（第 8 页 Table 2） | 1.2555 | 32.51 |

**本仓库按表内数值所作比较：**混训两列均低于对应单训与共同 base；Table 2 的 DiFT 结果回到两列 base 之上，但代码侧仍低于 Code-only。这比单纯“混训低于单训”的描述更贴近当前观察，不能再仅因作者实现／引用未核全而把它降为一般背景。

**证据限制：**Table 1 的部分上升箭头与相对 base 的数值矛盾；混训代码侧对 base、逻辑侧对单训的差距很小，该行没有给出误差范围。这里是原表数值的谨慎重读，不是稳健普遍双降、因果机制或业务复现的证明。Logic-only 本身也低于逻辑侧 base，不能描述成“两个单训都优于 base”。

## 局限与本案例关系（调研判断）

这是最接近“混训低于对应单训”的近期工作之一；它有助于把共享参数干扰与数据组织区分开。其数学、代码、逻辑、常识结果不覆盖 NL2SQL-noCoT 加长程交互轨迹；平均分结论也不等于两个目标域都高于共同起点。不能把本文识别的参数关联直接当作当前业务模型的因果机制。

## 代码核查

截至核查日，检查了官方论文页、全文链接及 GitHub 仓库检索，**作者公开 GitHub 仓库未核实**。官方页面提供 Supplemental 入口，本轮未核实附件中是否包含可运行实现；不能登记为“无代码”，也不能把同名图像特征 DiFT 或 token-loss DFT 的代码链接进来。Stars：未核实。

