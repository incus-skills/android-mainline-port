# Phase 0 — Source baseline and inventory

## 1. Purpose and scope

This inventory establishes the source baseline for the English reconstruction of `android-mainline-port`.

It records repository identity, source revision, file coverage, existing English material, and the initial document-role map. It is an inventory, not a translation, technical audit, or proposed redesign.

The Chinese operational documents remain the source of authority for this reconstruction. `README.en.md` is useful English orientation material, but its existence does not establish that the operational skill has been translated.

## 2. Repository baseline

| Item | Baseline |
|---|---|
| Repository | `incus-skills/android-mainline-port` |
| Source branch | `main` |
| Source commit | `5aa6297f40166bec966194d46795c4fd16b533db` |
| Source commit date | 2026-10-23 14:39:59 UTC |
| Source tree SHA | `58a97a59ded09e79c15e2e1a081907ef96581a34` |
| Reconstruction branch | `translation/english-reconstruction` |
| Reconstruction branch head at inventory | `ed9aaab9c553400ba03ffdda4c28f7c0598b580e` |
| Reconstruction branch tree SHA | `7e6913899c3e1034f40fe449f9bd34618baa4ed6` |
| Branch relationship | 2 commits ahead of `main`, 0 behind |
| Source changes on reconstruction branch | None observed; comparison reports only two added files under `translation/` |

The source branch head is the baseline for this inventory. If `main` advances during the work, the source revision must be rechecked and any affected analysis or translation reconsidered.

### Existing reconstruction-branch commits

- `c7438efe054f48fb3c2b3b5485e7045bb1451f9e` — add translation/reconstruction workflow.
- `ed9aaab9c553400ba03ffdda4c28f7c0598b580e` — add initial translation glossary.

## 3. Repository shape

The `main` tree contains 28 files:

- 5 root or integration files;
- 6 framework/reference documents;
- 15 experience-library documents;
- 2 supporting image assets.

The reconstruction branch contains those same 28 source files plus the two existing files under `translation/`.

## 4. File inventory and initial role map

Statuses below describe translation/reconstruction work, not the maturity or correctness of the source material.

Status meanings are defined in [README.md](README.md):
- `uninspected`: not yet analysed in full for its role and dependencies;
- `analysed`: role and dependencies documented;
- `translated`: English draft exists;
- `reviewed`: checked against the source;
- `validated`: checked in the whole-skill context.

### Root and integration files

| Source path | Initial role | English/reconstruction position | Status |
|---|---|---|---|
| `SKILL.md` | Agent Skill entry point; activation, navigation, workflow overview, discipline, decision principles and file index | [`translation/en/SKILL.md`](en/SKILL.md); source-based technical/semantic review completed in Pass 3 / Checkpoint 02 | `reviewed` |
| `README.md` | Chinese repository-level description, installation, examples, scope, risks and contribution guidance | [`README.en.md`](../README.en.md) is the English counterpart; source comparison and translation review completed in Pass 2 | `reviewed` |
| `README.en.md` | Existing English repository orientation and installation guide | Compared against `README.md`; one wording correction recorded in Pass 2 | `reviewed` |
| `LICENSE` | Repository licence and attribution conditions | Preserve applicable copyright and MIT licence notice in derived distribution | Preserve; no translation assumed |
| `.claude-plugin/marketplace.json` | Claude plugin marketplace and skill registration metadata | Integration artifact; Chinese descriptions may need a later compatibility/localisation decision | `uninspected` |

### Supporting assets

| Source path | Initial role | Treatment |
|---|---|---|
| `docs/assets/gzh-qr.png` | WeChat account/community reference image | Non-text asset; retain or explicitly exclude from any English-facing distribution |
| `docs/assets/xhs-qr.jpg` | Xiaohongshu community reference image | Non-text asset; retain or explicitly exclude from any English-facing distribution |

### Framework references

These documents are described by `SKILL.md` as the relatively stable outer framework. Their precise authority, dependencies and operational effects still require document-level reconstruction.

