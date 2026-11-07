# Task 017 — Phase 5 English edition package and installation-route assessment

## Purpose and observer position

Determine which repository package layout and installation/discovery routes could expose the independent English edition as a conformant, testable skill without confusing it with the source-language root package.

The project owner has established that the English edition is an independent continuity and distribution target and should be testable through existing provisioning workflows. Checkpoint 10 records that direction, while explicitly leaving package layout, branch/path selection, metadata, release mechanism and automation undecided.

Task 015 established that the current documented public installation routes select the source-language root package, not the English reconstruction, and that the English entrypoint's parent directory name does not match its declared Agent Skills `name`. Task 016 has now documented the English project's local experience lifecycle and upstream contribution path. This task addresses the next dependency: a decision-supporting assessment of viable English package and installation routes.

This is a **read-only assessment**. It must not implement a guessed distribution design or turn a recommendation into an owner decision.

## Position and governing context

- Traversal: **Pass 4 — whole-skill English validation**.
- Project phase: **Phase 5 — Whole-skill validation**.
- Current position: **Pass 4 / Checkpoint 11**.
- Accepted operations: Tasks 008–016, within their recorded boundaries.
- Repository: `incus-skills/android-mainline-port`.
- Branch: `translation/english-reconstruction`.
- Branch baseline at task preparation: commit `4887d48d57f0fdfb6bbe9b03ae4209136ff3b311`; tree `9269053bfc061cbe6827a5ab94b5677321ad0945` (recursive listing complete; `truncated: false`).
- Checkpoint 01: `reconstruction/zh/continuation/checkpoints/pass-4-checkpoint-01.md`, blob `598722e61e115068af5c90d90dbdf8224dd7c764`.
- Checkpoint 10: `reconstruction/zh/continuation/checkpoints/pass-4-checkpoint-10.md`, blob `c0ff7fa6eb50c3647d3159deb199c7cf1b6ad41d`.
- Checkpoint 11: `reconstruction/zh/continuation/checkpoints/pass-4-checkpoint-11.md`, blob `7dd2a73bb365a047ffa067f99fd3e01f9618f12b`.
- Task 015 completed record: `translation/tasks/completed/015-phase5-installation-skill-discovery-compatibility.md`, blob `44e21f48a98e9a79c8c9a0ac6506fde756435e0c`.
- Task 016 completed record: `translation/tasks/completed/016-phase5-recursive-experience-development-lifecycle.md`, blob `48ef17afd35227846c0a107992102e25cf5befef`.
- Phase 5 criteria: `translation/README.md`, blob `f9bf34ae28e838ae881f9e207797f26c1b327baa`.
- Inventory: `translation/INVENTORY.md`, blob `0a8e58e9a6f45fd54c718672dc8d7a3a9056f7ee`.
- Execution segmentation and eight-field handoff: `translation/EXECUTION-SEGMENTATION.md`, blob `05ee569421b89366bbc0993afe62cb22a0644913`.

## Evidence boundary and pinned repository inputs

Retrieve the current branch head and complete recursive tree before analysis. Verify every pinned file against its blob SHA. If any pin differs, stop and report the mismatch; do not silently substitute another revision. Additional repository files may be inspected only where a direct package or route dependency is demonstrated, with path and revision recorded.

Core package and distribution evidence:

- Source-language root skill entrypoint: `SKILL.md`, blob `9d9ea8aca4afde6ced79154958864196cc3da433`.
- Claude marketplace manifest: `.claude-plugin/marketplace.json`, blob `abdaaf3d1763db8108b3895b542088f201b3af16`.
- Marketplace directory tree: `.claude-plugin/`, tree `9134c5088697329f3d92aca741079973b4cf0852`.
- English-facing installation guide: `README.en.md`, blob `f099921d88bc8b5f138250fa546ed4287d92aadb`.
- Root README: `README.md`, blob `3f268fc2cc6614edc40e90e7852326e6a1c6d385`.
- English operational entrypoint: `translation/en/SKILL.md`, blob `b8c4e4e8e625552f9fd4dbd44e613f54ef359aec`.
- English experience-library instructions: `translation/en/experience/README.md`, blob `8670ddde09f42af5591f792c626d4028bb53fb14`.
- English package tree: `translation/en/`, tree `ec1ab0b8db21735d4905825872ac32dea2f5be11`.
- English references subtree: `translation/en/references/`, tree `41f3799c0dd0caa7bdef8e9b4820f45f21ace473`.
- English experience subtree: `translation/en/experience/`, tree `858f81387a8554a1c60f802bcf3bf49e4ee30114`.
- Licence: `LICENSE`, blob `333b4f81388f8b3e2e40e0b8677bb9e103e809a9`.

