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