| Source path | Initial role from repository indexes | Status |
|---|---|---|
| `references/process.md` | Detailed actions, outputs and completion criteria for the nine project stages | `reviewed` |
| `references/discipline.md` | Required project records, technical-debt handling, workspace structure, attribution, estimation and sub-agent work | `reviewed` |
| `references/decisions.md` | Technical decision defaults, conditional rules, mechanisms and limits | `reviewed` |
| `references/build-environment.md` | Host, toolchain, ccache, output directories, root-filesystem builds and build efficiency | `reviewed` |
| `references/verification.md` | Status vocabulary, evidence, cold-boot checks, automation and performance budgets | `reviewed` |
| `references/ubuntu.md` | Ubuntu layering, package boundaries, boot integration and distribution decoupling | `reviewed` |

### Experience library

The repository indexes describe this as an evolving collection of domain-specific routes, completion criteria and concrete failure experience. `experience/README.md` establishes the collection's navigation, entry format and maintenance rules; `patterns.md` abstracts recurring failure classes across domains.

| Source path | Initial domain or role | Status |
|---|---|---|
| `experience/README.md` | Experience-library index, usage, entry format, severity markers and maintenance rules | `reviewed` |
| `experience/patterns.md` | Cross-domain problem patterns and generic diagnostic approaches | `uninspected` |
| `experience/bootloader-and-boot.md` | Bootloader, first boot, debug channel and recovery | `uninspected` |
| `experience/kernel-and-resources.md` | Kernel configuration, resource ownership/reclamation, PMIC and device-tree values | `uninspected` |
| `experience/display.md` | Firmware display handoff, native display takeover, panel lifecycle and display behaviour | `uninspected` |
| `experience/gpu.md` | Adreno kernel support, firmware, Mesa and graphics stack | `uninspected` |
| `experience/touch-and-input.md` | Touch, keys, keyboard accessories, stylus and fingerprint | `uninspected` |
| `experience/audio.md` | Audio DSP, AudioReach, amplifiers, microphones, UCM and PipeWire | `uninspected` |
| `experience/wireless.md` | Wi-Fi and Bluetooth | `uninspected` |
| `experience/power-and-time.md` | Battery, charging, suspend, RTC/system time, watchdog and thermal management | `translated` |
| `experience/sensors-camera-media.md` | Sensors, rotation, camera and hardware video decoding | `uninspected` |
| `experience/usb-and-storage.md` | USB/Type-C, external display, storage, partitioning, dual boot, flashing and installation | `translated` |
| `experience/ubuntu-userspace.md` | Ubuntu packaging, dracut, snap and GNOME userspace integration | `translated` |
| `experience/build-and-release.md` | Build pipeline, caching, reproducibility and release | `uninspected` |
| `experience/testing-and-process.md` | Test judgement, automation, collaboration and process experience | `uninspected` |

## 5. Initial structural observations

These are observations from the repository tree and its indexes, not final conclusions about the skill's architecture.

1. The source explicitly distinguishes a relatively stable outer framework (`SKILL.md` and `references/`) from an experience library that is expected to grow as projects add confirmed experience.
2. The entry point provides multiple activation paths: new project, mid-project takeover, technical decision, troubleshooting and assessment-only.
3. The process is presented as nine stages, but the source also identifies permitted parallel work. It should not be modelled as a purely linear sequence without preserving those exceptions.
4. The experience library is not merely a collection of anecdotes. Its index describes reusable routes, completion criteria and structured failure records; its maintenance rules distinguish confirmed root causes from unresolved problems.
5. The source repeatedly distinguishes implementation signals from user-observable or hardware-backed success. Verification is therefore a cross-cutting concern, not simply a final testing stage.
6. Ubuntu is the concrete userspace baseline, while the framework claims broader hardware/process applicability and provides an explicit distribution adaptation boundary.
7. Plugin registration, human-facing installation guidance and agent-facing operational instructions are separate surfaces. Their compatibility and consistency should be considered during whole-skill validation.

