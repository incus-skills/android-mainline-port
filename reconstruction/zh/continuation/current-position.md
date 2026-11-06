# 当前观察位置与会话入口

## 当前入口

- 当前遍历：**Pass 1 — 初始结构与语言重构**
- 当前会话检查点：**Checkpoint 01**
- 当前源文件位置：`experience/display.md` 尚待检查。
- 下一步：读取并重构 `experience/display.md`，观察资源归属问题如何在显示子系统中展开。
- 详细的回顾性记录见 [Pass 1 / Checkpoint 01](checkpoints/pass-1-checkpoint-01.md)。

## 遍历编号约定

**Pass（遍历轮次）**表示对已声明范围从起点到终点的一次完整遍历。它不是会话编号，也不是单个文档的审查次数。

- 新的 Pass 从该范围的起始位置重新开始，并明确记录本轮目的。
- 一个 Pass 可以跨越多个会话；会话检查点记录同一 Pass 内的位置变化。
- 后续 Pass 应与先前 Pass 对照，记录观察位置变化后新增、消失或改变的区分与关系。
- Pass 的范围或目的若发生变化，应明确记录，不能只递增编号来掩盖范围改变。

## 本轮入口顺序

1. 阅读本文件，确认 Pass、当前文档位置和调查边界。
2. 阅读 [Pass 1 / Checkpoint 01](checkpoints/pass-1-checkpoint-01.md)。
3. 阅读 `reconstruction/zh/README.md`，确认中文重构记录的权威边界。
4. 阅读 `translation/INVENTORY.md`，核对已完成材料和源文件位置。
5. 阅读 `experience/patterns.md` 与 `experience/kernel-and-resources.md` 的相关记录，再独立检查原始源文件 `experience/display.md`。

## 当前继续约束

- 当前工作仍是结构、操作与语言的初始重构；不在此轮判断技术主张是否正确。
- 区分源文件明确陈述、跨文件关系、观察者推断和未解决问题。
- 不因某种结构有用，就把它提升为源文件已经定义的正式模型。
- 不把对话中出现、但尚未进入记录的推断当作既有证据。
- 对未解决问题应用有界重构充分性：只有在存在新判别证据来源、相关观察位置变化或具体后果检验时，才继续追查。
