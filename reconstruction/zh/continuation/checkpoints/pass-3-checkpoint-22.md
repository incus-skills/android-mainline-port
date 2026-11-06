# Pass 3 — Checkpoint 22: `experience/build-and-release.md` 语义审查

## 位置

- Pass：**Pass 3 — 操作性英文翻译的技术与语义审查**
- 文档：`experience/build-and-release.md`
- 中文源文：`experience/build-and-release.md`，`main`，blob SHA `05c40c2b974cd14f229db814bc1ef63b1e657c65`
- 英文译文：`translation/en/experience/build-and-release.md`，`translation/english-reconstruction`，blob SHA `411a9d962428bff35f6a8c66116ce789290028e2`（未修改）
- 审查记录：`translation/reviews/experience/build-and-release.md.md`，最终 blob SHA `b1d0c71907a8c6a6ddfbd586c4118f6229b71855`
- Worker 任务：[Task 006 — completed](https://github.com/incus-skills/android-mainline-port/blob/translation/english-reconstruction/translation/tasks/completed/006-pass3-experience-build-and-release.md)，最终 blob SHA `8bc62329aab109f927accd1e9aac6b2307176403`
- 前一检查点：[Checkpoint 21 — experience/ubuntu-userspace.md](pass-3-checkpoint-21.md)
- 下一文档：`experience/testing-and-process.md`
- 外部技术正确性验证、当前上游行为、构建基础设施、基准测试、发布操作和真机验证：未进行，且不属于本次任务范围。

## 独立重构与证据核对

协调位置按 Orchestrator ingress 重新获取并检查 Task 006、原始八字段 handoff、追加的 corrective handoff、完整中文源文、完整英文译文、完整审查记录、当前 Pass 3 治理材料及任务列明的依赖文件。

原任务列出的十四项证据基线在首次独立审查时均与记录的 blob SHA 匹配。纠正后再次获取的四项最终文件修订与 corrective handoff 一致：

- 中文源文：`05c40c2b974cd14f229db814bc1ef63b1e657c65`
- 英文译文：`411a9d962428bff35f6a8c66116ce789290028e2`
- 审查记录：`b1d0c71907a8c6a6ddfbd586c4118f6229b71855`
- Completed Task 006：`8bc62329aab109f927accd1e9aac6b2307176403`

独立核对确认：

- `LOCALVERSION_AUTO` 不再出现在 Pass 3 technical-token inventory 中；其在纠正说明中只作为被移除的错误项目出现。
- Pass 3 的执行模式比较使用 `full-system QEMU emulation`，未增加源文和译文均未指定的 `TCG`。
- 与 `qemu-user` 的区别仍保留。
- 原 Pass 1 审查记录保持原样；其中历史性的 `full-system QEMU/TCG` 表述被 corrective record 明确限定为 Pass 1 历史记录，不作为 Pass 3 比较依据。
- 源文和英文目标均未改变；原 worker handoff 与 corrective handoff 的八字段均可重构。
- 修正仅触及 Pass 3 审查记录中的两项 unsupported specificity 和相应 provenance clarification；未发现超出边界的修改。

## 审查结果

完整源文与完整译文的比较支持原 worker 的主要结论：没有发现需要修改英文目标的 material, source-justified discrepancy。文件的具体 pitfall 集合边界、build-environment 引用、测量与假设、执行环境、缓存和 artifact identity、reproducibility inputs、release integrity、版本节奏及 public/internal documentation 区分均得到保留。

本次拒绝接受的原因是审查记录本身出现了两个不受源文支持的技术细节，而不是英文译文存在缺陷。纠正后，Pass 3 比较与源文/译文证据重新一致。

## 接受裁定

- 原 worker 执行状态：**COMPLETED**。
- corrective worker 执行状态：**COMPLETED**。
- Orchestrator 独立裁定：**ACCEPTED**。
- 接受依据：原任务十四项基线已核对；纠正后的源文、译文、审查记录和任务修订匹配；两个 unsupported specificity 已移除；Pass 1 历史记录完整保留并明确区分；handoff 可重构；改动符合任务边界。
- 接受范围：仅接受 `experience/build-and-release.md` 的有界 Pass 3 翻译审查及其 corrected review record。
- 不代表：构建/发布技术主张已验证、当前上游行为已确认、真机测试或构建基础设施验证已完成，或 Pass 3/整个技能已完成。

Worker 完成、corrective worker 完成与 orchestrator 接受是三个独立事件。本检查点记录最后一项，不改写前两项的历史状态。

## 下一位置

Pass 3 的下一份 experience 文档为 `experience/testing-and-process.md`。源经验库和英文经验库索引均将其列在 build-and-release 之后。既有 Pass 1 审查记录指出 Pattern E/F/H/I 的显式索引关系；下一任务应检查这些关系是否在当前译文中准确表达，同时区分索引建立的关系与源文件正文中的 inline link。是否需要其他依赖，应由 worker 在任务边界内根据源文和现有审查证据判断。
