# Reconstruction and translation review: `experience/patterns.md`

## Source

- Repository: `incus-skills/android-mainline-port`
- Source branch: `main`
- Source path: `experience/patterns.md`
- Source blob SHA: `c485e34f6fb06808d7c2678b5a2280494e5f71c0`
- English artifact: [`translation/en/experience/patterns.md`](../../en/experience/patterns.md)
- Review status: `translated`; semantic and technical review remain outstanding.

## Translation intent and boundary

This is a functional English translation of the nine cross-domain problem patterns. It preserves the category sequence, recognition symptoms, general handling steps, concrete manifestations and domain-file references.

The translation does not validate the technical recommendations, assert that the categories are exhaustive, or convert the categories into a formal diagnostic decision tree or mutually exclusive classification system.

## Reconstructed document role

`patterns.md` is a cross-domain diagnostic routing layer within the experience library.

Its stated use is:

1. Encounter a new fault.
2. Compare its symptoms with the recognition criteria for the nine categories.
3. Use the corresponding general handling steps to guide initial investigation.
4. Consult the referenced domain files for concrete, device- and subsystem-specific entries.

The README directs users to this file before searching the domain-specific documents. The patterns therefore mediate between an observed symptom and the more detailed experience records. They do not replace those records.

## Reconstructed structure

Each category follows a repeated three-part structure:

- **How to recognise it** — a cluster of symptoms or circumstances that suggest the category.
- **General handling** — a reusable investigation or response route.
- **Common manifestations** — concrete examples and pointers to domain files.

The nine categories are:

| Category | Reconstructed concern |
|---|---|
| A | Bootloader transformations invalidate assumptions about the values or identity presented to the kernel and userspace. |
| B | Resource ownership and lifecycle are incomplete, so reclamation or provider/consumer behaviour disrupts dependent subsystems. |
| C | Ordering, timing or lifecycle assumptions fail across callbacks, services, devices and power transitions. |
| D | Residual state contaminates a test or makes a result non-reproducible. |
| E | The measurement, test criterion or environment is insufficient or misleading. |
| F | A lower-level success signal is mistaken for end-to-end user-visible functionality. |
| G | A local workaround duplicates a fix already available upstream or in a newer distribution component. |
| H | An implementation from another device or project is transferred without establishing which parts are device-specific. |
| I | The rollback/recovery mechanism has its own failure modes and is not independently reliable. |

## Relationships to other documents

- `experience/README.md` explicitly assigns this file the role of abstracting pitfalls from the domain files into problem categories and giving general recognition/handling guidance.
- The domain files remain the source of specific entries. The examples in each category point to those files.
- `references/verification.md` is relevant to Category E and to the distinction between lower-layer signals and real-device/user-visible evidence in Category F. This is a relationship of concern; the patterns file does not restate the verification framework.
- `references/discipline.md` is relevant to recording unresolved work and technical debt, particularly where a workaround or an unconfirmed root cause remains.
- `references/decisions.md` provides a broader conditional-decision catalogue. The problem patterns are diagnostic entry routes, not substitutes for those decision rules.

The source does not define a strict one-to-one mapping between categories and domain files. A single manifestation may be relevant to multiple categories, and the introduction explicitly says that one symptom may belong to more than one category.

## Terminology and translation decisions

| Source expression | English rendering | Reason |
|---|---|---|
| 问题模式 | Problem patterns | Preserves the cross-domain abstraction role without implying a formal classifier. |
| 怎么认出来 | How to recognise it | Keeps this as symptom-based recognition, not a definitive diagnosis. |
| 通用处理 | General handling | Describes a reusable investigative route rather than a guaranteed remedy. |
| 常见表现 | Common manifestations | Distinguishes examples from the category's recognition criteria and general handling. |
| 资源没有真正的拥有者 | A resource has no actual owner | Preserves the resource-ownership diagnosis without narrowing it to a particular kernel subsystem. |
| 残留状态 | Residual state | Retains the broad notion of stale state across system, build and device contexts. |
| 底层通过不等于用户能用 | Passing at a lower layer does not mean the user can use it | Preserves the distinction between component-level success and end-to-end usability. |
| 恢复路径 | Recovery path | Covers rollback and recovery actions, not only a single reboot operation. |
| 正对照 | Positive control | Uses the experimental-control term for comparison against known-good stock behaviour. |
| 常开电源 / 时钟 / 带宽投票 | always-on power / clock / bandwidth votes | Retains the source's resource-management terminology. |

Technical identifiers, commands, paths, kernel interfaces and file names are retained as written in the source.

## First-pass observations

1. **The categories are heuristic, not exclusive.** The source explicitly allows one symptom to belong to multiple categories. The categories are a way to orient investigation, not a partition of all possible faults.
2. **Recognition is not proof of root cause.** The "How to recognise it" sections describe symptom clusters. The "General handling" sections then prescribe evidence-gathering or discriminating actions. The translation preserves that sequence.
3. **The patterns often challenge an initially plausible signal.** Examples include bootloader-returned status, a loaded driver, a present device node, a passing offline test, a measurement from one data point, or a working development environment. These signals may be real but insufficient for the conclusion being drawn.
4. **Several categories concern boundaries between layers or contexts.** Categories A, B, C and F expose transformations or dependencies across bootloader/kernel/userspace, provider/consumer, event/lifecycle, and hardware-to-user paths. Categories D and E expose contamination or measurement-context problems. Categories G and H concern the provenance and transferability of implementations. Category I examines the reliability of the recovery mechanism itself.
5. **The general handling sections are not uniform algorithms.** They contain different forms of action: capture state, compare against stock, establish timing, isolate residual state, challenge a hypothesis, trace an end-to-end path, check upstream, distinguish transferable components, or test recovery under controlled failure. No artificial common template has been imposed beyond the source's own three-part structure.
6. **The domain references are navigational evidence.** They show where the source expects detailed examples to be found, but do not establish that every example in those files has been independently verified during this translation pass.

## Ambiguities and limits preserved

- The source does not specify how to choose a primary category when symptoms overlap. No priority or tie-breaking rule has been added.
- It does not define a threshold for concluding that a category's suspected cause is confirmed. The README's separate confirmed/unresolved admission rule remains relevant.
- Some category descriptions combine several related mechanisms. The translation retains their breadth rather than splitting them into new categories.
- "Positive control" in Category A is translated as a comparison using the stock system; the source does not specify a complete test protocol.
- Category I's instruction to use SysRq when D-state tasks are present is preserved as a source recommendation, not technically endorsed here.
- The examples and upstream status statements may be version-sensitive. They are translated as source claims and have not been checked against current upstream repositories.

## Review boundary

This record reconstructs the file's diagnostic role, nine-category structure and relationships to the experience library and framework references. It does not establish that:
- the categories are complete, optimal or technically correct;
- every recognition symptom uniquely identifies its category;
- the general handling steps are safe or sufficient for every device;
- the examples remain current;
- every linked domain entry supports the pattern in the same way.

Those questions require a separate semantic/technical review, including whole-library and, where appropriate, real-device evidence. The Chinese source remains authoritative for this translation pass.
