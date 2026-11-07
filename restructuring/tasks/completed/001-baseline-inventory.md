# Task 001 — Baseline inventory and provenance assessment

**Worker status:** COMPLETE — read-only assessment and handoff report  
**Orchestrator acceptance:** Pending  
**Execution date:** 2026-11-07  
**Repository:** `incus-skills/android-mainline-port`  
**Ingress branch:** `translation/english-reconstruction`  
**Ingress commit:** `219f70ea64ff540f021f4a24e611c6989a38414b`  
**Task record:** `restructuring/tasks/active/001-baseline-inventory.md` (blob `34a81dc5d4956712fbf022b664c0537c9ac052a4`)

## 1. Ingress identity and method

The ingress branch HEAD was resolved before inspection as `219f70ea64ff540f021f4a24e611c6989a38414b`. Its recursive Git tree is complete (`truncated: false`) and contains 182 entries. The task record was read in full and its ingress revision was fixed to that branch HEAD for this assessment.

Read-only repository views used:

- Repository metadata: `GET /repos/incus-skills/android-mainline-port`.
- Branch inventory: `GET /repos/incus-skills/android-mainline-port/branches?per_page=100`.
- Commit metadata: `GET /repos/{owner}/{repo}/commits/{sha}`.
- Recursive trees: `GET /repos/{owner}/{repo}/git/trees/{sha}?recursive=1`.
- Exact file views: GitHub repository file/content views at the ingress ref.
- Tree comparison: the upstream baseline tree, the fork default-branch tree and the ingress-branch tree were compared by path and blob/tree SHA.

No local checkout, shell command, test runner, validator, installer or provisioning workflow was run. Findings below distinguish repository facts from inferences and recommendations.

## 2. Repository and branch inventory

### Target repository

The target is the public repository `incus-skills/android-mainline-port`. GitHub repository metadata identifies it as a fork whose parent is `yzddmr6/android-mainline-port`. Its default branch is `main`.

At ingress, the target repository exposed exactly these branches:

| Branch | Commit | Observed role |
|---|---|---|
| `main` | `f1dc15240feaaef2b46623a98f91d95c7f6f39e1` | Repository default branch; source-language package baseline |
| `translation/english-reconstruction` | `219f70ea64ff540f021f4a24e611c6989a38414b` | English reconstruction, translation records and restructuring task work |

No separate English distribution branch, validation-harness branch or other reconstruction/validation branch appeared in the complete branch listing. This is a statement about the branches returned at ingress, not a claim that no such branch could be created later.

The fork's `main` commit `f1dc15240feaaef2b46623a98f91d95c7f6f39e1` has tree SHA `58a97a59ded09e79c15e2e1a081907ef96581a34`. The ingress branch has commit `219f70ea64ff540f021f4a24e611c6989a38414b`; its commit parent is `b8a2df7aa5c6dc8f2ffaf2be68cf5c6b2f8d0b64`.

### Upstream repository

The verified upstream/parent repository is `yzddmr6/android-mainline-port`, whose default branch is `main`. At inspection, its `main` branch pointed to `5aa6297f40166bec966194d46795c4fd16b533db`, dated 2026-10-23 14:39:59 UTC. That commit's tree SHA is also `58a97a59ded09e79c15e2e1a081907ef96581a34`.

The fork's default-branch commit and the upstream commit therefore have different commit identities but the same tree SHA. The upstream repository currently exposes only its `main` branch in the branch listing.

### Source-tree comparison at ingress

The recursive upstream baseline tree contains 33 entries. All 33 paths are present in the ingress branch tree. Comparing the common paths by blob/tree SHA shows one difference: root `README.en.md`. The other source-tree paths, including the root `SKILL.md`, `references/`, `experience/`, `README.md`, `.claude-plugin/marketplace.json`, `LICENSE` and `docs/assets/`, match the upstream baseline tree.

This supports the observation that the reconstruction branch retains the source-language package at the repository root while adding the English package and reconstruction/coordination material under separate paths. The comparison does not imply that every new path is product content.

## 3. Upstream provenance

### Evidenced source baseline

