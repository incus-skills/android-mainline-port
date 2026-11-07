# Task 002 — English edition construction plan and path-ownership map

**Worker status:** COMPLETE — read-only planning assessment and handoff  
**Orchestrator acceptance:** Pending  
**Execution date:** 2026-11-07  
**Repository:** `incus-skills/android-mainline-port`  
**Ingress branch:** `translation/english-reconstruction`  
**Exact task ingress commit:** `90866b5baa5f8946e2b61596f99cc411066fc607`  
**Task record:** `restructuring/tasks/active/002-english-edition-construction-plan.md` (blob `a4d6aa751fa72c4b90c3d39d1308ae1f1e27ad53`)

## 1. Ingress identity, evidence pins and method

The task's exact ingress commit was retrieved and its recursive tree inspected. The task record was issued by the immediately following commit `b6f475325db1504fdf1d15735fd8425777b38b66` (message: `Issue Task 002: plan English edition construction without changes`), whose only changed path is the new Task 002 record. The reconstruction branch pointed to that task-issue commit during execution. Analysis below is pinned to the task-specified ingress commit, not to later branch content. The handoff report is written as a new file on the branch's current head; it does not alter any evidence input.

All Task 002 evidence pins were verified at `90866b5baa5f8946e2b61596f99cc411066fc607`:

| Evidence | Verified identity |
|---|---|
| Task 001 completed report | `restructuring/tasks/completed/001-baseline-inventory.md` — blob `999227c12486f095602d34f8fcd80c12f1778159` |
| Task 001 acceptance | `restructuring/acceptance/001-baseline-inventory-acceptance.md` — blob `756630135622a1b4d594fb9e3cbe21ce4169c591`; disposition ACCEPTED |
| Candidate upstream commit | `yzddmr6/android-mainline-port@5aa6297f40166bec966194d46795c4fd16b533db`; commit tree `58a97a59ded09e79c15e2e1a081907ef96581a34` |
| English package tree | `translation/en/` — tree `ec1ab0b8db21735d4905825872ac32dea2f5be11` |
| English entrypoint | `translation/en/SKILL.md` — blob `b8c4e4e8e625552f9fd4dbd44e613f54ef359aec` |
| English repository guide | `README.en.md` — blob `f099921d88bc8b5f138250fa546ed4287d92aadb` |
| Marketplace manifest | `.claude-plugin/marketplace.json` — blob `abdaaf3d1763db8108b3895b542088f201b3af16` |
| Root licence | `LICENSE` — blob `333b4f81388f8b3e2e40e0b8677bb9e103e809a9` |
| Accepted distribution assessment | `translation/tasks/completed/017-phase5-english-distribution-package-assessment.md` — blob `ff1d2a27e321b3c1b372008cbfc5114ceded3b90` |

No required pin differed. Task 001 acceptance confirms the baseline report was independently reviewed and accepted. The Task 017 assessment was used as an accepted prior analysis, with the current ingress README, manifest and package tree rechecked.

**Repository views used:** GitHub repository/branch/commit metadata; complete recursive Git tree views for the candidate upstream tree, fork `main`, and exact task ingress; exact file/blob views for the pinned records; and subtree view for `translation/en/`. All recursive tree responses used for comparison were complete (`truncated: false`). No local checkout, shell command, package validator, installer, workflow, or runtime test was run.

## 2. Candidate base comparison

### Verified identities

- Candidate upstream commit: `5aa6297f40166bec966194d46795c4fd16b533db`.
- Candidate upstream commit tree: `58a97a59ded09e79c15e2e1a081907ef96581a34`.
- Fork `main` commit at assessment: `f1dc15240feaaef2b46623a98f91d95c7f6f39e1`; its tree is also `58a97a59ded09e79c15e2e1a081907ef96581a34`.
- Exact reconstruction ingress commit: `90866b5baa5f8946e2b61596f99cc411066fc607`; its tree is `2e5747a557f7b06a64c253e6ab8cb660e69491af`.

The candidate commit is retrievable from both the upstream repository and the fork. The fork's `main` commit is a different commit identity but has the exact same tree SHA as the candidate upstream commit. Thus the candidate and fork-main source snapshots are tree-identical; they are not the same commit.

### Source-root comparison