## 6. Authority and evidence boundaries

- **Source authority for this reconstruction:** the repository content at the recorded `main` commit, subject to upstream changes.
- **Existing English aid:** `README.en.md`; useful for orientation and terminology, but not a substitute for the Chinese operational documents.
- **Methodological reference:** `skills-development`; informs the investigation but does not automatically govern or replace the Android skill's domain-specific structure.
- **Technical correctness:** not established by inventory or translation. Claims about Android hardware behaviour, kernel interfaces, firmware and current upstream state require their own evidence and, where relevant, real-device validation.
- **Translation fidelity:** must be assessed separately from technical correctness and operational validation.

## 7. Open items for subsequent phases

1. Reconstruct the actual reading/dependency graph from links and operational instructions, rather than treating the directory layout as the complete architecture.
2. Determine which rules are global skill requirements, which are process-stage requirements, which are domain-specific recommendations and which are examples derived from particular devices or versions.
3. Identify source claims whose validity is time-sensitive (kernel versions, upstream status, driver support, distribution versions and agent installation conventions).
4. Decide English target paths and packaging/localisation treatment only after the source relationships and consumer expectations are better understood.
5. Preserve distinctions between source requirements, observed project experience, inferred rationale and unresolved questions.

## 8. Phase 0 exit assessment

**Baseline and file scope are explicit.** The repository tree, source revision, existing English material, source-document set and initial document roles have been recorded.

**Phase 0 is complete as an inventory.** This does not complete Phase 1 structural reconstruction, establish technical correctness, or validate an English skill. Those remain separate tasks.


## 9. Reconstruction progress checkpoint

This section records work completed after the Phase 0 baseline above; it does not rewrite the historical baseline.