`translation/INVENTORY.md` records the Phase 0 baseline at lines 13–26:

- Repository: `incus-skills/android-mainline-port`.
- Source branch: `main`.
- Source commit: `5aa6297f40166bec966194d46795c4fd16b533db`.
- Source commit date: 2026-10-23 14:39:59 UTC.
- Source tree SHA: `58a97a59ded09e79c15e2e1a081907ef96581a34`.
- Reconstruction branch: `translation/english-reconstruction`.
- The inventory records the reconstruction branch as two commits ahead of `main`, zero behind, with no source changes observed at that recorded baseline.

The recorded commit SHA resolves in the verified upstream repository `yzddmr6/android-mainline-port` and is its current `main` HEAD. The fork's current `main` commit has the same tree SHA. The ingress reconstruction branch retains all 33 source-tree paths and all source blobs except the modified root `README.en.md`.

**Assessment:** `5aa6297f40166bec966194d46795c4fd16b533db` is a strongly evidenced candidate for the source baseline from which the English reconstruction began. The tree identity and recorded inventory corroborate one another. The distinction between the upstream commit identity (`5aa...`) and the fork's later commit identity (`f1dc...`) should be retained in any later provenance statement: they identify different commits with identical trees, not interchangeable commit IDs.

This worker does **not** select or declare the final base revision for a future English distribution branch. That decision remains with the repository owner/orchestrator.

### Relevant provenance records

- `translation/INVENTORY.md` — source baseline, original 28-file source inventory and document mapping; blob `0a8e58e9a6f45fd54c718672dc8d7a3a9056f7ee`.
- `translation/README.md` — translation method, phases, evidence and provenance principles; blob `f9bf34ae28e838ae881f9e207797f26c1b327baa`.
- `translation/reviews/` — per-document reconstruction/review records. The ingress tree contains 23 review files.
- `translation/tasks/completed/` — 17 completed worker task records, including Pass 3 source-based reviews and Pass 4 whole-skill validation operations.
- `reconstruction/zh/continuation/checkpoints/` — 50 checkpoint/scope records preserving traversal positions, evidence and acceptance history.
- `reconstruction/zh/continuation/current-position.md` — current reconstruction continuation and hold record; blob `f86d33467c65b477ede556054f41105620dad5ab`.

## 4. English package inventory

### Package tree and complete path list

The current English operational package is `translation/en/`, tree SHA `ec1ab0b8db21735d4905825872ac32dea2f5be11`. Its recursive inventory is complete and contains 22 files: one entrypoint, six framework references and fifteen experience documents.

Entrypoint:

- `translation/en/SKILL.md` — blob `b8c4e4e8e625552f9fd4dbd44e613f54ef359aec`.

Framework references:

- `translation/en/references/build-environment.md`
- `translation/en/references/decisions.md`
- `translation/en/references/discipline.md`
- `translation/en/references/process.md`
- `translation/en/references/ubuntu.md`
- `translation/en/references/verification.md`

Experience library:

- `translation/en/experience/README.md` — blob `8670ddde09f42af5591f792c626d4028bb53fb14`.
- `translation/en/experience/audio.md`
- `translation/en/experience/bootloader-and-boot.md`
- `translation/en/experience/build-and-release.md`
- `translation/en/experience/display.md`
- `translation/en/experience/gpu.md`
- `translation/en/experience/kernel-and-resources.md`
- `translation/en/experience/patterns.md`
- `translation/en/experience/power-and-time.md`
- `translation/en/experience/sensors-camera-media.md`
- `translation/en/experience/testing-and-process.md`
- `translation/en/experience/touch-and-input.md`
- `translation/en/experience/ubuntu-userspace.md`
- `translation/en/experience/usb-and-storage.md`
- `translation/en/experience/wireless.md`

The references subtree SHA is `41f3799c0dd0caa7bdef8e9b4820f45f21ace473`; the experience subtree SHA is `858f81387a8554a1c60f802bcf3bf49e4ee30114`.

### Metadata, links and dependencies