The candidate source tree has 33 tree entries (28 blobs and 5 directory entries). The exact reconstruction-ingress tree contains 186 entries and is complete. All 28 source-baseline blobs are present at the same root-relative paths on the reconstruction branch. Of those 28 blobs, 27 have identical blob SHAs. The only source-root content difference is:

| Path | Candidate base blob | Reconstruction-ingress blob | Finding |
|---|---|---|---|
| `README.en.md` | `664eed85965dc1325b900b5d16033c112a2f8800` | `f099921d88bc8b5f138250fa546ed4287d92aadb` | English repository guide has been updated on the reconstruction branch; it is not an operational package path. |

The root operational source package (`SKILL.md`, `references/`, `experience/`) is otherwise byte-identical to the candidate base. The candidate tree also contains the same source-language marketplace manifest, MIT licence, Chinese README and QR assets as the fork's `main`.

### Assessment, not base selection

The evidence supports `5aa6297f40166bec966194d46795c4fd16b533db` as a clean and strongly grounded starting-point candidate for a dedicated English product branch:

1. It is the pinned upstream source revision recorded by `translation/INVENTORY.md`.
2. Its tree is identical to the fork's `main` tree at the time of this assessment.
3. The reconstruction branch has no source-root operational changes relative to that tree; the sole source-root blob difference is the English repository guide.
4. The 22-file English operational package exists separately under `translation/en/` and can be promoted without taking reconstruction/task records into the product branch.

No source-root difference requires a different base. The README difference is supporting documentation, not evidence of an operational source-package divergence. If the owner wants to retain the newer English guide, it can be deliberately carried into the English branch as a support-file change rather than changing the source baseline. The owner retains the final base decision; this report does not select or create a branch.

## 3. Exact 22-file English package promotion map

The English package tree has 24 tree entries: 22 files and the two directories `experience/` and `references/`. Every file maps to the same relative path at repository root. Blob SHAs below identify the exact English source file verified under `translation/en/`.

| # | Source path | Proposed root destination | English source blob |
|---:|---|---|---|
| 1 | `translation/en/SKILL.md` | `SKILL.md` | `b8c4e4e8e625552f9fd4dbd44e613f54ef359aec` |
| 2 | `translation/en/experience/README.md` | `experience/README.md` | `8670ddde09f42af5591f792c626d4028bb53fb14` |
| 3 | `translation/en/experience/audio.md` | `experience/audio.md` | `7764597a750a7cc4c22873feddcb5005fef2537d` |
| 4 | `translation/en/experience/bootloader-and-boot.md` | `experience/bootloader-and-boot.md` | `f402f95d30f53aedb4f8a38cf5fcbc7d21f32de6` |
| 5 | `translation/en/experience/build-and-release.md` | `experience/build-and-release.md` | `411a9d962428bff35f6a8c66116ce789290028e2` |
| 6 | `translation/en/experience/display.md` | `experience/display.md` | `ed9ec9b19090dd5907e132569545082b94c43d27` |
| 7 | `translation/en/experience/gpu.md` | `experience/gpu.md` | `aefbce022e3e7df841664b561e392e39b3bbdb0c` |
| 8 | `translation/en/experience/kernel-and-resources.md` | `experience/kernel-and-resources.md` | `b980cd153c556e632fa53b876a4230de12b936f1` |
| 9 | `translation/en/experience/patterns.md` | `experience/patterns.md` | `7309618ad08085bb514b6800a580774f829fe312` |
| 10 | `translation/en/experience/power-and-time.md` | `experience/power-and-time.md` | `48c1c865ff07ee79e484af2c37f4ffcc8d57a7bc` |
| 11 | `translation/en/experience/sensors-camera-media.md` | `experience/sensors-camera-media.md` | `dbd1f5cef992ccccf1dbba3bc9ddc22342296343` |
| 12 | `translation/en/experience/testing-and-process.md` | `experience/testing-and-process.md` | `b78afbf1b3a9f05a6765a51b2241eb56bd5c0eca` |
| 13 | `translation/en/experience/touch-and-input.md` | `experience/touch-and-input.md` | `31e2d8ee5a40417e9d1ba4cad5f8c008fdde154c` |
| 14 | `translation/en/experience/ubuntu-userspace.md` | `experience/ubuntu-userspace.md` | `47324f6d9e29c4ea19dc911cd6f67f144e4e5954` |
| 15 | `translation/en/experience/usb-and-storage.md` | `experience/usb-and-storage.md` | `7c3225d7a7f8650ce27a345922937cdadcf0cc59` |
| 16 | `translation/en/experience/wireless.md` | `experience/wireless.md` | `8bf6c27e2eb3a88fd0b55c8bd02dd7b10c1ee087` |
| 17 | `translation/en/references/build-environment.md` | `references/build-environment.md` | `0b1e27b7939d6e6062d9abe567ba565ed8042bb8` |
| 18 | `translation/en/references/decisions.md` | `references/decisions.md` | `3e63f6f4b54ab987c9e438dbf015d4f7f72d85c9` |
| 19 | `translation/en/references/discipline.md` | `references/discipline.md` | `078e020ad497e9ec69388ad55c5beeb4677fd22a` |
| 20 | `translation/en/references/process.md` | `references/process.md` | `3e770386533dda9f0b5162043c08e9ffea454467` |
| 21 | `translation/en/references/ubuntu.md` | `references/ubuntu.md` | `7e70f8bb2457641984c194bc13a48396d5956ded` |
| 22 | `translation/en/references/verification.md` | `references/verification.md` | `1c0ee1a531dee1a7a42653a453231550f0302023` |