| Source document | Reconstruction / English artifact | Current status | Notes |
|---|---|---|---|
| `SKILL.md` | [`translation/en/SKILL.md`](en/SKILL.md) | `reviewed` | Source-based semantic review completed in Pass 3 / Checkpoint 02; two scope/meaning refinements recorded. External technical and activation validation remain outstanding. [Review record](reviews/SKILL.md.md). |
| `references/process.md` | [`translation/en/references/process.md`](en/references/process.md) | `reviewed` | Pass 3 source-based semantic review completed; no correction required. External technical validation remains outstanding. [Review record](reviews/references/process.md.md). |
| `references/discipline.md` | [`translation/en/references/discipline.md`](en/references/discipline.md) | `reviewed` | Pass 3 source-based semantic review completed; no correction required. External technical validation remains outstanding. [Review record](reviews/references/discipline.md.md). |
| `references/decisions.md` | [`translation/en/references/decisions.md`](en/references/decisions.md) | `reviewed` | Pass 3 source-based semantic review completed; no correction required. Domain-specific ambiguities and external technical validation remain open. [Review record](reviews/references/decisions.md.md). |
| `references/verification.md` | [`translation/en/references/verification.md`](en/references/verification.md) | `reviewed` | Pass 3 source-based semantic review completed; no correction required. Evidence boundaries and independent status vocabularies preserved; technical claims and test-method suitability remain unvalidated. [Review record](reviews/references/verification.md.md). |
| `references/build-environment.md` | [`translation/en/references/build-environment.md`](en/references/build-environment.md) | `reviewed` | Pass 3 source-based semantic review completed; no correction required. Build-time claims and tool compatibility remain unvalidated. [Review record](reviews/references/build-environment.md.md). |
| `references/ubuntu.md` | [`translation/en/references/ubuntu.md`](en/references/ubuntu.md) | `reviewed` | Pass 3 source-based semantic review completed; release-attachment bundle wording corrected. Layer boundaries and distribution-specific scope preserved; technical claims remain unvalidated. [Review record](reviews/references/ubuntu.md.md). |
| `experience/README.md` | [`translation/en/experience/README.md`](en/experience/README.md) | `reviewed` | Pass 3 source-based semantic review completed; no correction required. Collection scope, retrieval routes, knowledge-admission boundary and maintenance rules preserved; technical claims remain unvalidated. [Review record](reviews/experience/README.md.md). |
| `experience/patterns.md` | [`translation/en/experience/patterns.md`](en/experience/patterns.md) | `translated` | Nine cross-domain diagnostic categories, recognition symptoms, general handling routes and domain references translated; category overlap and technical-validation boundary preserved. [Review record](reviews/experience/patterns.md.md). |
| `experience/bootloader-and-boot.md` | [`translation/en/experience/bootloader-and-boot.md`](en/experience/bootloader-and-boot.md) | `translated` | Recommended boot route, RAM/persistent completion criteria, first-boot checklist, boot-chain pitfalls and unresolved recovery questions translated; technical claims remain unvalidated. [Review record](reviews/experience/bootloader-and-boot.md.md). |
| `experience/kernel-and-resources.md` | [`translation/en/experience/kernel-and-resources.md`](en/experience/kernel-and-resources.md) | `translated` | Kernel configuration, provider/consumer relationships, resource reclamation, module availability, regulator/OPP constraints and unresolved timing question translated. Chinese descriptive reconstruction and translation review recorded separately. [Review record](reviews/experience/kernel-and-resources.md.md). |
| `experience/display.md` | [`translation/en/experience/display.md`](en/experience/display.md) | `translated` | Firmware-display borrowing, native takeover, acceptance criteria, lifecycle and display-specific failure records translated; technical claims remain unvalidated. [Review record](reviews/experience/display.md.md). |
| `experience/gpu.md` | [`translation/en/experience/gpu.md`](en/experience/gpu.md) | `translated` | Kernel/firmware/Mesa route, hardware-rendering acceptance, GPU and Mesa failure records, explicit resource relationship and unresolved new-GPU issues translated; technical claims remain unvalidated. [Review record](reviews/experience/gpu.md.md). |
| `experience/sensors-camera-media.md` | [`translation/en/experience/sensors-camera-media.md`](en/experience/sensors-camera-media.md) | `translated` | Sensor/automatic-rotation, camera and hardware-video routes, layered acceptance evidence, fresh-boot state, application-level availability, explicit pattern references and unresolved boundaries translated; technical claims remain unvalidated. [Review record](reviews/experience/sensors-camera-media.md.md). |
| `experience/usb-and-storage.md` | [`translation/en/experience/usb-and-storage.md`](en/experience/usb-and-storage.md) | `translated` | USB role and Type-C route, storage identity, active-slot rotation, sparse-image write semantics, backup consistency, recovery channel and installation boundaries translated; technical claims remain unvalidated. [Review record](reviews/experience/usb-and-storage.md.md). |
| `experience/ubuntu-userspace.md` | [`translation/en/experience/ubuntu-userspace.md`](en/experience/ubuntu-userspace.md) | `translated` | Ubuntu package lifecycle, initramfs hooks, dracut, snap/GPU provider-consumer behaviour, GNOME Initial Setup, systemd cold-boot failures and explicit architecture/pattern references translated; technical claims remain unvalidated. [Review record](reviews/experience/ubuntu-userspace.md.md). |
| `experience/build-and-release.md` | [`translation/en/experience/build-and-release.md`](en/experience/build-and-release.md) | `translated` | Build-time measurement, ccache/OUT identity, execution environments, reproducibility controls, cleanup, release integrity, package-version ordering and version cadence translated; technical claims remain unvalidated. [Review record](reviews/experience/build-and-release.md.md). |

The English path `translation/en/` is the current working destination for translated documents. The Chinese source tree remains unchanged.

The process translation preserves the source's stated stage overlaps, Stage 5/6 relationship, per-peripheral cycle, version rhythm, and the absence of a Stage 8 completion criterion. This is a translation record, not evidence that the underlying Android procedures have been technically validated.

### Translation record

Translation commentary, reconstruction decisions and first-pass review are preserved separately from functional skill files under `translation/reviews/`.

