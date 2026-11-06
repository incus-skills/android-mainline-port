# Pass 1 — Checkpoint 12: Testing Judgement, Automation, Collaboration and Process

## 1. Position and scope

- Traversal: **Pass 1 — initial structural and linguistic reconstruction**
- Previous checkpoint: [Pass 1 / Checkpoint 11](pass-1-checkpoint-11.md)
- Completed source: `experience/testing-and-process.md`
- Source branch: `main`
- Source blob SHA: `ff1dbea0833dc9e9eb2f668000855f73ec0a9599`
- Collection index: `experience/README.md`
- Pattern index: `experience/patterns.md`
- English translation: `translation/en/experience/testing-and-process.md`
- Translation review: `translation/reviews/experience/testing-and-process.md.md`
- Chinese descriptive reconstruction: `reconstruction/zh/testing-and-process.md`
- Technical validation: not performed

Pass 1 remains the initial structural and linguistic reconstruction. This checkpoint does not validate technical claims, test procedures, kernel parameters, or device-safety recommendations.

## 2. Reconstructed document role

The source is a single sequence of concrete pitfalls under one heading. Although its title names testing judgement, automation, collaboration and process, the body does not divide itself into those four formal categories.

It contains no recommended route, formal acceptance checklist, process sequence or dedicated unresolved section. It spans evidence quality, test-tool validity, host scripts, user participation, device concurrency, diagnostic reasoning, sub-agent task constraints, reuse of comparable/community work, external documentation, recovery safety and contribution review.

The translation preserves this source shape rather than imposing a workflow or taxonomy.

## 3. Main reconstructed distinctions

### Evidence and acceptance

The source distinguishes code or service execution from functional usability. A service being active, a probe succeeding, QEMU passing, Bluetooth being up or a shadow buffer containing an image does not alone establish user-facing function.

It also separates:
- offline evidence from real-device evidence;
- machine/hardware evidence from human confirmation;
- lower-layer tests from the user's actual entry point;
- runtime switching from cold-boot validation of an image containing the change.

### Test validity and measurement

The criteria, orchestrator, wrapper, data, host script and observation method may themselves be faulty. The source calls for negative test inputs, preserving exceptions and evidence, and re-evaluating saved evidence after a tool fix without rerunning the device or relaxing the criteria.

Root-cause claims are challenged with another data point or control group. Exclusion experiments begin from a clean boot. If other conditions are unknown, lack of observed improvement must remain qualified.

### Host/device and user boundaries

Host-side scripts are part of the test system and can waste real-device opportunities through addressing, routing, terminal, line-ending, exit-status, output-capture, USB re-enumeration or Fastboot-handoff problems.

User actions require advance notice and a clear observation target. Long-running recorders may replace short interaction windows. Desktop automation can reduce avoidable user actions, but injected input does not prove physical touch.

Only one operator may act on the device at a time. Evidence is bound to boot ID; host logs without one cannot be attributed.

### Collaboration, continuity and safety

NO-GO records preserve rejected approaches and reverted changes across context compression or handoff. Sub-agent tasks are constrained by one approach, allowed files, one verification command, a pass criterion and a stopping condition. A claim of testing remains unverified unless the method matches actual use.

The candidate-kernel incident demonstrates a distinction between a safety rule existing in the experience library and that rule being delivered in the active task brief. The source requires the relevant constraints to be present in task instructions and calls for stopping after the first hardware failure.

### Reuse and external communication

Comparable projects should be examined before designing a process. Community implementations are compared at the level of device-tree approach, resource management, init and installation before choosing a baseline switch or feature-family import.

External documentation is for first-time users, conclusions have one owning location, and the main agent reviews before release. Community contributions are accepted where possible, subject to project-scope and legal/ethical boundaries, with author/source provenance recorded after merge.

## 4. Explicit cross-document relationships

- `experience/README.md` lists this document's scope as judgement criteria, host scripts, automated testing, parallel collaboration, sub-agents and cooperation with the user. It directs readers to `patterns.md` before searching domain-specific files.
- `experience/patterns.md` explicitly references this document under:
  - **Pattern E:** measurement method errors, offline success versus real-device failure, runtime switching versus cold boot, and host-script issues.
  - **Pattern F:** lower-layer success versus user usability, including `hci0 up`, camera capture and shadow-buffer display.
  - **Pattern H:** limits on copying another implementation and comparing foundations before reuse.
  - **Pattern I:** unreliable recovery paths, including candidate-kernel/GPU hang/shutdown failure.
