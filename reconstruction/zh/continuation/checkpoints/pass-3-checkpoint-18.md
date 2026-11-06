# Pass 3 — Checkpoint 18: `experience/power-and-time.md` 语义审查

## 位置

- Pass：**Pass 3 — 操作性英文翻译的技术与语义审查**
- 文档：`experience/power-and-time.md`
- 源分支：`main`
- 源 blob SHA：`d74ba3b4bc320f2036f185e7614d8cdc1a703c6b`
- 英文译文：`translation/en/experience/power-and-time.md`
- 英文译文 blob SHA：`48c1c865ff07ee79e484af2c37f4ffcc8d57a7bc`
- 译文审查记录：`translation/reviews/experience/power-and-time.md.md`
- 译文审查记录 blob SHA：`43c5209fc80ef19b1c611bc51fea77f2e2d20435`
- 前一文档：`experience/wireless.md`
- 前一检查点：[Checkpoint 17 — experience/wireless.md](pass-3-checkpoint-17.md)
- 对应执行记录：[Task 002 — Pass 3 review of experience/power-and-time.md](../../../../translation/tasks/completed/002-pass3-experience-power-and-time.md)
- 下一文档：`experience/sensors-camera-media.md`
- 外部技术正确性验证：未进行。

## 审查结果

Task 002 的 worker handoff 已由协调位置独立重构并评估。完整源文/译文对照覆盖 Pass 3 的七项审查维度；发现一处源文支持的数值表达差异并已修正：源文 `每秒十几次` 表示每秒十余次，原译文 “dozens per second” 夸大了频率，现改为 “more than ten times per second”。

译文保留了电池测量与充电状态、只读 fuel-gauge 访问与控制写入、私有快充认证与普通充电器识别、PMIC RTC 与 SDAM 偏移、deep suspend 与 s2idle、时钟测量与实际休眠时间、充电控制交接与普通休眠入口之间的区别。标题中的 watchdog 与正文中有限的 thermal management 覆盖不对称，仍作为源文覆盖边界保留，没有补写推断出的程序或验收标准。

源文明确指向 `display.md` 的休眠唤醒黑屏问题，英文目标保留该引用并指向预期英文对应文档。三个源文未解决问题及额外的覆盖不足均继续开放。

## Pass 3 处置

- 源文对照技术与语义翻译审查：**完成**。
- 译文修改：**一处源文支持的数值表达修正**。
- 审查记录：已追加 Pass 3 结果，保留原 Pass 1 内容。
- 源文与 display 依赖：未修改，任务指定修订保持不变。
- 外部技术正确性、当前上游行为和真机行为：**未验证**。
- 整体技能验证：仍未完成。
- 下一文档：`experience/sensors-camera-media.md`。

本检查点记录 Task 002 的 worker 执行结果及协调位置的独立评估，不证明电源、充电、PMIC、RTC、休眠、时钟、watchdog 或 thermal 技术主张正确。