- [`translation/reviews/SKILL.md.md`](reviews/SKILL.md.md) records the `SKILL.md` source revision, translation intent, terminology decisions, review observations and current validation boundary.
- [`translation/reviews/references/discipline.md.md`](reviews/references/discipline.md.md) records the `references/discipline.md` source revision, status-vocabulary mapping, structural observations, ambiguities and review boundary.
- [`translation/reviews/references/process.md.md`](reviews/references/process.md.md) records the `references/process.md` source revision, nine-stage procedure, concurrency rules, terminology decisions, unresolved boundaries and review status.
- [`translation/reviews/references/decisions.md.md`](reviews/references/decisions.md.md) records the `references/decisions.md` source revision, decision-entry structure, terminology choices, structural observations, ambiguities and the explicit separation between translation and technical validation.
- [`translation/reviews/references/verification.md.md`](reviews/references/verification.md.md) records the `references/verification.md` source revision, seven-label evidence structure, cross-document vocabulary distinctions, structural observations and review boundary.
- [`translation/reviews/references/build-environment.md.md`](reviews/references/build-environment.md.md) records the `references/build-environment.md` source revision, build-infrastructure structure, terminology choices, cross-document relationships and review boundary.
- [`translation/reviews/references/ubuntu.md.md`](reviews/references/ubuntu.md.md) records the `references/ubuntu.md` source revision, layered model, package responsibilities, terminology choices, cross-document relationships and review boundary.
- [`translation/reviews/experience/README.md.md`](reviews/experience/README.md.md) records the `experience/README.md` source revision, collection role, navigation and maintenance structure, terminology choices, unresolved boundaries and review boundary.
- [`translation/reviews/experience/patterns.md.md`](reviews/experience/patterns.md.md) records the `experience/patterns.md` source revision, nine-category diagnostic structure, cross-document relationships, terminology choices, overlapping-category ambiguity and review boundary.
- [`translation/reviews/experience/bootloader-and-boot.md.md`](reviews/experience/bootloader-and-boot.md.md) records the `experience/bootloader-and-boot.md` source revision, boot route, acceptance criteria, pitfall structure, terminology choices, unresolved recovery questions and review boundary.
- [`translation/reviews/experience/kernel-and-resources.md.md`](reviews/experience/kernel-and-resources.md.md) records the `experience/kernel-and-resources.md` source revision, Category B development, provider/consumer and timing distinctions, cross-document relationships, terminology choices and technical-validation boundary.
- [`translation/reviews/experience/display.md.md`](reviews/experience/display.md.md) records the `experience/display.md` source revision, route distinctions, native-takeover sequence, acceptance model, diagnostic correspondences, cross-document relationships and technical-validation boundary.
- [`translation/reviews/experience/gpu.md.md`](reviews/experience/gpu.md.md) records the `experience/gpu.md` source revision, layered graphics route, hardware-rendering acceptance criteria, Mesa/GPU failure surfaces, explicit resource relationship, terminology choices and technical-validation boundary.
- [`translation/reviews/experience/touch-and-input.md.md`](reviews/experience/touch-and-input.md.md) records the touch architecture distinctions, panel/touch lifecycle, input acceptance, keyboard electrical/protocol separation, fingerprint boundary, explicit cross-file relationships and the absence of stylus-specific body content.
- [`translation/reviews/experience/audio.md.md`](reviews/experience/audio.md.md) records the AudioReach/ADSP route, layered evidence boundaries, topology and amplifier distinctions, PipeWire/UCM and fresh-state issues, explicit collection-level links, unresolved risks and technical-validation boundary.
- [`translation/reviews/experience/wireless.md.md`](reviews/experience/wireless.md.md) records the separate Wi-Fi/Bluetooth routes and acceptance sequences, firmware/board-data and device-identity distinctions, regulatory/lifecycle cases, explicit pattern references, unresolved issues and technical-validation boundary.
- [`translation/reviews/experience/power-and-time.md.md`](reviews/experience/power-and-time.md.md) records the power/time route, measurement-versus-status distinction, suspend and clock evidence, explicit pattern references, watchdog/thermal coverage gaps, unresolved issues and technical-validation boundary.
- [`translation/reviews/experience/sensors-camera-media.md.md`](reviews/experience/sensors-camera-media.md.md) records the sensor/rotation, hardware-video and camera routes, layered acceptance evidence, explicit Pattern D/F/I references, unresolved camera boundaries and technical-validation boundary.
- [`translation/reviews/experience/usb-and-storage.md.md`](reviews/experience/usb-and-storage.md.md) records USB/Type-C and storage routes, active-slot image-set rotation, FILL/DONT_CARE semantics, backup limits, explicit Pattern A/D/I references and the absence of a dedicated unresolved section.
- [`translation/reviews/experience/ubuntu-userspace.md.md`](reviews/experience/ubuntu-userspace.md.md) records the separation from the Ubuntu architecture reference, package/initramfs lifecycle, snap provider-consumer boundary, GNOME setup contexts, cold-boot/systemd cases, explicit Pattern C/D/G references and the unresolved inhibitor-lock cause.
- [`translation/reviews/experience/build-and-release.md.md`](reviews/experience/build-and-release.md.md) records the build-environment reference boundary, measurement and execution-context distinctions, cache/artifact identity, reproducibility inputs, release integrity, external-documentation boundary and explicit Pattern D reference.
- [`translation/reviews/experience/testing-and-process.md.md`](reviews/experience/testing-and-process.md.md) records evidence and acceptance distinctions, test-tool and measurement failure modes, host/device and user boundaries, collaboration constraints, the candidate-kernel safety incident, explicit Pattern E/F/H/I references and the technical-validation boundary.