The English entrypoint frontmatter declares `name: android-mainline-port`. Its immediate parent directory is named `en`, not `android-mainline-port`. The Agent Skills directory/name constraint was examined and accepted as a static finding in Task 017: the package's content and metadata are otherwise compatible, but the current parent-directory/name relationship is not.

The English entrypoint's relative references point to `references/` and `experience/` within the package. The experience index's relative reference to `../references/ubuntu.md` also resolves within `translation/en/`. Task 017 independently checked the complete package tree and found no required source-language root file or external experience file needed to satisfy those internal references.

The repository-root `LICENSE` is outside `translation/en/`. Task 017 records this as an unresolved distribution/legal packaging question rather than a runtime dependency. The root `README.en.md`, `.claude-plugin/marketplace.json`, `docs/assets/gzh-qr.png` and `docs/assets/xhs-qr.jpg` are also outside the English package. Whether any of these should be copied, linked, retained only at repository level or excluded from a standalone English distribution requires a packaging decision.

The English tree's source-language leakage was separately reviewed in completed Task 014. The current-position record reports that the fixed English tree had no unexpected source-language text that would obstruct English readers. This is a recorded static review result, not a claim that all repository-level material is English.

### Package placement and collision observations

The source-language operational package currently occupies the repository root:

- `SKILL.md`
- `references/`
- `experience/`

The English package currently occupies `translation/en/`. Consequently, placing the English package at the normal root-level package location on an English distribution branch would overlap the current root package paths. This is a real path/ownership boundary, not permission to move or replace files in this task.

The existing root `SKILL.md` also declares `name: android-mainline-port`. The English entrypoint declares the same skill identity. The repository root is named `android-mainline-port`, whereas the nested English directory is named only `en`. The least disruptive conformant layout previously identified in Task 017 was to nest the English package under `translation/en/android-mainline-port`; a dedicated distribution branch/root is another option. Neither is selected here. The agreed restructuring boundary instead states that the eventual English edition should use a normal root-level package layout and remain close to upstream, with translation/reconstruction records separate from the operational package.

## 5. Reconstruction and coordination record inventory

### Descriptive reconstruction and continuation

The `reconstruction/zh/` tree contains 64 files:

- `reconstruction/zh/README.md`.
- Retrospective topical reconstruction records: `audio.md`, `build-and-release.md`, `display.md`, `gpu.md`, `kernel-and-resources.md`, `power-and-time.md`, `sensors-camera-media.md`, `testing-and-process.md`, `touch-and-input.md`, `ubuntu-userspace.md`, `usb-and-storage.md` and `wireless.md`.
- `reconstruction/zh/continuation/current-position.md`.
- Fifty checkpoint/scope records under `reconstruction/zh/continuation/checkpoints/`: Pass 1 Checkpoints 01–12 plus `pass-1-scope-reconciliation.md`; Pass 2 Checkpoint 01 plus `pass-2-scope-closure.md`; Pass 3 Checkpoints 01–23; and Pass 4 Checkpoints 01–12.

The current-position record identifies Pass 4 / Checkpoint 12, records Tasks 008–017 as independently accepted within their stated boundaries, and says the next translation-project operation is on hold pending owner decisions about English package identity/layout/distribution and external provisioning evidence. The record also states that Pass 4 remains open and that Task 017 acceptance does not establish runtime installation, activation, provisioning success or whole-skill completion.

The current-position record explicitly notes that the “Application to the current position” paragraph in `translation/EXECUTION-SEGMENTATION.md` is stale: it still names Pass 3 / Checkpoint 16 / `experience/wireless.md`. The current-position record and applicable checkpoint, not that stale paragraph, are the position evidence. This stale paragraph was not changed.

### Translation and coordination records

The `translation/` tree contains the following record groups:

- Governing/coordination documents: `translation/README.md`, `translation/INVENTORY.md`, `translation/GLOSSARY.md`, `translation/ORCHESTRATOR-INGRESS.md`, `translation/WORKER-INGRESS.md` and `translation/EXECUTION-SEGMENTATION.md`.
- Functional English package: the 22 files under `translation/en/` listed above.
- Review history: 23 files under `translation/reviews/` (review index, entrypoint, six references and fifteen experience documents).
- Completed translation task history: 17 files under `translation/tasks/completed/`.

