# Pass 3 — Checkpoint 21: `experience/ubuntu-userspace.md` 语义审查

## 位置

- Pass：**Pass 3 — 操作性英文翻译的技术与语义审查**
- 文档：`experience/ubuntu-userspace.md`
- 中文源文：`experience/ubuntu-userspace.md`，`main`，blob SHA `091d4ba144a77ad20707259a840d6599b2e501da`
- 英文译文：`translation/en/experience/ubuntu-userspace.md`，修改前 blob SHA `baa0b4ee44c5507e3fc5df2ad527da24e6c06355`，修改后 blob SHA `47324f6d9e29c4ea19dc911cd6f67f144e4e5954`
- 审查记录：`translation/reviews/experience/ubuntu-userspace.md.md`，Pass 1 原始 blob SHA `c9d57da433c3dbfb92321fa1ef92874b1a2d6a35`，追加 Pass 3 后 blob SHA `181d7c2de13af82f24c5697c316a9a713e6f7f6d`
- Worker 任务：[Task 005 — completed](https://github.com/incus-skills/android-mainline-port/blob/translation/english-reconstruction/translation/tasks/completed/005-pass3-experience-ubuntu-userspace.md)
- 前一检查点：[Checkpoint 20 — experience/usb-and-storage.md](pass-3-checkpoint-20.md)
- 下一文档：`experience/build-and-release.md`
- 外部技术正确性验证、当前上游行为和真机验证：未进行，且不属于本次任务范围。

## 独立重构与证据核对

协调位置通过 Orchestrator ingress 重新读取已完成的 Task 005、八字段 handoff、完整中文源文、修改后的英文译文、完整审查记录及任务指定的治理和依赖材料。

任务指定的十四项证据基线均与执行前记录的 blob SHA 匹配。执行后重新获取的目标和审查记录 SHA 与 handoff 所述一致。中文源文仍为原始 blob SHA。Task 005 已从 pending 路径移至 `translation/tasks/completed/`；该移动只证明 worker 执行和 handoff 已完成，不作为接受依据。

协调位置直接从 Git blob 读取目标修改前后的内容，验证修改后目标与修改前目标相比恰好只有两处指定替换：
- SquashFS/snap-confine 条目的适用范围由 `Ubuntu` 恢复为 `Ubuntu; 2026`。
- zram-generator 条目的适用范围由 `Ubuntu` 恢复为 `Ubuntu; 2026`。

审查记录修改后的完整内容以原 Pass 1 审查记录为精确前缀，Pass 1 内容未被改写或删除。worker handoff 的八个字段均存在。源文、架构参考、索引、glossary、inventory 和 continuation records 均未被 worker 修改。

## 审查结果

协调位置核对了完整源文、完整目标和 Pass 3 审查记录。worker 对七项维度的报告与所见材料一致：

- 命令力度、条件范围、决策分支、技术 token 和条目结构得到保留；未发现超出两处年份限定之外、可由源文支持的目标修正。
- 源文明确将整体架构和包拆分指向 `../references/ubuntu.md`，并把本文件限定为具体问题记录；该边界及英文对应引用保留。架构文档只核对了引用对应关系，没有进行技术审查。
- 源经验索引和英文索引支持文档顺序；这不构成 USB/storage 与 Ubuntu userspace 之间的技术依赖。
- Pass 1 审查中的 Pattern C/D/G 对应关系和观察者重构关系仍作为审查记录中的观察，不提升为源文明确依赖。
- gnome-shell 持有但未释放的 sleep-delay inhibitor lock 仍未识别；目标保留源文的不确定性。
- 当前上游状态、真实设备行为和记录中技术解决方案的正确性均未验证，符合任务排除边界。

## 接受裁定

- Worker 执行状态：**COMPLETED**。
- Orchestrator 独立裁定：**ACCEPTED**。
- 接受依据：十四项任务证据基线匹配；源文未变；目标仅包含两处源文支持的年份恢复；Pass 1 审查完整保留；handoff 可重构；改动符合许可边界；未发现阻止接受的证据缺口。
- 接受范围：仅接受 `experience/ubuntu-userspace.md` 的有界 Pass 3 翻译审查及其两处修正。
- 不代表：Ubuntu 技术主张已验证、当前上游状态已确认、真机验收已完成，或 Pass 3/整个技能已完成。

Task 005 的 worker 完成与 orchestrator 接受是两个独立事件。本检查点记录后者，不改变前者的原始记录。

## 下一位置

Pass 3 的下一份 experience 文档为 `experience/build-and-release.md`。其选择依据是源经验库和英文经验库索引中的文档顺序；是否存在额外依赖，应在下一任务的证据重构中判断。下一 worker 操作应在本检查点、current-position 和 inventory 完成协调后再建立。
