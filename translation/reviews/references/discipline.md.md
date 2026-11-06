# Translation Record: `references/discipline.md`

## Source and provenance

| Item | Value |
|---|---|
| Source path | `references/discipline.md` |
| Source ref | `main` |
| Source blob SHA at translation | `24b3eaa83f4ddfac70e08f0de5fbec6efce42313` |
| English artifact | [`translation/en/references/discipline.md`](../en/references/discipline.md) |
| Translation branch | `translation/english-reconstruction` |
| Translation status | `translated` |

The source document was fetched from `main` before translation. The English artifact is a functional translation, not a revision of the source discipline.

## Translation intent

Preserve the source's operational force and distinctions:

- mandatory records versus recommended document formats;
- project status versus feature-level detail;
- temporary implementation compromises versus unfinished features;
- records that own conclusions versus records that link to them;
- private evidence versus material suitable for public repositories;
- user authorisation boundaries;
- primary-agent responsibility versus bounded sub-agent tasks;
- estimated uncertainty and real-device test capacity.

The translation retains the source's section order, tables, examples, directory layout, technical tokens, and requirements. It does not add a new record type, validation gate, approval mechanism, or workflow.

## Terminology and translation decisions

| Source expression | English rendering | Reason / boundary |
|---|---|---|
| 工作纪律 | Working Discipline | Describes the required conduct and record-keeping discipline, not a policy hierarchy. |
| 状态表 | Status table | Kept distinct from the work log and detailed feature documents. |
| 工作日志 | Work log | The source explicitly says to record changes in conclusions, not a chronological activity diary. |
| 放弃方案记录 | Abandoned-approach record | Preserves the distinction between an abandoned approach and a failed experiment. |
| 技术债表 | Technical-debt table | One shared table; not a general list of unfinished features. |
| 还技术债 | Pay down technical debt | Preserves the source's metaphor of returning to temporary compromises and resolving them. |
| 真机 | Real device | Kept distinct from offline tests and emulator-based evidence. |
| 最早出错的层 | Earliest failing layer | Preserves the instruction to locate the first layer at which the failure occurs, rather than patching downstream symptoms. |
| 一次只改一个变量 | Change only one variable at a time | Retains the experiment-control requirement. |
| 子 agent | Sub-agent | Retains the explicit delegation relationship to the primary agent. |

### Status vocabulary

The source defines six Chinese labels:

| Chinese source label | English label |
|---|---|
| 待调研 | Research pending |
| 有源码线索 | Source-code lead available |
| 离线通过 | Passed offline |
| 真机启动 | Boots on a real device |
| 真机通过 | Passed on a real device |
| 真机失败 | Failed on a real device |

The translated status table uses the English labels as its operational vocabulary. Their distinctions are preserved: a source-code lead is not a successful test; booting on a real device is not the same as passing there; offline success is not real-device success.

These six labels belong to the discipline document's project/status-table instruction. The separately defined evidence classifications in `references/verification.md` must not be assumed to be the same vocabulary or mapped onto these labels without source evidence.

## Structural observations

1. **The document specifies record responsibilities, not rigid templates.** It requires certain information to be preserved while leaving exact document formats to the project.
2. **The records have distinct ownership roles.** In particular, the status table carries current state and the unique next step; the work log records changes in conclusions; detailed feature documents carry investigation and evidence; the technical-debt table owns temporary compromises.
3. **The one-owner rule is explicit.** A conclusion belongs in one place, with links from other documents. Failures and rollbacks are appended rather than rewriting history.
4. **The workspace separates private working material from public deliverables.** The private workspace contains records and internal tools; the kernel and integration repositories remain independent repositories with different responsibilities.
5. **Privacy constraints are stronger than ordinary repository hygiene.** Device identifiers, calibration data, backups, passwords, and logs containing them are restricted to the Git-ignored `private/` directory; passwords are passed through environment variables.
6. **User authorisation is a boundary on consequential repository actions.** Pushing, repository creation, release, visibility changes, and history rewriting each require asking the user.
7. **The estimation model is evidence-capacity based.** It estimates uncertain subsystem layers and real-device test rounds, rather than person-days or code volume.
8. **Sub-agent delegation is constrained and accountable.** The primary agent retains planning, acceptance, communication, and verification of critical evidence; sub-agents receive bounded tasks with explicit permitted files, verification, stop conditions, and record updates.

