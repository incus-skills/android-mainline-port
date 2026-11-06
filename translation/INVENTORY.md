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
| `SKILL.md` | Agent Skill entry point; activation, navigation, workflow overview, discipline, decision principles and file index | Operational English counterpart not yet created | `uninspected` |
| `README.md` | Chinese repository-level description, installation, examples, scope, risks and contribution guidance | `README.en.md` is the existing English orientation counterpart; consistency review remains necessary | `uninspected` |
| `README.en.md` | Existing English repository orientation and installation guide | Existing English material; not evidence that operational references and experience records are translated | Review required |
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
| `references/process.md` | Detailed actions, outputs and completion criteria for the nine project stages | `uninspected` |
| `references/discipline.md` | Required project records, technical-debt handling, workspace structure, attribution, estimation and sub-agent work | `uninspected` |
| `references/decisions.md` | Technical decision defaults, conditional rules, mechanisms and limits | `uninspected` |
| `references/build-environment.md` | Host, toolchain, ccache, output directories, root-filesystem builds and build efficiency | `uninspected` |
| `references/verification.md` | Status vocabulary, evidence, cold-boot checks, automation and performance budgets | `uninspected` |
| `references/ubuntu.md` | Ubuntu layering, package boundaries, boot integration and distribution decoupling | `uninspected` |

### Experience library

The repository indexes describe this as an evolving collection of domain-specific routes, completion criteria and concrete failure experience. `experience/README.md` establishes the collection's navigation, entry format and maintenance rules; `patterns.md` abstracts recurring failure classes across domains.

| Source path | Initial domain or role | Status |
|---|---|---|
| `experience/README.md` | Experience-library index, usage, entry format, severity markers and maintenance rules | `uninspected` |
| `experience/patterns.md` | Cross-domain problem patterns and generic diagnostic approaches | `uninspected` |
| `experience/bootloader-and-boot.md` | Bootloader, first boot, debug channel and recovery | `uninspected` |
| `experience/kernel-and-resources.md` | Kernel configuration, resource ownership/reclamation, PMIC and device-tree values | `uninspected` |
| `experience/display.md` | Firmware display handoff, native display takeover, panel lifecycle and display behaviour | `uninspected` |
| `experience/gpu.md` | Adreno kernel support, firmware, Mesa and graphics stack | `uninspected` |
| `experience/touch-and-input.md` | Touch, keys, keyboard accessories, stylus and fingerprint | `uninspected` |
| `experience/audio.md` | Audio DSP, AudioReach, amplifiers, microphones, UCM and PipeWire | `uninspected` |
| `experience/wireless.md` | Wi-Fi and Bluetooth | `uninspected` |
| `experience/power-and-time.md` | Battery, charging, suspend, RTC/system time, watchdog and thermal management | `uninspected` |
| `experience/sensors-camera-media.md` | Sensors, rotation, camera and hardware video decoding | `uninspected` |
| `experience/usb-and-storage.md` | USB/Type-C, external display, storage, partitioning, dual boot, flashing and installation | `uninspected` |
| `experience/ubuntu-userspace.md` | Ubuntu packaging, dracut, snap and GNOME userspace integration | `uninspected` |
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
| `SKILL.md` | [`translation/en/SKILL.md`](en/SKILL.md) | `translated` | English translation committed from the supplied draft; source frontmatter description translated. Translation review notes remain separate from the functional skill text. Semantic/technical review remains outstanding. |
| `references/process.md` | [`translation/en/references/process.md`](en/references/process.md) | `translated` | Document-level structure reconstructed; English draft committed. Semantic/technical review remains outstanding. |
| `references/discipline.md` | [`translation/en/references/discipline.md`](en/references/discipline.md) | `translated` | Record responsibilities, privacy boundaries, attribution, estimation and sub-agent constraints translated; semantic/technical review remains outstanding. [Review record](reviews/references/discipline.md.md). |
| `references/decisions.md` | [`translation/en/references/decisions.md`](en/references/decisions.md) | `translated` | Nine-section conditional decision catalogue translated; technical claims deliberately not validated in this pass. [Review record](reviews/references/decisions.md.md). |

The English path `translation/en/` is the current working destination for translated documents. The Chinese source tree remains unchanged.

The process translation preserves the source's stated stage overlaps, Stage 5/6 relationship, per-peripheral cycle, version rhythm, and the absence of a Stage 8 completion criterion. This is a translation record, not evidence that the underlying Android procedures have been technically validated.

### Translation record

Translation commentary, reconstruction decisions and first-pass review are preserved separately from functional skill files under `translation/reviews/`.

- [`translation/reviews/SKILL.md.md`](reviews/SKILL.md.md) records the `SKILL.md` source revision, translation intent, terminology decisions, review observations and current validation boundary.
- [`translation/reviews/references/discipline.md.md`](reviews/references/discipline.md.md) records the `references/discipline.md` source revision, status-vocabulary mapping, structural observations, ambiguities and review boundary.
- [`translation/reviews/references/decisions.md.md`](reviews/references/decisions.md.md) records the `references/decisions.md` source revision, decision-entry structure, terminology choices, structural observations, ambiguities and the explicit separation between translation and technical validation.

Future document translations should retain equivalent decision records rather than leaving them only in conversation. Each translation record is a separate reconstruction artifact; it is not part of the functional skill text.
