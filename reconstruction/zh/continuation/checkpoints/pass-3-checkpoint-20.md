# Pass 3 — Checkpoint 20: `experience/usb-and-storage.md` 语义审查

## 位置

- Pass：**Pass 3 — 操作性英文翻译的技术与语义审查**
- 文档：`experience/usb-and-storage.md`
- 中文源文：`experience/usb-and-storage.md`，`main`，blob SHA `1737f41450dcbe6ebd6880790a4b4e8c072bc9c7`
- 英文译文：`translation/en/experience/usb-and-storage.md`，blob SHA `7c3225d7a7f8650ce27a345922937cdadcf0cc59`
- 审查记录：`translation/reviews/experience/usb-and-storage.md.md`，Pass 3 追加后 blob SHA `12e20c452b0f2a00b554adc8fbae0d4112818b77`
- Worker 任务：[Task 004 — completed](https://github.com/incus-skills/android-mainline-port/blob/translation/english-reconstruction/translation/tasks/completed/004-pass3-experience-usb-and-storage.md)
- Worker completion commit：[42d07313f1cd21239711eb814918635ddecdb76b](https://github.com/incus-skills/android-mainline-port/commit/42d07313f1cd21239711eb814918635ddecdb76b)
- Worker task move commit：[08bba9dbbedffc5b46c7a47c7b11aad850fbdfe1](https://github.com/incus-skills/android-mainline-port/commit/08bba9dbbedffc5b46c7a47c7b11aad850fbdfe1)
- 前一检查点：[Checkpoint 19 — experience/sensors-camera-media.md](pass-3-checkpoint-19.md)
- 下一文档：`experience/ubuntu-userspace.md`
- 外部技术正确性验证、当前上游行为和真机验证：未进行。

## 独立重构与评估

协调位置重新读取了已完成的 Task 004、八字段 worker handoff、完整中文源文、完整英文译文、Pass 1/Pass 3 审查记录和任务指定的治理材料。

任务记录中的十二项证据基线均与执行前指定的 blob SHA 匹配。中文源文仍为原始 blob SHA。英文目标由 `e756e98882ece48f879aad7434098d0a7706a07d` 更新为 `7c3225d7a7f8650ce27a345922937cdadcf0cc59`。审查记录由原 Pass 1 blob `9498835f6aaa108dd732121de46bc40ab9f10255` 追加为 `12e20c452b0f2a00b554adc8fbae0d4112818b77`；协调位置将当前审查记录与原始 Pass 1 blob 比较，确认原内容完整保留为前缀。

独立核对了 worker 的提交序列：目标译文提交只修改目标文件；审查提交只修改审查记录；handoff 提交只修改 Task 004；移动提交只改变任务记录路径。改动范围符合任务许可边界。

## 审查结果

Worker 对照了全部七项 Pass 3 维度。协调位置检查了源文、修改后的译文和审查记录，未发现超出任务边界的改动或阻止接受的证据缺口。

- 维护镜像的超时行为被澄清为：超时到期时若没有活动会话，则自动返回 Fastboot；该表述与源文的无会话超时回退要求一致。
- 恢复了四处源文明确给出的 `2026` 适用年份：B 槽/recovery 限制、主机 Fastboot A/B 写槽、双系统安装后的 Android 首启失败，以及 Qualcomm DWC3 + QMP 主机模式不稳定。
- sparse image 的 FILL 写零与 DONT_CARE 跳过写入、bootloader 槽状态与 GPT 属性、逐分区备份与原子快照、设备本地预置与公开镜像内容仍保持区分。
- USB/Type-C、UCSI/VBUS、存储身份、活动槽轮换、安装环境和恢复通道的各自条件与安全边界未被合并为统一模型。
- 源文没有直接 Markdown 链接或命名的领域文档依赖。Pass 1 记录中的 Pattern A/D/I 对应关系仍是审查记录观察，不被提升为源文明确依赖。
- 源文未给出的分区几何、活动槽实现细节、量化验收阈值、UFS 属性细节和维护镜像实现/测试证据继续保持未指定。

## 处置

- Worker 执行：**完成**。
- 协调位置独立评估：**接受**。
- 英文译文修改：**五处源文支持的修正**（一处维护镜像超时澄清，四处恢复年份限定）。
- Pass 3 审查记录：**已追加；Pass 1 内容完整保留**。
- 中文源文、glossary、Pass 3 标准、inventory 和 continuation records：**worker 未修改**。
- 外部技术正确性与真机行为：**未验证**。
- 整体技能验证：**仍未完成**。

Task 004 的完成和接受仅表示此文件的有界 Pass 3 翻译审查已完成，不表示其技术主张已验证，也不表示 Pass 3 整体结束。

## 下一位置

下一文件为 `experience/ubuntu-userspace.md`。源经验库索引与英文索引均将其列在 USB/storage 之后。该文档明确把推荐整体架构和包拆分指向 `references/ubuntu.md`，并将自身范围限定为具体问题记录；这项源文明确的参考关系应随任务保留，但不得因此把架构文档的技术验证纳入本次文件级翻译审查。