Use Task 015 as the prior static route analysis and independently verify the relevant current repository facts. Use Task 016 and the current English files to preserve the established independent-continuity and upstream-contribution intent. Do not reopen Pass 3 source-to-target review.

## Primary specifications

Retrieve current primary documentation and record the page title, URL, retrieval date and exact requirements used:

1. Agent Skills specification: `https://agentskills.io/specification`
   - Skill directory structure, required frontmatter, `name` constraints and parent-directory matching, `description`, relative file references and validation.
2. Claude Code plugin documentation:
   - `https://code.claude.com/docs/en/plugins`
   - `https://code.claude.com/docs/en/plugins/create-marketplace`
   - `https://code.claude.com/docs/en/plugins/marketplace-reference`
   - Standalone skills versus plugins, marketplace structure, plugin source resolution, component selection and manifest behavior.
3. Skills CLI documentation:
   - `https://www.skills.sh/docs/cli`
   - `https://github.com/vercel-labs/skills/blob/main/docs/commands.md`
   - Supported repository, branch/path and subdirectory source forms; skill selection and installation scope.
4. Existing provisioning or distribution documentation in this repository, but only where a direct relationship to the intended English installation route can be demonstrated.

Do not use third-party tutorials, forum posts, memory or inferred installer behavior as compatibility authority. Do not claim runtime compatibility from static documentation.

## Scope of assessment

1. **Reconstruct the actual package topology.**
   - Distinguish repository root, source-language root skill, English operational tree, Claude marketplace and any other package or provisioning entrypoints.
   - Identify which files and relative references must travel together for the English skill to remain complete.
   - Confirm whether the English tree can be packaged as a self-contained skill directory without importing files outside its intended package root.

2. **Reconstruct the established distribution intent.**
   - The English edition has its own continuity and experience stream.
   - The English project retains its experience regardless of upstream PR outcome.
   - Selected material may be translated/adapted and proposed upstream.
   - The intended English edition should be testable through existing provisioning workflows.
   - Do not reinterpret this as a decision to replace, rename or remove the source-language root package.

3. **Compare viable package-layout options.**
   - Assess only options grounded in the actual repository structure and supported distribution mechanisms.
   - For each option, identify the proposed package root and directory name, required `SKILL.md` name, relative-reference behavior, source-language package interaction, marketplace component path, CLI source form and consequences for existing README instructions.
   - Include the least disruptive option and any cleaner long-term option if the evidence supports both.
   - Do not assume a particular name, path, branch strategy or packaging mechanism before comparing the evidence.

4. **Compare documented installation/discovery routes.**
   - Claude Code marketplace/plugin route.
   - Skills CLI route, including explicit branch/subdirectory selection where supported.
   - Manual clone or direct provisioning route already documented in the repository.
   - Any additional route only if a repository artifact or primary specification establishes it as relevant.
   - For each, trace source repository and branch → selected package directory → `SKILL.md` → referenced files → installed/discovered skill identity.
   - Distinguish static route validity from actual installation, activation and provisioning success.

5. **Assess provisioning testability.**
   - Identify the existing provisioning workflow(s) the project intends to use, if discoverable from the pinned repository evidence.
   - Determine what package form and source locator those workflows require.
   - If the workflow is not represented in the available repository evidence, report the missing dependency; do not invent its behavior.
   - Identify the smallest later runtime test that could establish installation and activation, without executing it in this task.

6. **Produce a decision-supporting comparison.**
   - Summarise the viable options, advantages, disadvantages, compatibility constraints and likely change surface.
   - Recommend a preferred option only where the repository and primary specifications support a reasoned recommendation.
   - Separate that recommendation from decisions that require project-owner confirmation.
   - Identify which changes would be required in package layout, frontmatter, marketplace manifest, README installation instructions, provisioning configuration and release artifacts.
   - Identify any route-specific unknowns that need a later bounded runtime test.

## Exclusions