### Relative-reference preservation

The proposed mapping preserves every package-relative path: the package entrypoint remains at the root, and `references/` and `experience/` remain sibling directories with their existing names. Task 017's accepted static assessment independently confirms that the English package's internal relative references resolve within the 22-file package and do not require source-language root files or external experience files. The mapping therefore requires no path rewrite within those 22 files. Static completeness is not proof of installation or runtime activation.

## 4. Root-path collision and ownership map

Every one of the 22 proposed destinations already exists in the candidate source tree as part of the source-language operational skill. For every row in Section 3, the candidate-base blob and reconstruction-ingress blob are identical to one another and different from the English package blob. Accordingly, in the dedicated English branch these 22 destinations must be replaced by the corresponding English files. The source-language versions remain preserved on the source/default branch and on the reconstruction branch; they are not deleted from those branches.

| Root path(s) | Existing owner at candidate base | English-branch treatment | Ownership result |
|---|---|---|---|
| `SKILL.md` | Source-language skill entrypoint | Replace with Section 3 row 1 | English skill owns root entrypoint on English branch |
| `experience/README.md` and 14 domain files listed in Section 3 | Source-language experience library | Replace each with corresponding English file | English experience library owns `experience/` on English branch |
| Six `references/*.md` files listed in Section 3 | Source-language reference library | Replace each with corresponding English file | English references own `references/` on English branch |
| `README.md` | Chinese repository guide | Do not carry as English `README.md`; either omit from English branch or retain under a clearly distinct name such as `README.zh.md` | Owner decision: whether a Chinese repository guide is retained on the English distribution branch |
| `README.en.md` | English repository guide, updated on reconstruction branch | Promote/adapt as root `README.md`; remove or make the old `README.en.md` a deliberate pointer only if compatibility requires it | English guide becomes the default repository landing page; its content needs the edits in Section 5 |
| `LICENSE` | MIT licence | Retain unchanged at repository root | Shared repository/distribution support; not an operational skill file |
| `.claude-plugin/marketplace.json` | Chinese-described marketplace entry selecting root `./` | Retain path only if Claude marketplace distribution is intended; English branch needs English metadata describing the English root package | English branch metadata is branch-specific; source/reconstruction branch manifest remains untouched |
| `docs/assets/gzh-qr.png`, `docs/assets/xhs-qr.jpg` | Chinese-community QR images referenced by the English guide | Retain only if the corresponding community section remains in the English guide; otherwise omit as unused source-community assets | Owner decision on whether these Chinese-community links belong in English distribution |

**Directory collisions:** the source base has root `experience/` and `references/` directories. The promoted package uses the same directory names and replaces their operational contents. There is no additional destination directory rename.

The remaining candidate-base root files outside the 22-file operational package are exactly the six blob paths shown in the last four rows above: `README.md`, `README.en.md`, `LICENSE`, `.claude-plugin/marketplace.json`, and the two QR assets. The `.claude-plugin/` and `docs/assets/` directory entries are their parent directories, not additional files. No other root-level source-baseline files were found.

