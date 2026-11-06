# 当前观察位置与会话入口

## 当前入口

- 当前遍历：**Pass 3 — 操作性英文翻译的技术与语义审查**
- 当前会话检查点：**Pass 3 / Checkpoint 19**
- 最近完成文档：`experience/sensors-camera-media.md`
- 下一文档：`experience/usb-and-storage.md`
- 已完成审查：`SKILL.md`、`references/discipline.md`、`references/process.md`、`references/decisions.md`、`references/build-environment.md`、`references/verification.md`、`references/ubuntu.md`、`experience/README.md`、`experience/patterns.md`、`experience/bootloader-and-boot.md`、`experience/kernel-and-resources.md`、`experience/display.md`、`experience/gpu.md`、`experience/touch-and-input.md`、`experience/audio.md`、`experience/wireless.md`、`experience/power-and-time.md`、`experience/sensors-camera-media.md`。
- Pass 3 范围：`SKILL.md`、六份 `references/` 文档和十五份 `experience/` 文档。
- 当前观察位置：检查英文功能表达是否保留源文命令力度、条件范围、决策逻辑、技术 token、结构和跨文件引用。
- 明确排除：不以此轮证明技术主张在当前上游或真实设备上正确；不进行真机验证。
- 最近检查点：[Pass 3 / Checkpoint 19 — experience/sensors-camera-media.md](checkpoints/pass-3-checkpoint-19.md)。
- 前一检查点：[Pass 3 / Checkpoint 18 — experience/power-and-time.md](checkpoints/pass-3-checkpoint-18.md)。
- Pass 3 范围声明：[Pass 3 / Checkpoint 01](checkpoints/pass-3-checkpoint-01.md)。
- 执行上下文分段：[Context-segmented execution](../../../translation/EXECUTION-SEGMENTATION.md)。该安排是执行层面的候选方案，不改变 Pass 3 的范围、标准或观察位置。

## 遍历编号约定

**Pass（遍历轮次）**表示对已声明范围从起点到终点的一次完整遍历。它不是会话编号，也不是单个文档的审查次数。

- 新的 Pass 从该范围的起始位置重新开始，并明确记录本轮目的。
- 一个 Pass 可以跨越多个会话；会话检查点记录同一 Pass 内的位置变化。
- 后续 Pass 应与先前 Pass 对照，记录观察位置变化后新增、消失或改变的区分与关系。
- Pass 的范围或目的若发生变化，应明确记录，不能只递增编号来掩盖范围改变。

## 本轮入口顺序

1. 阅读本文件，确认 Pass 3 的目的、范围和当前文档位置。
2. 阅读 [Pass 3 / Checkpoint 19](checkpoints/pass-3-checkpoint-19.md)，必要时回看 [Checkpoint 17](checkpoints/pass-3-checkpoint-17.md) 及更早检查点。
3. 阅读 `translation/README.md` Phase 4 和 `translation/GLOSSARY.md`。
4. 阅读 [Context-segmented execution](../../../translation/EXECUTION-SEGMENTATION.md)，仅用于建立任务边界和上下文加载策略；不得将其误作新的翻译审查标准。
5. 对照 `experience/usb-and-storage.md` 的中文源文、英文译文和既有审查记录。
6. 区分翻译语义审查与外部技术正确性验证；不要把后者默认为本轮任务。

## 后续工作约束

- Pass 1 的结构重构和 Pass 2 的 README 对照审查作为前序基线。
- 不修改原始中文源文件。
- 不因文风偏好改写已忠实表达的内容。
- 不把源文未建立的关系补入译文。
- 外部正确性、时效性和真机行为问题单独记录，不在此轮臆断。
- 对未解决问题应用有界重构充分性：只有在存在新判别证据来源、相关观察位置变化或具体后果检验时，才继续追查。
- 文件级 worker 若发现缺失依赖或无法支持下一跳的证据，应报告并停止，不得自行扩大范围；由协调位置决定是否加载额外材料或改变任务边界。