- Do not modify, rename, move, copy or delete any skill files or package directories.
- Do not change frontmatter, marketplace manifests, plugin metadata, installation instructions, README files, provisioning configuration, release files or branch layout.
- Do not publish, install, activate or execute the English skill.
- Do not install packages, run untrusted repository scripts or alter agent settings.
- Do not submit an upstream pull request.
- Do not reopen Pass 3 translation fidelity or assess Android technical correctness, device safety or real-device behavior.
- Do not choose a new product identity or silently resolve a project-owner decision.
- Do not treat static compatibility as runtime success or Pass 4 completion.

## Permitted changes

- Read-only with respect to all repository content and project configuration.
- Append the required eight-field handoff to this task record only.
- After execution and handoff, move the task record unchanged except for the appended handoff to `translation/tasks/completed/`.
- Do not update checkpoints, current-position records or the inventory. Those are orchestrator integration responsibilities after independent acceptance.

## Stop conditions

Stop and report if:
- any pinned file or tree differs from its stated SHA;
- the English package's complete dependency tree cannot be reconstructed;
- the applicable primary specifications do not establish a required compatibility rule;
- provisioning behavior depends on unavailable configuration or tooling;
- a useful comparison would require implementing, installing or publishing a candidate package;
- a required product/distribution choice cannot be inferred from the established owner decision.

## Validation

- Confirm branch identity, branch head, complete recursive tree and every pinned SHA.
- Confirm the package tree and relative references are complete for each option considered.
- Verify every claimed frontmatter or manifest constraint against the current primary specification.
- Trace each route to the exact package directory and entrypoint; distinguish selected source-language and English packages.
- Check whether any proposed package root would require files outside that root.
- Record exact source paths, revisions, specification sections and unresolved assumptions.
- Review the comparison for accidental implementation decisions or claims of runtime success.

## Required worker handoff

Append a reconstructable handoff using the eight fields in `translation/EXECUTION-SEGMENTATION.md`:

1. **Task identity:** Pass 4, Task 017, branch, task/document revisions.
2. **Observer position:** package/distribution question and explicit exclusions.
3. **Observations:** repository topology, route behavior, applicable specification constraints and evidence.
4. **Decisions:** options compared and any supported recommendation, with reasons; distinguish recommendation from owner decision.
5. **Unresolved matters:** missing provisioning evidence, route-specific unknowns and owner decisions.
6. **Validation:** tree and pin checks, reference checks, specification retrieval and route reconstruction.
7. **Change boundary:** confirm no repository content or configuration was changed.
8. **Next dependency:** the smallest owner decision or separately bounded implementation/runtime test required.

State whether the task was completed or stopped for an adequacy reason. The worker must not claim orchestrator acceptance.


---

# Worker handoff — Task 017

**Execution status: COMPLETED — read-only package and distribution assessment. Worker execution is finished; awaiting independent orchestrator acceptance.** No package was changed, installed, activated, published or runtime-tested.

## 1. Task identity

- Repository: incus-skills/android-mainline-port.
- Branch: translation/english-reconstruction.
- Current branch head inspected: 1a068d628a26fe7dbcb86a8005b4bde0beff2690.
- Current head tree: d481c8ff58e796e6a0f80dfb11f3090b3addc32b; recursive listing complete, truncated=false, 177 entries.
- Task record before handoff: translation/tasks/017-phase5-english-distribution-package-assessment.md, blob bdfe1a9f55773f571415d5d283bbc308a4a0fb86.
- Task preparation baseline: commit 4887d48d57f0fdfb6bbe9b03ae4209136ff3b311; tree 9269053bfc061cbe6827a5ab94b5677321ad0945. The branch has advanced since preparation. The current branch head and complete tree were retrieved; all pinned files and named package subtrees listed below retain their specified revisions. The changed whole-repository tree is not treated as a pin mismatch because the task pins individual files and specific subtrees, and those were verified.
- Traversal: Pass 4 — whole-skill English validation; Phase 5; Checkpoint 11.
- Governing checkpoint pins verified:
  - Checkpoint 01: reconstruction/zh/continuation/checkpoints/pass-4-checkpoint-01.md — 598722e61e115068af5c90d90dbdf8224dd7c764.
  - Checkpoint 10: reconstruction/zh/continuation/checkpoints/pass-4-checkpoint-10.md — c0ff7fa6eb50c3647d3159deb199c7cf1b6ad41d.
  - Checkpoint 11: reconstruction/zh/continuation/checkpoints/pass-4-checkpoint-11.md — 7dd2a73bb365a047ffa067f99fd3e01f9618f12b.