Operationally useful continuation records include the current-position record, the translation README/inventory/glossary, the current ingress instructions, the execution-segmentation description, the applicable current checkpoint and any active task packet. Completed task records and earlier checkpoints are historical evidence and acceptance provenance; they should not be treated as disposable merely because they no longer define the immediate next operation. The English package itself is operational content, distinct from these coordination/reconstruction records.

The restructuring task area at ingress contains one active task, `restructuring/tasks/active/001-baseline-inventory.md`. No `restructuring/tasks/completed/` directory existed in the ingress tree. The task itself explicitly specifies `restructuring/tasks/completed/001-baseline-inventory.md` as the report path. No competing restructuring completed-record convention was present in the inspected tree; the required path is therefore followed for this handoff. The active task record is left untouched.

## 6. Validation and provisioning inventory

### Actual repository configuration

- `.claude-plugin/marketplace.json` — the only identified plugin/discovery manifest; blob `abdaaf3d1763db8108b3895b542088f201b3af16`. It declares marketplace and plugin-entry name `android-mainline-port`, `source: "./"` and `skills: ["./"]`.
- `.claude-plugin/plugin.json` is absent from the complete ingress tree. Task 017's specification review found that absence is not itself a schema defect because the marketplace entry can act as the plugin manifest.
- Root `LICENSE` — MIT licence file; not included inside `translation/en/`.
- No `.github/`, `.gitlab/`, `.forgejo/`, `scripts/`, `tests/`, `fixtures/`, install/provision directory or CI workflow/configuration was identified in the complete ingress tree. Task 017 independently records the absence of repository-local CI/provisioning configuration at its checkpoint lines 64 and 98.
- No separate validation-harness branch or harness-specific executable tooling was present in the branch inventory/tree.

### Documented installation/distribution paths

Root `README.en.md` is documentation, not executable provisioning configuration. Its Install section (approximately lines 49–124) documents:

- Skills CLI installation using `npx skills add yzddmr6/android-mainline-port -g`.
- Claude Code marketplace/plugin commands using `yzddmr6/android-mainline-port`.
- Gemini CLI installation from the upstream repository URL.
- Manual clone instructions targeting the upstream repository and a directory named `android-mainline-port`.
- A claude.ai/desktop release ZIP path.
- A prompt asking an agent to clone the upstream repository.

These commands use the upstream repository locator without specifying the reconstruction branch or `translation/en/` subdirectory. Task 017 independently checked the README and concluded that the documented routes select the source-language root package, not the English reconstruction.

The actual marketplace manifest also uses repository-root-relative `./` for both plugin source and skills component. Its current entry therefore selects the root package. The English package is not selected by this manifest.

The README's release ZIP instructions are documentation only; no repository-local release workflow or provisioning configuration was found. The external provisioning workflow referenced by the translation project's distribution intent is not present in this repository. Its source locator, branch/ref selection, package path and automation behavior cannot be established from the inspected files.

No installation, activation, provisioning, package execution, validator or workflow was run. Static documentation must not be represented as proof of successful provisioning.

## 7. Risk and ambiguity register

