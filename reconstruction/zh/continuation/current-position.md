# 当前观察位置与会话入口

## 当前入口

- 当前遍历：**Pass 4 — whole-skill English validation**。
- 当前项目阶段：**Phase 5 — Whole-skill validation**，依照 `translation/README.md`。
- 当前会话检查点：**Pass 4 / Checkpoint 01 — 验证范围建立，Task 008 待执行**。
- 前一遍历：Pass 3 — 操作性英文翻译的技术与语义审查。
- 前一遍历完成位置：Pass 3 / Checkpoint 23；`experience/testing-and-process.md` 是最后一份文件级审查文档。
- Pass 3 已声明的文件级 source-based review exit condition 已达到；这不代表 Phase 5 或整个 skill 已验证。
- Pass 4 已完成的 worker 操作：无。
- 下一操作：**Task 008 — Phase 5 entrypoint and navigation integrity**，检查 `translation/en/SKILL.md` 与固定版本的英文文档树。
- 当前观察位置：项目级验证英文 skill 的导航、依赖、工作流可用性、一致性，以及安装和 skill-discovery metadata。此位置不是 Pass 3 的延伸。
- Pass 4 范围与 exit condition：[Pass 4 / Checkpoint 01](checkpoints/pass-4-checkpoint-01.md)。
- 前一遍历闭合记录：[Pass 3 / Checkpoint 23](checkpoints/pass-3-checkpoint-23.md)。
- Pass 3 原始范围声明：[Pass 3 / Checkpoint 01](checkpoints/pass-3-checkpoint-01.md)。
- Phase 5 定义：[translation/README.md](../../../translation/README.md)。
- 执行上下文分段：[Context-segmented execution](../../../translation/EXECUTION-SEGMENTATION.md)。这是仍待观察效果的执行安排，不改变 Phase 5 的验证标准。
- 当前待执行任务：[Task 008](../../../translation/tasks/008-phase5-entrypoint-navigation-integrity.md)。

## 遍历编号约定

**Pass（遍历轮次）**表示对已声明范围从起点到终点的一次完整遍历。它不是会话编号，也不是单个文档的审查次数。

- 新的 Pass 从该范围的起始位置重新开始，并明确记录本轮目的。
- 一个 Pass 可以跨越多个会话；会话检查点记录同一 Pass 内的位置变化。
- 后续 Pass 应与先前 Pass 对照，记录观察位置变化后新增、消失或改变的区分与关系。
- Pass 的范围或目的若发生变化，应明确记录，不能只递增编号来掩盖范围改变。
- Pass 4 对应 Phase 5 的 whole-skill validation；Phase 与 Pass 是不同层级，不能混为一谈。

## 本轮入口顺序

1. 阅读本文件，确认 Pass 4 / Checkpoint 01 和当前待执行任务。
2. 阅读 [Pass 4 / Checkpoint 01](checkpoints/pass-4-checkpoint-01.md)，确认新遍历的目的、范围、证据边界与 exit condition。
3. 回看 [Pass 3 / Checkpoint 23](checkpoints/pass-3-checkpoint-23.md)，确认前一遍历的接受范围及明确排除项；不要重开已闭合的文件级翻译审查。
4. 阅读 `translation/README.md` Phase 5 和 `translation/INVENTORY.md`，区分已完成的 source-based review 与 whole-skill validation。
5. 阅读 `translation/EXECUTION-SEGMENTATION.md`，按其 evidence-boundary、worker/orchestrator 分工和八字段 handoff 工作；不要把该方案视为已验证。
6. 阅读 [Task 008](../../../translation/tasks/008-phase5-entrypoint-navigation-integrity.md)，并从其固定 blob/tree revisions 建立 worker 证据边界。

## Pass 4 约束

- Pass 3 的文件级翻译审查已经闭合。Phase 5 中发现的可用性或技术问题，不自动成为 Pass 3 翻译缺陷。
- Phase 5 包括入口与导航、代表性工作流、跨文档一致性/依赖、源语言泄漏，以及安装和 skill-discovery metadata 兼容性。各项应按证据和依赖分成有界操作。
- 单个 worker task 的完成不等于 orchestrator acceptance；单个 task 被接受也不等于 Pass 4 完成。
- 只有独立重构 handoff 并作出 **ACCEPTED** 裁定后，才能把该操作纳入已接受的项目位置并推进相应 continuity records。**REJECTED** 和 **UNRESOLVED** 不得作为已接受进展。
- Worker 不得假定省略的上下文不存在。缺少依赖或证据不足时，应指出具体缺口并停止，不得自行扩大范围。
- 不修改原始中文源文件或英文翻译目标文件，除非另行建立明确且有界的修正任务。
- 不把 source-faithful translation 当作当前上游正确性、真机行为或操作安全的证明；这些问题必须单独建立范围与证据边界。
- 不因 ingress、任务记录或 handoff 的存在而宣称 context segmentation 已有效；其效果仍是观察对象。
- 不从当前协调位置裁定整体项目完整性；保留独立评估所需的证据和未决事项。

## Pass 3 历史闭合

Pass 3 的范围为 `SKILL.md`、六份 `references/` 文档及十五份 `experience/` 文档。最后一份 `experience/testing-and-process.md` 于 Checkpoint 23 完成独立接受。Pass 3 不包括 whole-skill validation、skill activation、代表性工作流执行、当前技术正确性或真机验证。具体文件、blob revisions 和接受依据以 Checkpoint 23 为准。