- Prior task pins verified:
  - Task 015 completed record — 44e21f48a98e9a79c8c9a0ac6506fde756435e0c.
  - Task 016 completed record — 48ef17afd35227846c0a107992102e25cf5befef.
  - Phase 5 criteria, translation/README.md — f9bf34ae28e838ae881f9e207797f26c1b327baa.
  - Inventory, translation/INVENTORY.md — 0a8e58e9a6f45fd54c718672dc8d7a3a9056f7ee.
  - Execution contract, translation/EXECUTION-SEGMENTATION.md — 05ee569421b89366bbc0993afe62cb22a0644913.
- Core distribution pins verified:
  - Root source-language SKILL.md — 9d9ea8aca4afde6ced79154958864196cc3da433.
  - .claude-plugin/marketplace.json — abdaaf3d1763db8108b3895b542088f201b3af16.
  - .claude-plugin tree — 9134c5088697329f3d92aca741079973b4cf0852.
  - README.en.md — f099921d88bc8b5f138250fa546ed4287d92aadb.
  - README.md — 3f268fc2cc6614edc40e90e7852326e6a1c6d385.
  - English entrypoint translation/en/SKILL.md — b8c4e4e8e625552f9fd4dbd44e613f54ef359aec.
  - English experience instructions translation/en/experience/README.md — 8670ddde09f42af5591f792c626d4028bb53fb14.
  - English package tree translation/en/ — ec1ab0b8db21735d4905825872ac32dea2f5be11.
  - English references subtree — 41f3799c0dd0caa7bdef8e9b4820f45f21ace473.
  - English experience subtree — 858f81387a8554a1c60f802bcf3bf49e4ee30114.
  - Root LICENSE — 333b4f81388f8b3e2e40e0b8677bb9e103e809a9.
- No pinned revision mismatch was found. The current branch head, recursive tree and every named file/tree pin above were verified before analysis.

## 2. Observer position

Determine which package layout and documented installation/discovery routes could expose the English edition as a conformant, testable skill while preserving the source-language root package and the English edition's independent continuity.

This is static package, metadata and route analysis. It does not select a product identity, branch strategy, release mechanism or final package path. It does not establish runtime installation, activation, provisioning success, Android technical correctness or Pass 4 completion.

## 3. Observations

### Repository topology and package completeness

- The repository root is the source-language package: root SKILL.md, references/ and experience/. Its skill name is android-mainline-port. The README manual-clone destination is also explicitly named android-mainline-port, so the root package's directory/name relationship is conformant when cloned to that destination.
- The English operational tree is translation/en/. Its immediate directory name is en, while translation/en/SKILL.md declares name: android-mainline-port. This is the Agent Skills parent-directory/name mismatch already identified in Task 015; it remains present in the current pinned revision.
- The English package tree is complete in the current recursive branch tree: 22 blobs comprising one SKILL.md, six reference documents, and fifteen experience documents (including the experience README). The references and experience subtrees match the task's pinned tree SHAs.
- The English SKILL.md uses relative references rooted at its own package directory. The referenced references/ and experience/ paths are present in translation/en/. The experience README also points to ../references/ubuntu.md, which resolves within the English package. The Task 016 additions to the skill and experience README add lifecycle instructions but do not introduce a dependency on files outside the package.
- The English package can travel as a complete set of its 22 files without importing the source-language root references/ or experience/ directories. This establishes relative-file completeness, not Agent Skills directory-name conformance.
- The root LICENSE is outside translation/en/. The Agent Skills specification does not require a license field, but if the English package is distributed as an independently consumable artifact, whether to bundle the license or declare it in frontmatter should be decided as part of release packaging. This is a distribution/legal packaging question, not a missing runtime reference.
- The current complete repository tree contains no .github, .gitlab, .forgejo, scripts, install, provision or similarly named provisioning/CI configuration paths. No machine-readable provisioning workflow was found in the repository evidence.

### Established English distribution intent

- Checkpoint 10 and Task 016 establish the English edition as an independent continuity and distribution target, with its own experience stream and a contribution path upstream.
- The English project retains locally accumulated experience regardless of whether an upstream proposal is accepted. Local capture, consolidation, validation, selection, adaptation, proposal and upstream incorporation remain separate stages.
- The stated goal is to make the English edition testable through existing provisioning workflows. The repository does not identify or contain the external provisioning configuration that would establish the exact source locator, package form or automation behavior. This missing dependency is recorded rather than inferred.
- This intent does not authorise replacing, renaming or removing the source-language root package.

