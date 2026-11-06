# 当前观察位置与会话入口

## 当前入口

- 当前遍历：**Pass 3 — 操作性英文翻译的技术与语义审查**
- 当前会话检查点：**Pass 3 / Checkpoint 23 — 文件级审查范围闭合**
- 最近完成文档：`experience/testing-and-process.md`
- 下一文档：无；Checkpoint 01 声明的十五份 experience 文档已全部完成 Pass 3 审查。
- 已完成审查：`SKILL.md`、`references/discipline.md`、`references/process.md`、`references/decisions.md`、`references/build-environment.md`、`references/verification.md`、`references/ubuntu.md`、`experience/README.md`、`experience/patterns.md`、`experience/bootloader-and-boot.md`、`experience/kernel-and-resources.md`、`experience/display.md`、`experience/gpu.md`、`experience/touch-and-input.md`、`experience/audio.md`、`experience/wireless.md`、`experience/power-and-time.md`、`experience/sensors-camera-media.md`、`experience/usb-and-storage.md`、`experience/ubuntu-userspace.md`、`experience/build-and-release.md`、`experience/testing-and-process.md`。
- Pass 3 范围：`SKILL.md`、六份 `references/` 文档和十五份 `experience/` 文档；上述文件级 source-based semantic review 均已完成并记录。
- 当前观察位置：Pass 3 的文件级审查 exit condition 已达到；保留 source-based translation review 与外部技术验证之间的边界。
- 明确排除：本轮不证明技术主张在当前上游或真实设备上正确；未进行真机验证、skill activation 或代表性工作流执行。
- 最近检查点：[Pass 3 / Checkpoint 23 — experience/testing-and-process.md](checkpoints/pass-3-checkpoint-23.md)。
- 前一检查点：[Pass 3 / Checkpoint 22 — experience/build-and-release.md](checkpoints/pass-3-checkpoint-22.md)。
- Pass 3 范围声明：[Pass 3 / Checkpoint 01](checkpoints/pass-3-checkpoint-01.md)。
- 执行上下文分段：[Context-segmented execution](../../../translation/EXECUTION-SEGMENTATION.md)。该安排是执行层面的候选方案，不改变 Pass 3 的范围、标准或观察位置。

## 遍历编号约定

**Pass（遍历轮次）**表示对已声明范围从起点到终点的一次完整遍历。它不是会话编号，也不是单个文档的审查次数。

- 新的 Pass 从该范围的起始位置重新开始，并明确记录本轮目的。
- 一个 Pass 可以跨越多个会话；会话检查点记录同一 Pass 内的位置变化。
- 后续 Pass 应与先前 Pass 对照，记录观察位置变化后新增、消失或改变的区分与关系。
- Pass 的范围或目的若发生变化，应明确记录，不能只递增编号来掩盖范围改变。

## 本轮入口顺序

1. 阅读本文件，确认 Pass 3 文件级审查范围已闭合。
2. 阅读 [Pass 3 / Checkpoint 23](checkpoints/pass-3-checkpoint-23.md)，必要时回看 [Checkpoint 22](checkpoints/pass-3-checkpoint-22.md) 及更早检查点。
3. 阅读 [Pass 3 范围声明 / Checkpoint 01](checkpoints/pass-3-checkpoint-01.md)，确认本轮 exit condition 与排除项。
4. 阅读 `translation/README.md` Phase 4 和 `translation/INVENTORY.md`，区分已完成的 source-based review 与尚未执行的外部/whole-skill validation。
5. 若启动后续工作，先建立新的观察位置、范围与证据边界；不要将后续验证隐含为 Pass 3 的延伸。

## 后续工作约束

- Pass 1 的结构重构和 Pass 2 的 README 对照审查作为前序基线。
- 不修改原始中文源文件。
- 不因文风偏好改写已忠实表达的内容。
- 不把源文未建立的关系补入译文。
- 外部正确性、时效性和真机行为问题单独记录，不在此轮臆断。
- 对未解决问题应用有界重构充分性：只有在存在新判别证据来源、相关观察位置变化或具体后果检验时，才继续追查。
- 文件级 worker 若发现缺失依赖或无法支持下一跳的证据，应报告并停止，不得自行扩大范围；由协调位置决定是否加载额外材料或改变任务边界。
- Pass 3 文件级审查完成不等于 whole-skill validation、skill activation、代表性工作流执行或技术正确性验证完成。