| Area | Evidence-backed observation | Risk / unresolved matter |
|---|---|---|
| Upstream provenance | Inventory pins upstream source commit `5aa6297...`; upstream `main` still points to it; fork `main` has the same tree SHA. | Strong candidate is evidenced, but final base selection remains an owner/orchestrator decision. Preserve the distinction between commit identity and shared tree identity. |
| Package identity | Both root and English entrypoints declare `android-mainline-port`; English package parent is `en`. | Directory/name mismatch blocks straightforward standalone package conformance. |
| Root path ownership | Source-language `SKILL.md`, `references/` and `experience/` occupy root; English equivalents are under `translation/en/`. | A root-level English package requires a branch/layout decision; do not overwrite or move current files under this task. |
| Marketplace | Actual manifest selects `./` at repository root. | Current plugin route exposes source-language root, not English package. Coexistence, replacement or separate marketplace is undecided. |
| README/install routes | Existing English README uses upstream default repository/root locators. | Current documented routes do not provision the English reconstruction branch/package. |
| External provisioning | No provisioning workflow/configuration exists in the inspected repository tree. | Required external package locator and workflow behavior remain unknown; cannot claim independent provisioning. |
| Licence | Root `LICENSE` is outside the 22-file English package. | Standalone distribution treatment must be decided. |
| Assets/localisation | Root `docs/assets/` contains `gzh-qr.png` and `xhs-qr.jpg`; marketplace descriptions and source-language README are Chinese. | Whether these belong in an English distribution is a packaging/localisation decision. |
| Language leakage | Task 014 reports no unexpected source-language leakage in the fixed English package tree. | This does not make the wider repository English-only; root/source and historical records intentionally include Chinese. |
| Validation | No CI, test, fixture, validator or harness configuration was found in the complete tree. | Static package completeness has not been converted into executable validation or runtime installation evidence. |
| Continuity | Current-position record remains at Pass 4 / Checkpoint 12 and records an owner-decision hold; segmentation document contains a stale embedded position paragraph. | Keep the authoritative position distinct from the stale paragraph; do not update continuity records in this task. |
| Task-record convention | Only restructuring Task 001 exists under `active/`; the required `completed/` output directory was not present at ingress. | The task explicitly supplies the report path; this report creates that path without moving or rewriting the active task. |

## 8. Suggested task dependencies

The following are dependency observations, not new task assignments or owner decisions.

1. **Owner decision required before product-layout implementation:** confirm the English edition's root-level package placement and distribution branch/ref, including how the source-language root package is preserved outside that product branch.
2. **Owner decision required before discovery metadata changes:** decide whether the source-language marketplace entry remains alongside an English entry, is replaced, or is published through a separate marketplace. This affects current consumer behavior.
3. **Owner decision required for packaging:** decide how the root MIT licence, README, marketplace metadata and any assets are treated in the English distribution.
4. **External evidence required before provisioning work:** identify the external provisioning workflow/configuration and obtain its actual package locator/ref expectations. Do not infer these from README examples.
5. **After those decisions/evidence:** a separately bounded implementation task can establish the product branch/package layout and update only the authorised distribution metadata. A subsequent validation task can run static validators and disposable-environment installation/activation tests against the exact intended branch/ref.
6. **Validation-harness separation:** keep test-only scripts, fixtures and CI material out of the operational English package; the harness branch and its synchronization relationship remain to be designed by the owner/orchestrator.

No Task 002 or subsequent operation is issued by this worker.

## 9. Explicit non-actions and handoff

- No product file, source file, English package file, README, metadata, installation instruction or licence file was changed.
- No existing reconstruction, translation, review, checkpoint, current-position, acceptance or task record was changed.
- No branch was created, renamed or deleted. The target branch will advance only by the commit that adds this required handoff report.
- No workflow, validator, fixture, installer or provisioning configuration was created or run.
- No installation, activation, release, publication or real-device test was performed.
- No final upstream base revision, English package path, distribution branch/ref, marketplace strategy or licence-packaging decision was selected.
- No OE terminology, category, dependency or structural requirement was introduced.

**Incomplete areas:** external provisioning configuration and its package locator are not present in the repository; no runtime installation/activation test was authorised; final base/layout/distribution decisions are explicitly reserved to the owner/orchestrator. These remain incomplete for those reasons, not because the inspected repository paths were silently omitted.

**Exact ingress:** `translation/english-reconstruction` at `219f70ea64ff540f021f4a24e611c6989a38414b`.  
**Report path:** `restructuring/tasks/completed/001-baseline-inventory.md`.  
**Commands/views:** GitHub repository metadata, branch listing, commit metadata, complete recursive tree views, exact file views and SHA comparisons as described in Section 1; no shell commands were run.  
**Worker disposition:** ready for independent orchestrator reconstruction and acceptance. The active task record remains in `restructuring/tasks/active/`; worker completion does not constitute acceptance.