### Chinese reconstruction surface

A separate descriptive reconstruction surface has now been established under `reconstruction/zh/`.

- [`reconstruction/zh/README.md`](../reconstruction/zh/README.md) defines its purpose, authority boundary and relationship to the original Chinese source, English reconstruction and functional translation.
- [`reconstruction/zh/kernel-and-resources.md`](../reconstruction/zh/kernel-and-resources.md) records the retrospective Chinese reconstruction of the kernel/resource document, including observer position, source provenance, its relationship to problem pattern B and `display.md`, recovered mechanisms, unresolved questions and the current sufficiency boundary.
- [`reconstruction/zh/display.md`](../reconstruction/zh/display.md) records the retrospective reconstruction of the display routes, takeover sequence, acceptance evidence, diagnostic correspondences, resource relationships and unresolved boundaries.
- [`reconstruction/zh/gpu.md`](../reconstruction/zh/gpu.md) records the retrospective reconstruction of the GPU and graphics-stack route, completion criteria, failure surfaces, explicit cross-document links, observer correspondences and unresolved boundaries.
- [`reconstruction/zh/touch-and-input.md`](../reconstruction/zh/touch-and-input.md) records the touch/input architecture distinctions, user-facing acceptance, panel lifecycle and electrical/protocol failure surfaces, explicit cross-document relationships, unresolved issues and the stylus coverage gap.
- [`reconstruction/zh/audio.md`](../reconstruction/zh/audio.md) records the AudioReach/ADSP route, hardware topology, layered machine/electrical/human evidence, cross-document correspondences, safety boundary and unresolved issues.
- [`reconstruction/zh/wireless.md`](../reconstruction/zh/wireless.md) records the separate Wi-Fi and Bluetooth routes, acceptance sequences, failure mechanisms, explicit pattern references, observer-reconstructed correspondences and unresolved boundaries.
- [`reconstruction/zh/power-and-time.md`](../reconstruction/zh/power-and-time.md) records battery/charging distinctions, RTC and suspend routes, clock-measurement boundaries, explicit pattern references, title/body coverage gaps and unresolved issues.

This surface is our descriptive reconstruction, not a duplicate of the original source or a claim about author intent. Its records are retrospective where indicated. Author feedback, if obtained later, must be preserved as a separate source rather than silently merged into the original reconstruction.