### Agent Skills specification

Primary page retrieved 2026-11-07: Specification — Agent Skills, https://agentskills.io/specification.
Sections used: Directory structure; SKILL.md format; Frontmatter; name field; description field; File references; Validation.

- A skill is a directory containing at least SKILL.md.
- name is required, 1–64 characters, lowercase alphanumeric characters and hyphens, no leading/trailing hyphen or consecutive hyphens, and must match the parent directory name.
- description is required, non-empty, 1–1024 characters, and should describe what the skill does and when to use it.
- File references are relative to the skill root.
- Both current names use permitted characters and are 21 characters long. The English description is non-empty and within the length limit. The defect is specifically the parent directory name en, not the English metadata value itself.
- The specification identifies skills-ref validate as the reference validator. No validator was installed or run.

### Claude Code plugin and marketplace specifications

Primary pages retrieved 2026-11-07:
- Plugins overview — Claude Code Docs, https://code.claude.com/docs/en/plugins.
- Create a marketplace — Claude Code Docs, https://code.claude.com/docs/en/plugins/create-marketplace.
- Marketplace reference — Claude Code Docs, https://code.claude.com/docs/en/plugins/marketplace-reference.

Sections used: plugin overview and component model; marketplace file location and root; required top-level and plugin-entry fields; relative path plugin source; how an entry combines with plugin.json; strict mode; plugin source ref behavior.

- The current marketplace file is at the documented .claude-plugin/marketplace.json location. Its JSON parses. It contains required marketplace fields name, owner and plugins. Its sole plugin entry contains required name and source.
- Current marketplace name and plugin entry name are both android-mainline-port. The entry has source: "./" and skills: ["./"].
- Relative plugin source paths resolve from the marketplace root (the repository root), not from .claude-plugin/. Therefore the current source selects the repository root.
- .claude-plugin/plugin.json is absent from the complete current tree. The current marketplace reference explicitly says that when plugin.json is absent, the marketplace entry acts as the plugin manifest, including its component fields. Absence is not itself a schema defect.
- The skills component currently points to "./", so the marketplace entry exposes the root skill, not translation/en/SKILL.md.
- The marketplace and plugin descriptions are Chinese. The consulted schema does not require English-language descriptions; their language is a localisation/discoverability consideration, not a schema failure.
- The marketplace source reference supports a branch/tag ref for GitHub or Git sources; when omitted, the repository default branch is used. A marketplace added from an unqualified repository URL therefore does not, by itself, select translation/english-reconstruction.
- A separate marketplace plugin entry could in principle point to a same-repository English package directory using a relative source path. Whether to add a separate English plugin entry, replace the current root plugin entry, or publish a distinct marketplace is an owner decision. A separate entry would also need a distinct plugin entry name if both editions are to coexist.

### Skills CLI documentation and README route reconstruction

Primary pages retrieved 2026-11-07:
- CLI — Skills Documentation, https://www.skills.sh/docs/cli.
- CLI reference — vercel-labs/skills, https://github.com/vercel-labs/skills/blob/main/docs/commands.md.

Sections used: skills add; Source Formats; Options; Examples; Installation Scope.

- The CLI documentation supports repository sources, full GitHub URLs, direct GitHub paths to a skill subdirectory, local directory paths, tag/commit selection, and --skill selection by name.
- README.en.md currently documents npx skills add yzddmr6/android-mainline-port -g without a branch or subdirectory. The command therefore targets that repository's default branch and repository root, not the English reconstruction branch.
- The README's manual clone command also clones the unqualified repository URL into a directory named android-mainline-port. It therefore receives the default-branch root package, not the English subtree.
- The current public repository evidence was independently checked: yzddmr6/android-mainline-port is a distinct repository (ID 1427247107) whose default branch is main. Its main tree is 5aa6297f40166bec966194d46795c4fd16b533db and contains no translation/en directory. The incus-skills repository's main tree is f1dc15240feaaef2b46623a98f91d95c7f6f39e1 and likewise contains no translation/en directory. The English tree exists on the explicitly named translation/english-reconstruction branch.
- A direct subdirectory URL is a documented CLI source form. Once the English package is in a directory whose basename matches its name, a URL to that branch and subdirectory is the clearest static route. The exact URL shape with this slash-containing branch name was not exercised; that branch/path lookup remains a runtime validation point.
- The CLI also supports --skill selection by name. Both current root and English SKILL.md files declare the same name, and the consulted CLI documentation does not define name-only selection behavior for duplicate names in one repository. Direct subdirectory selection avoids relying on that ambiguity.
- The README also documents a Gemini CLI route and a claude.ai/desktop release-download route. Their installer-specific selection semantics and exact release-archive contents are not established by the primary documentation authorised for this assessment. They remain unverified and must not be treated as English installation routes.
- The Skills CLI documentation describes user/project install scope. It does not establish the behavior of an external provisioning workflow not represented in this repository.