## 5. Supporting material and distribution metadata

### Necessary operational package content

The 22 files in Section 3 are the complete operational English skill package identified by the pinned tree. They must travel together at repository root. The entrypoint retains `name: android-mainline-port`; its destination directory is the repository root package location, whose directory name is the repository name `android-mainline-port` when installed as a skill. No source-language root `SKILL.md`, reference or experience file is needed by the English package's internal references.

### Repository-level distribution/support content

**English guide (`README.en.md`, blob `f099921...`):**

- It is a repository guide, not part of the 22-file operational package.
- On an English edition branch, it should become the default `README.md` so the repository landing page describes the edition being distributed.
- Its current first paragraph links `[中文](README.md)`. If the English file is promoted to `README.md`, that link becomes self-referential. Remove the link if the Chinese guide is not carried, or point it to `README.zh.md` if the owner elects to retain a separately named Chinese guide.
- The current guide says, under “Scope and freshness”, “The skill itself is written in Chinese”. That statement becomes false for the English operational edition and must be revised.
- Its install examples currently identify `yzddmr6/android-mainline-port` without the English branch/ref. Those commands select the repository's ordinary/default root package, not this dedicated English branch. They must be replaced with the exact approved English branch/ref locator after its name and consumer route are fixed. No branch name or command syntax is invented here.
- The “Follow and discuss” section is written in English but describes Chinese-language WeChat/Xiaohongshu channels and embeds the two QR assets. This is not operational skill content; retain it only if the owner wants those channels represented in the English distribution.
- The guide links `LICENSE` and both QR images. If those sections/links remain, the referenced files must remain at the stated root-relative paths.

**Chinese guide (`README.md`, blob `3f268fc2cc6614edc40e90e7852326e6a1c6d385`):**

This is source-language repository-level material, not part of the English operational package. It must not occupy the default `README.md` path on an English edition branch if the English guide is to be the default landing page. Whether to omit it or preserve it as `README.zh.md` is an owner choice. The source/default branch and reconstruction branch remain unchanged either way.

**Licence (`LICENSE`, blob `333b4f81388f8b3e2e40e0b8677bb9e103e809a9`):**

The MIT licence should be retained at repository root in the English edition. It is repository-level legal/distribution material rather than one of the 22 skill content files. If the eventual consumer route exports only a nested skill directory or constructs a ZIP that excludes repository-root files, the licence must still be included in that distributed artifact or otherwise made available with it. Whether to add a frontmatter `license` field is not required to resolve the path collision and remains a packaging/specification decision; Task 017 previously flagged this as an owner decision rather than silently changing metadata.

**QR assets (`docs/assets/gzh-qr.png`, blob `435b9e57df119444a8d5060e3d2a0a4b3e68a80d`; `docs/assets/xhs-qr.jpg`, blob `61cba2b814976dbdfeed24637acc95a21cf50cce`):**

These are repository-level images, not skill package dependencies. The current English guide references them for Chinese community channels. Retain them only with that section; if the section is removed, the assets are not required by the operational package. No image contents were modified or independently interpreted.

### Source-language-only and reconstruction-only material

- The source-language operational package currently at root is replaced only on the dedicated English branch; it remains intact on source/default and reconstruction branches.
- The Chinese root README is source-language repository documentation. It is not a dependency of the English package.
- The Chinese marketplace descriptions are source-language metadata. They should remain untouched on the reconstruction/source branch; an English distribution branch needs English-facing metadata if it publishes a marketplace listing.
- `translation/`, `reconstruction/` and `restructuring/` are reconstruction, translation and coordination records, not operational English skill dependencies. The owner has specified that reconstruction records remain on the existing reconstruction branch. They should not be copied into the English product branch. The validation harness and fixtures likewise belong on their separate workstream/branch, not inside the operational package.

### Marketplace and provisioning boundary

The current `.claude-plugin/marketplace.json` has marketplace name `android-mainline-port`, a plugin entry with the same name, Chinese descriptions, `source: "./"` and `skills: ["./"]`. It selects the current repository-root package. On the source/reconstruction branch, that is the source-language skill and the manifest must remain untouched.