- The source body itself contains no direct links to these files. These are explicit collection-index and pattern-index relationships, not inline links in the document.

## 5. Observer-reconstructed correspondences

These are cross-entry observations, not a source-defined formal model:

- **Evidence surfaces:** service/probe/emulator state, hardware indicators, user-perceptible output and human confirmation are distinct.
- **The test system is also tested:** criteria, harnesses, data, host scripts and observation methods can produce false results.
- **Falsifiability:** negative inputs, controls and second data points challenge apparent passes and root-cause explanations.
- **Clean-state dependence:** boot identity, clean boots, RAM images, session state and prior package/slot contents affect attribution.
- **Operator/task serialization:** one operator per device, boot-ID-bound evidence and constrained task briefs reduce interference.
- **Knowledge storage versus delivery:** the candidate-kernel incident distinguishes a rule's presence in the library from its presence in the operator's task instructions.
- **Recovery as an experimental constraint:** the ability to regain device access constrains which tests can safely be performed.
- **Automation versus physical confirmation:** injected software events and real physical interaction remain distinct evidence.

## 6. Unestablished or underspecified boundaries

The source does not define:
- a universal test route or complete orchestration model;
- a formal evidence state machine or transition rules;
- a device-locking or ownership mechanism for the one-operator rule;
- a complete schema for boot-ID-bound evidence;
- a universal representation for negative test inputs;
- a general sub-agent lifecycle or task-template format;
- how to select the single verification command for every possible sub-agent task;
- technical validation of the specific safety procedures and kernel settings across devices and kernel configurations.

The title's four themes are not formal body categories. GNOME/2026-specific cases remain labelled with their stated applicability rather than generalized.

These absences are preserved as source boundaries, not treated as defects to repair in this pass.

## 7. Bounded sufficiency

For Pass 1's structural and linguistic purpose, the pitfall sequence, evidence distinctions, test-tool and measurement failure surfaces, host/device/user boundaries, diagnostic and clean-boot constraints, collaboration and continuity rules, safety incident, contribution boundary, and explicit Pattern E/F/H/I relationships have been recovered.

The remaining questions do not alter what the source states and do not block this document-level reconstruction. Further investigation would require a changed scope or new discriminating evidence, such as actual task templates, evidence records, orchestration code or device-specific validation.

## 8. Position change and next action

The experience-library traversal has reached its final document as ordered by `experience/README.md`: `experience/testing-and-process.md`.

This does **not** by itself establish that the entire Pass 1 source scope is complete. The next action is to compare the declared Pass 1 scope against `translation/INVENTORY.md` and the original repository inventory, identifying any remaining source or integration documents that belong to this pass. Do not infer completion merely from reaching the end of the experience index.

Pass 1 purpose remains unchanged. No new Pass is created. Pass number, document position and session checkpoint number remain separate coordinates.

## 9. Re-entry sequence

1. Read `reconstruction/zh/continuation/current-position.md`.
2. Read this checkpoint, then Checkpoint 11 and earlier checkpoints as needed.
3. Read `reconstruction/zh/README.md` and `translation/INVENTORY.md`.
4. Revisit `experience/README.md`, `experience/patterns.md` (Patterns E/F/H/I), `references/verification.md`, `references/discipline.md` and relevant source/reconstruction records as needed.
5. Reconstruct the remaining Pass 1 scope from the inventory and initial source baseline. Do not assume that completion of the experience library means completion of the whole source repository.

## 10. Source and validation boundary

- Source repository: `incus-skills/android-mainline-port`
- Source branch: `main`
- Reconstruction branch: `translation/english-reconstruction`
- Source blob SHA: `ff1dbea0833dc9e9eb2f668000855f73ec0a9599`
- Translation source comparison: performed.
- Structural reconstruction: performed.
- Technical validation: not performed.
- Real-device operational validation: not performed.
- Whole-skill English validation: not performed.