### Route comparison

| Route | Current source and selection | English-edition assessment |
|---|---|---|
| Claude Code marketplace | Marketplace is rooted at the repository root; current entry source ./ and skills ./ select root SKILL.md. README command does not specify the reconstruction branch. | Current route selects the source-language package. An English route needs an explicit branch/ref and an entry whose source/component path selects the English package. |
| Skills CLI | README command selects yzddmr6/android-mainline-port main at repository root. | Current route selects source-language root. CLI documents direct subdirectory URLs and local paths, so a branch/subdirectory route is statically available in principle after directory-name conformance. Slash-containing branch path requires a test. |
| Manual clone | README clones default branch to a directory named android-mainline-port. | Current route selects source-language root. A branch-specific clone plus selection of the English package subdirectory/local path is possible, but is not the documented current instruction. |
| Gemini CLI | README points to the unqualified yzddmr6 repository URL. | The URL does not identify the reconstruction branch. Gemini-specific package selection and activation are outside the primary documentation retrieved; unresolved. |
| claude.ai/desktop archive | README refers to a release download. | Exact archive contents and English entrypoint selection were not inspected; unresolved. |

### Package-layout options

1. **Keep translation/en as the skill directory and change its frontmatter name to en.**
   - Lowest structural change and no package-file move.
   - Satisfies the immediate directory/name rule only by changing the skill's discovery identity from android-mainline-port to en.
   - The identity and resulting CLI/agent discovery semantics would need explicit owner approval. Not preferred absent a decision that en is the intended skill name.

2. **Nest the existing English package under translation/en/android-mainline-port.**
   - Move the complete 22-file package together, preserving its internal references and frontmatter name.
   - The immediate package directory would then match name: android-mainline-port.
   - Keeps the English edition within its existing translation/en organisational area and leaves the source-language root package untouched.
   - Requires updating repository records and documentation that currently refer to translation/en/SKILL.md, translation/en/references/ or translation/en/experience/ as package-root paths. The branch/path CLI route and marketplace relative source can then select the named package directory.
   - This is the least disruptive conformant option supported by the current topology, if the owner wants to retain the existing skill name.

3. **Create a dedicated distribution package root, such as distribution/android-mainline-port, or publish the English package at the root of a dedicated distribution branch/repository.**
   - Gives consumers an unambiguous named package root and clean distribution boundary.
   - Can make manual clone and provisioning locators simpler.
   - Requires a larger change to the current translation tree or an explicitly governed release/export process. A duplicate copy would create a divergence risk unless a synchronisation mechanism is deliberately designed.
   - Cleaner as a long-term distribution boundary only if the owner wants a separate published artifact/branch/repository.

4. **Replace or duplicate the current marketplace plugin entry.**
   - Keeping the existing root entry preserves source-language distribution.
   - A separate entry can expose the English package independently, with its own plugin entry name and source path. Replacing the current entry would change what existing users receive.
   - The choice between coexistence, replacement or a separate marketplace is a project-owner decision, not inferable from the current manifest.

### Decision-supporting assessment

- The strongest evidence-supported recommendation is to preserve the English skill's current metadata identity and make the distributable package directory itself named android-mainline-port. Of the repository-local options, nesting the complete package under translation/en/android-mainline-port is the least disruptive conformant route. This is a recommendation for owner consideration, not an implementation decision.
- Changing the frontmatter name to en would satisfy the directory rule at the cost of changing the established skill identity. No evidence establishes that the owner wants that identity.
- A dedicated distribution branch or repository may be cleaner for consumers but introduces branch/release governance not present in the current repository evidence.
- The current marketplace and README instructions should not be described as English distribution routes. They presently select the source-language root package.
- The English package's relative content is self-contained, but its current directory name is not conformant. Static path completeness must not be conflated with installability or runtime activation.

## 4. Decisions