On a dedicated English branch whose root contains the English skill, the minimum analogous metadata for a Claude marketplace route is a marketplace manifest at `.claude-plugin/marketplace.json` with the established marketplace/plugin identity, English descriptions, and root-relative source/component selection (`./`). Task 017's specification assessment found no requirement to add `.claude-plugin/plugin.json` merely because it is absent. This describes the minimum branch-local metadata shape; it does not prove that a branch-specific marketplace locator works or that installation activates the English skill.

If the intended route is the Skills CLI or manual clone rather than Claude marketplace, the repository guide must identify the exact English branch/ref and root package. Task 017 records that CLI documentation describes repository subdirectory sources in principle, but the slash-containing reconstruction branch URL was not executed. The precise consumer route remains to be chosen and tested. A root manifest does not, by itself, change the repository's default branch or make consumers select the English branch.

**External provisioning evidence gap:** no provisioning workflow/configuration, install scripts, CI workflow, or validation harness was found in the complete task-ingress repository tree. Therefore the external workflow referenced by the distribution intent cannot be identified or validated from this repository. Before provisioning work, the orchestrator needs:
1. The actual external workflow repository/application and exact configuration file or workflow entrypoint.
2. The locator format it consumes (repository, branch/tag/commit ref, and package path/subdirectory).
3. Whether it pins a moving branch, immutable commit, release/tag, or archive.
4. The intended consumer/installer and install destination or package-selection behavior.
5. Its update/refresh semantics and how it reports the selected package.
6. A representative run/log or an executable test procedure showing that the expected English root `SKILL.md` and its complete package are what gets installed/loaded.

Until that evidence is supplied and independently inspected, no locator/ref should be guessed and no claim of provisioning compatibility, installation, discovery or activation should be made.

## 6. Ordered implementation and validation sequence

This is a dependency-ordered proposal for a later implementation worker. It is not an authorization to perform any step.

| Order | Step and prerequisites | Expected changed paths | Required evidence / boundary |
|---:|---|---|---|
| 1 | Owner confirms the exact base commit, English branch name, and that the dedicated branch is the English product/distribution target. | New branch ref only; no file changes. | Record base commit and tree SHA; confirm branch starts from the owner-selected source snapshot. Candidate `5aa6297...` is strongly supported but not selected here. |
| 2 | Promote the complete 22-file package from the pinned English package source. Confirm root package identity and path ownership. | Replace root `SKILL.md`, all listed `experience/*.md` and all listed `references/*.md`. | Compare promoted blobs against Section 3; confirm exactly 22 operational files, `name: android-mainline-port`, and unchanged relative-reference paths. Source branch files remain untouched. |
| 3 | Resolve repository support material and the consumer route. Owner decisions: Chinese README retention; QR/community section retention; marketplace route/coexistence; exact branch locator. | Likely `README.md` (promoted/adapted from `README.en.md`), optional `README.zh.md`, `LICENSE` retained, optional `docs/assets/*`, and `.claude-plugin/marketplace.json` if marketplace distribution is selected. | Check every README link and asset reference; remove self-link; correct the stale language statement; ensure install commands target the exact English branch/ref; preserve the MIT licence in any distributed archive. Keep source/reconstruction branch metadata untouched. |
| 4 | Static validation after package and support-file changes. | No additional product paths if checks are read-only; any validation code belongs on the separate harness branch. | Validate Agent Skills directory/name/frontmatter constraints; exact 22-file inventory; all relative links/references; no missing external package dependencies; marketplace JSON/schema and selected root source; README links/assets; licence presence. Record tool/version, command, exit status and evidence. |
| 5 | Obtain external provisioning evidence from Section 5 and select one exact supported consumer route. Static checks must pass first. | No product changes expected unless a defect is found and separately authorized; provisioning test configuration/logs belong outside the operational package. | Run in a disposable environment using the exact branch/ref and locator. Confirm the installed package's root `SKILL.md`, declared identity, complete references/experience tree, selected consumer and activation/discovery result. Preserve logs and exact tested commit. |
| 6 | Implement the separate validation-harness workstream after product branch/ref is stable. | Harness-only scripts, fixtures, CI/workflow configuration and test documentation on the separate validation-harness branch. | Harness must test a specified English product commit/ref without adding test-only files to the operational English package. Record how it tracks product revisions and how its results are handed back to product acceptance. |
| 7 | Independent orchestrator review and acceptance. | Acceptance record only, if accepted. | Reconstruct from task, implementation commits, static validation and provisioning evidence. Worker completion is not acceptance. |