Future document translations should retain equivalent decision records rather than leaving them only in conversation. Each translation record is a separate reconstruction artifact; it is not part of the functional skill text.


### Session ingress and traversal checkpoints

The Chinese reconstruction surface includes a session-ingress record so a new session can re-establish the active observer position without relying on conversation history.

- [Current position](../reconstruction/zh/continuation/current-position.md) is the active entry point. It identifies the current traversal pass, document position, next source and re-entry reading sequence.
- [Pass 1 / Checkpoint 01](../reconstruction/zh/continuation/checkpoints/pass-1-checkpoint-01.md) preserves the retrospective observer position, evidence boundaries, continuation rationale and cold-start success criteria.
- [Pass 1 / Checkpoint 02](../reconstruction/zh/continuation/checkpoints/pass-1-checkpoint-02.md) records the display reconstruction and the transition to GPU.
- [Pass 1 / Checkpoint 03](../reconstruction/zh/continuation/checkpoints/pass-1-checkpoint-03.md) records the GPU reconstruction and the transition to touch and input.
- [Pass 1 / Checkpoint 04](../reconstruction/zh/continuation/checkpoints/pass-1-checkpoint-04.md) records the touch-and-input reconstruction and the transition to audio.
- [Pass 1 / Checkpoint 05](../reconstruction/zh/continuation/checkpoints/pass-1-checkpoint-05.md) records the audio reconstruction and the transition to wireless.
- [Pass 1 / Checkpoint 06](../reconstruction/zh/continuation/checkpoints/pass-1-checkpoint-06.md) records the wireless reconstruction and the transition to power and time.

A **Pass** identifies one complete traversal of a declared scope from beginning to end. A pass may span multiple sessions; session checkpoints record movement within that pass. A later traversal starts a new pass and should preserve a comparison with earlier observations. Pass number must not be conflated with document position or session/checkpoint number.


## 10. Pass 1 scope reconciliation and closure

Pass 1 is complete for the operational skill core defined by the Phase 1 exit condition in `translation/README.md): `SKILL.md`, the six framework references and the fifteen experience-library documents. Each has an English counterpart and a separate translation review record. The final experience document, `experience/testing-and-process.md`, was completed at Pass 1 / Checkpoint 12.

The remaining six files in the original 28-file baseline were classified separately:

| Material | Pass 1 treatment | Deferred work |
|---|---|---|
| `README.md` | Read as repository-facing overview, installation guide, examples and high-level content map; not an operational procedure document | Detailed user-facing translation/localisation and consistency review |
| `README.en.md` | Read as existing English orientation aid, not source authority | Whole-document consistency and current installation compatibility review |
| `.claude-plugin/marketplace.json` | Inspected as plugin registration metadata | Localisation and integration compatibility decision |
| `LICENSE` | Preserve unchanged | Confirm preservation in later distribution work |
| `docs/assets/gzh-qr.png` | Non-text community reference image | Retain/exclude decision for English distribution |
| `docs/assets/xhs-qr.jpg` | Non-text community reference image | Retain/exclude decision for English distribution |

This is consistent with the translation workflow's later phases for supporting examples/user-facing material and whole-skill integration validation. Pass 1 completion does not imply these deferred tasks, technical validation or whole-skill operational validation are complete.

See [Pass 1 scope reconciliation](../reconstruction/zh/continuation/checkpoints/pass-1-scope-reconciliation.md) for the evidence boundary and closure rationale.


## 11. Pass 2 — repository README review

- `README.md` is mapped to the existing root-level `README.en.md`; the latter was reviewed against the complete Chinese source and one wording correction was made.
- Review record: [`translation/reviews/README.md.md`](reviews/README.md.md).
- Descriptive reconstruction: [`reconstruction/zh/README.md`](../reconstruction/zh/README.md).
- This does not establish current technical accuracy or external installation compatibility. QR asset treatment and marketplace metadata remain separate decisions.