- **Assessment result: PARTIALLY COMPATIBLE.**
  - English package contents and internal relative references are complete.
  - English Agent Skills frontmatter values satisfy field presence, character and length constraints.
  - The current English package directory does not satisfy the required parent-directory/name match.
  - Existing documented public install routes select the source-language root package, not the English reconstruction.
  - Claude marketplace JSON is structurally compatible for its current source-language root plugin; the missing plugin.json is not a defect.
  - The CLI documents a direct subdirectory source mechanism that could support a conformant English package after layout and branch selection are resolved.
- Provisional recommendation: retain the established skill name and expose the English tree from a directory named android-mainline-port. The repository-local nested option is the least disruptive candidate. No changes were made.
- No conclusion is made about actual provisioning, installation, discovery, activation or runtime success.

## 5. Unresolved matters

- Owner decision: retain android-mainline-port as the English skill name, or adopt a distinct English-specific identity.
- Owner decision: nested package under translation/en, dedicated distribution directory, or separate distribution branch/repository.
- Owner decision: keep source-language and English packages as separate marketplace entries, replace the current entry, or use a separate marketplace.
- Owner decision: which branch/ref is the intended stable English distribution target and how it is exposed to consumers.
- External provisioning configuration/tooling is not present in the repository tree. Its expected package locator and install behavior must be supplied or inspected in a separately bounded operation.
- The CLI's direct GitHub subdirectory URL using the slash-containing branch name has not been executed.
- Gemini CLI selection/activation and the contents of the README's release archive remain unverified.
- No validator or installer was installed or run. No runtime test was performed.
- Root LICENSE is outside the English package; the release owner should determine whether to bundle it or add an appropriate frontmatter license reference.
- No separate investigation proposal was created; the items above are explicit evidence gaps and owner decisions, not findings or implementation instructions.

## 6. Validation

- Retrieved current branch ref and commit; confirmed branch identity and current head tree.
- Retrieved the complete recursive current branch tree: 177 entries, truncated=false.
- Verified every task-pinned file SHA and each named package subtree SHA listed in Task Identity. No pin mismatch was found.
- Verified Task 017's own pre-handoff blob SHA before writing.
- Confirmed current English package tree has 22 blobs and the pinned references and experience subtrees.
- Checked the English entrypoint, experience README and their internal path relationships against the complete package tree. The existing English package files travel together; no root source-language references or experience files are needed to satisfy the English entrypoint's relative references.
- Parsed the current marketplace JSON and inspected the complete .claude-plugin tree. Confirmed plugin.json is absent.
- Retrieved the six authorised primary documentation pages on 2026-11-07 and recorded their titles, URLs and relevant sections above.
- Independently checked the README target repository's identity, default branch and current main tree, and checked the incus-skills main tree. Neither main tree contains translation/en.
- Reviewed current README installation commands and the previously accepted Task 015 route analysis. Task 015's prior static findings were rechecked against the current README and repository facts; where old repository tree SHAs had changed, only current retrieved facts were used.
- Searched the complete repository tree for provisioning/CI configuration. None was present in the inspected tree.
- Validation is static repository/specification/path analysis only. No CLI, Claude Code, Gemini or release-archive runtime test was performed.

## 7. Change boundary

- Repository content and configuration remained read-only during assessment.
- This appended handoff is the only content change authorised by Task 017.
- No skill files, package directories, frontmatter, marketplace manifest, README, provisioning configuration, release artifacts, branch layout, checkpoints, inventory or current-position records were modified.
- No installation, activation, publication, upstream contribution, package execution or runtime test was performed.
- Task execution completion does not constitute orchestrator acceptance.

## 8. Next dependency

- The orchestrator should independently reconstruct and evaluate this handoff before accepting Task 017.
- Before implementation, obtain owner confirmation of the English skill identity, package root and distribution branch/ref, and whether the source-language marketplace entry remains alongside an English entry.
- Once those choices are confirmed, the smallest useful bounded implementation/test sequence is: create the conformant named package root without changing its internal relative layout; update only the necessary package-path references and selected distribution metadata; run the applicable static validators; then test one CLI direct-subdirectory installation and one Claude Code marketplace installation in a disposable environment, confirming the exact English SKILL.md and expected component inventory are loaded.
- The existing external provisioning workflow must be identified before claiming that the English edition is testable through it.
- Worker execution is complete. **Orchestrator acceptance remains pending.**