### Separation constraints

- English operational package and consumer-facing metadata belong on the English edition branch.
- Translation/reconstruction history and coordination records remain on `translation/english-reconstruction`.
- Validation scripts, fixtures and CI belong on the separate validation-harness branch.
- Do not put harness files under the English skill's `references/` or `experience/` directories.
- Do not treat static path closure as provisioning success or runtime activation.

## 7. Risk register and remaining owner decisions

| Risk / decision | Evidence | Required resolution |
|---|---|---|
| Final base commit | Candidate upstream commit and fork `main` have identical tree SHA; no operational source-root divergence found. | Owner selects the exact commit; report makes no selection. |
| English branch name and stable ref | Dedicated English branch is agreed, but no exact branch name/ref appears in Task 002 evidence. | Owner confirms the name and consumer-facing stable ref. |
| Source-language package path ownership | All 22 promoted root destinations collide with existing source-language package files. | Branch separation resolves the collision: English owns those paths on English branch; source-language files remain on source/reconstruction branch. |
| Chinese README | `README.md` is Chinese and conflicts with the English default landing page. | Omit or rename to `README.zh.md`; owner choice. |
| English README | `README.en.md` has self-link to `README.md`, a stale statement that the skill is written in Chinese, and install commands targeting the upstream default branch. | Promote/adapt as `README.md`; repair links, language description and locators after branch/route decision. |
| Chinese community QR assets | English README references WeChat/Xiaohongshu QR images and says the channels are Chinese. | Retain section and assets or remove both from English branch; owner choice. |
| Marketplace route | Existing manifest selects root `./` but has Chinese descriptions; current source branch manifest must remain unchanged. | Decide whether English branch publishes a marketplace entry and whether it coexists with or is separate from source-language distribution. |
| Licence packaging | Root MIT licence is outside the 22-file skill package. | Retain at repository root; ensure archives/exported packages include it. Decide separately whether frontmatter license metadata is desired. |
| External provisioning | No workflow/configuration or package locator is present in the repository. | Obtain actual workflow and locator/ref semantics before provisioning validation. |
| CLI/Gemini/archive behavior | Prior accepted Task 017 analysis found static documentation gaps; no runtime tests were performed. | Test only the selected consumer routes in a disposable environment; do not infer behavior from documentation. |
| Validation-harness boundary | No harness branch/tooling existed in the inspected tree. | Keep future test code/fixtures/CI separate and define how it validates a pinned English product commit. |

## 8. Explicit non-actions and handoff

- No branch was created, renamed, updated or deleted.
- No product file was copied, moved, renamed, deleted or rewritten.
- No README, licence, marketplace manifest, skill frontmatter, package metadata, or asset was changed.
- No validation scripts, fixtures, CI workflows, provisioning configuration or harness branch were created.
- No final base commit, branch name, marketplace coexistence strategy, README language policy, QR-asset treatment or frontmatter licence choice was selected.
- No external provisioning workflow was guessed or claimed to have been tested.
- No package validator, installer, consumer CLI, Claude Code, Gemini CLI, release archive, or runtime activation was run.
- No translation/reconstruction/current-position record was modified and translation work was not resumed.
- No Task 003 was issued and no implementation operation was performed.

**Incomplete areas and reasons:** the exact English branch name/ref, Chinese README and community-asset treatment, marketplace publication strategy, and external provisioning workflow are unresolved by the evidence and remain owner/orchestrator decisions. Runtime validation is intentionally deferred to a separately authorized task after the exact consumer locator is established.

**Exact task ingress:** `translation/english-reconstruction` at `90866b5baa5f8946e2b61596f99cc411066fc607`.  
**Task issue commit / branch head observed during execution:** `b6f475325db1504fdf1d15735fd8425777b38b66` (only adds the Task 002 record).  
**Required report path:** `restructuring/tasks/completed/002-english-edition-construction-plan.md`.  
**Repository views:** GitHub commit, branch, recursive tree, subtree and exact blob/file views; SHA comparisons as detailed above. No shell commands were run.  
**Worker disposition:** ready for independent orchestrator reconstruction and acceptance. The active Task 002 record remains in `restructuring/tasks/active/`; worker completion does not constitute acceptance.
