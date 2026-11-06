# 当前观察位置与会话入口

## 当前入口

- 最近完成遍历：**Pass 2 — 仓库面向读者的英文材料（进行中）**
- 当前会话检查点：**Pass 2 / Checkpoint 01**
- 最近完成的源文件：`README.md`，与 `README.en.md` 对照审查
- 当前源文件位置：仓库 README 对照审查已完成；本 Pass 的后续范围尚未扩展。
- 当前观察位置：从操作性技能核心转向仓库面向读者的介绍、安装和使用入口。
- 下一步：先明确是否将 Pass 2 扩展到其他面向用户的材料，或关闭当前 README 配对审查范围并另行声明后续遍历。不要自动把 marketplace 元数据、二维码资产决策、外部安装兼容性和整体技能验证合并进本 Pass。
- 最近检查点：[Pass 2 / Checkpoint 01 — Repository README](checkpoints/pass-2-checkpoint-01.md)。
- 前一遍历基线：[Pass 1 scope reconciliation](checkpoints/pass-1-scope-reconciliation.md)。

## 遍历编号约定

**Pass（遍历轮次）**表示对已声明范围从起点到终点的一次完整遍历。它不是会话编号，也不是单个文档的审查次数。

- 新的 Pass 从该范围的起始位置重新开始，并明确记录本轮目的。
- 一个 Pass 可以跨越多个会话；会话检查点记录同一 Pass 内的位置变化。
- 后续 Pass 应与先前 Pass 对照，记录观察位置变化后新增、消失或改变的区分与关系。
- Pass 的范围或目的若发生变化，应明确记录，不能只递增编号来掩盖范围改变。

## 本轮入口顺序

1. 阅读本文件，确认 Pass 2 的目的、范围和观察位置。
2. 阅读 [Pass 2 / Checkpoint 01](checkpoints/pass-2-checkpoint-01.md)。
3. 阅读 [README 翻译审查](../../../translation/reviews/README.md.md) 和 [中文重构](../README.md) 中对应记录。
4. 阅读 `translation/INVENTORY.md` 与 `translation/README.md`，确认文件映射和后续阶段边界。
5. 在继续前明确是否扩展 Pass 2 的范围；不要默认技术验证、整体验证和元数据本地化属于当前任务。

## 后续工作约束

- Pass 1 仍是已关闭的操作性核心重构基线。
- Pass 2 当前只完成 README/README.en 对照审查，不代表 Pass 2 整体关闭。
- 区分源文件明确陈述、英文对应表达、观察者推断和外部时效问题。
- 不把翻译保真度等同于技术正确性或安装兼容性。
- 对未解决问题应用有界重构充分性：只有在存在新判别证据来源、相关观察位置变化或具体后果检验时，才继续追查。