## Ambiguities and limits preserved

- The source says the exact document format is left to the project. No canonical schemas or mandatory filenames are invented beyond the names and locations explicitly given.
- The source requires rejected hypotheses to retain their original wording and have the scope of rejection appended. The translation preserves this instruction without defining a format for doing so.
- The source gives examples of temporary workarounds but does not define a closed list. The translation retains them as examples.
- The source says the private workspace contains two independent public repositories, while the tree depicts a private local workspace containing them. The translation preserves this architecture rather than interpreting it as a particular Git submodule or worktree arrangement.
- The source's instruction that changing attribution arrangements after publication requires rewriting history is preserved as written; no Git procedure or exception is added.
- The workload ranges (2–5 test rounds per uncertain layer; 10–30 minutes per round) are source estimates, not independently validated empirical guarantees.
- The source does not specify a formal audit cadence for technical debt, a formal definition of "same failure", or a standard sub-agent task template. None has been added.
- The source's instruction to ask the user before consequential repository actions is translated as an authorisation requirement; it does not imply that any such action is authorised in this translation project.

## Review observations

- The source's distinction between an unfinished feature and a temporary compromise is central and has been retained.
- The status vocabulary is presented in English in the translated artifact. The source labels and their correspondence are recorded here for provenance.
- The workspace tree, privacy boundary, repository separation, attribution requirements, estimation method, and sub-agent controls have been retained.
- No technical procedures were independently validated. In particular, the engineering estimates and recommendations remain translated source claims, not findings from this reconstruction.

## Current assessment

**Status: translated.**

- Structural fidelity: first-pass review complete.
- Operational requirements: translated.
- Technical identifiers and paths: retained.
- Source ambiguities: preserved; no unsupported workflow structures added.
- Semantic review: outstanding.
- Technical validation: not performed.
- Whole-skill validation: outstanding.

This record describes the translation and our reading of the source. It is not an authoritative statement of the original authors' intent beyond what the source text establishes.


## Pass 3 — technical and semantic review

Source/translation pair rechecked against `references/discipline.md` at blob `24b3eaa83f4ddfac70e08f0de5fbec6efce42313`.

### Review results

- All eight required-record categories are represented, with the source's record responsibilities and the distinction between the status table, work log, feature documents, technical-debt table, abandoned-approach record, experiment records and external documentation preserved.
- The six status labels retain their distinct meanings. In particular, a source-code lead, an offline pass, a real-device boot and a real-device pass are not collapsed.
- The distinction between unfinished features and temporary compromises registered as technical debt is preserved.
- Record timing, single-owner/linking rule, append-only treatment of failures/rollbacks, evidence provenance and automated-check recommendation are retained.
- The workspace layout, independent repository roles, private-data boundary, attribution rules and user authorisation requirements are retained.
- The workload-estimation method preserves its layer-based uncertainty model, 2–5 real-device rounds per uncertain layer and 10–30 minute estimate per round.
- Sub-agent scope, verification, stopping conditions, device restrictions and primary-agent accountability are retained.
- No unsupported template, status transition, approval mechanism or technical-debt closure condition was added.

### Corrections

No source-driven semantic correction was required. The existing translation was retained unchanged.

### Boundaries

This review establishes source-based semantic correspondence only. It does not validate the empirical workload estimates, device-safety practices, repository recommendations or other technical claims against external evidence. The separate evidence classifications in `references/verification.md` remain to be reviewed independently; the six status terms here are not mapped onto them.

- Source-based semantic review: completed for this document.
- Technical correctness validation: not performed.
- Whole-skill validation: outstanding.
