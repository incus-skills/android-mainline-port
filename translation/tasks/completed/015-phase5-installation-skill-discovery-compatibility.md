# Task 015 — Phase 5 installation and skill-discovery metadata compatibility review

## Purpose and observer position

Perform one bounded, read-only review of the repository's installation, packaging and skill-discovery surfaces as they relate to the English operational edition.

Pass 4 / Checkpoint 01 defines installation and skill-discovery metadata compatibility as a Phase 5 requirement. Tasks 008–014 have been accepted within their separate boundaries. Pass 3 source-based translation review remains closed.

The question is not whether the English prose is translated correctly. It is whether the documented distribution routes and repository metadata expose a valid skill package, and whether a consumer following the English-facing installation instructions actually receives the intended skill entrypoint and its referenced files.

## Position and governing criteria

- Traversal: **Pass 4 — whole-skill English validation**.
- Project phase: **Phase 5 — Whole-skill validation**.
- Current position: Pass 4 / Checkpoint 08.
- Accepted operations: Tasks 008–014.
- Checkpoint 01: `reconstruction/zh/continuation/checkpoints/pass-4-checkpoint-01.md`, blob SHA `598722e61e115068af5c90d90dbdf8224dd7c764`.
- Checkpoint 08: `reconstruction/zh/continuation/checkpoints/pass-4-checkpoint-08.md`.
- Phase 5 criteria: `translation/README.md`, blob SHA `f9bf34ae28e838ae881f9e207797f26c1b327baa`.
- Inventory and packaging observations: `translation/INVENTORY.md`, blob SHA `0a8e58e9a6f45fd54c718672dc8d7a3a9056f7ee`.
- Context-segmentation boundary: `translation/EXECUTION-SEGMENTATION.md`, blob SHA `05ee569421b89366bbc0993afe62cb22a0644913`.

## Evidence boundary

### Repository evidence

Use the following files at their exact current branch revisions, and retrieve the current branch/tree before inspection to confirm the pins. If any listed SHA differs, stop and report the mismatch rather than silently substituting a revision.

- Root `SKILL.md`: `9d9ea8aca4afde6ced79154958864196cc3da433`.
- `.claude-plugin/marketplace.json`: `abdaaf3d1763db8108b3895b542088f201b3af16`.
- `.claude-plugin/plugin.json`: absent at task preparation (GitHub contents API returned 404); independently reconfirm its state.
- `README.en.md`: `f099921d88bc8b5f138250fa546ed4287d92aadb`.
- `README.md`: `3f268fc2cc6614edc40e90e7852326e6a1c6d385`.
- English operational entrypoint `translation/en/SKILL.md`: `f613aa7ef89eb9204c4f9804faf7aebace596a4e`.
- `translation/README.md`: `f9bf34ae28e838ae881f9e207797f26c1b327baa`.
- `translation/INVENTORY.md`: `0a8e58e9a6f45fd54c718672dc8d7a3a9056f7ee`.
- The relevant repository tree and directory listings needed to reconstruct each route's actual paths.

Do not inspect Chinese source prose to assess translation fidelity. Root `SKILL.md` may be inspected only for package location, frontmatter structure, metadata fields, language of discovery text, and references/path resolution. Do not compare its operational contents with the English target.

### Primary specifications

Only consult the following primary documentation, as retrieved on 2026-11-07. Record exact page title, URL, retrieval date, and the sections/requirements actually used. If the current page has materially changed, report the difference and pin the text consulted.

1. Agent Skills specification: `https://agentskills.io/specification`
   - Directory structure and `SKILL.md` frontmatter.
   - Required `name` and `description`, naming/length constraints and parent-directory name relationship.
   - Relative file references and validation guidance.
2. Claude Code plugin overview: `https://code.claude.com/docs/en/plugins`
   - Distinction between a standalone skill, plugin and marketplace.
   - Plugin manifest and marketplace roles.
3. Claude Code marketplace creation guide: `https://code.claude.com/docs/en/plugins/create-marketplace`
   - Marketplace file location, required fields, plugin source paths and validation.
4. Claude Code marketplace reference: `https://code.claude.com/docs/en/plugins/marketplace-reference`
   - Top-level fields, plugin entry fields, component fields, source resolution, and behavior when `plugin.json` is absent or present.
5. skills CLI documentation:
   - `https://www.skills.sh/docs/cli`
   - `https://github.com/vercel-labs/skills/blob/main/docs/commands.md`
   - Supported repository/path source forms, skill selection, installation layout and discovery behavior relevant to the README instructions.

Do not use third-party tutorials, forum posts, assumptions from memory or unrelated agent-specific conventions as compatibility authorities.

## Scope of work

1. Reconstruct the repository layout from the branch tree. Establish which directories are:
   - the repository root;
   - the root skill package, if any;
   - the English translation tree;
   - the Claude Code marketplace;
   - the plugin source selected by the marketplace entry.
2. Parse the repository's marketplace JSON and skill frontmatter as data. Check syntax and compare each field actually used against the current primary specification. Do not assume an unrecognised field is valid merely because JSON parsing succeeds.
3. Determine what the current Claude Code marketplace route resolves:
   - marketplace file location and required fields;
   - marketplace name and plugin entry name;
   - plugin source path resolution from marketplace root;
   - component declaration and the effect of the missing/present `plugin.json`;
   - which `SKILL.md` file is exposed to Claude Code;
   - whether its relative references resolve within the installed package.
4. Determine what the documented `npx skills add` route resolves:
   - repository root versus selected skill subdirectory;
   - how the CLI identifies the skill directory and `SKILL.md`;
   - which frontmatter and referenced files are installed;
   - whether the README command targets the default branch or the reconstruction branch;
   - whether the English translation can be selected through a documented supported path.
5. Review the manual clone and other English-facing installation instructions in `README.en.md`. For each route, reconstruct the exact path and entrypoint a consumer receives. Check the asserted skill directory name, metadata name and references.
6. Distinguish:
   - syntactic/schema compatibility;
   - package-layout compatibility;
   - install-command correctness;
   - whether the route selects the English or source-language entrypoint;
   - discovery-description language and likely discoverability;
   - a route that is valid for the upstream Chinese distribution but not an English edition;
   - uncertainty caused by a validator or installer not being available.
7. Record exact file paths, blob SHAs, relevant lines/JSON paths, spec citations/sections, and confidence for each finding.
8. If a command-line validator is already available in the worker environment, it may be run read-only against a temporary copy of the pinned files. Do not install tools, execute repository scripts, or perform network-dependent package installation. If no validator is available, record static analysis only.
9. Conclude whether the current English-facing installation/discovery surfaces are compatible, partially compatible, incompatible, or unresolved, and state precisely which distribution route and intended consumer that conclusion applies to.

## Exclusions

- Do not reopen Pass 3 or perform source-to-target semantic comparison.
- Do not rewrite, translate, rename, move or delete any skill, README, manifest or metadata file.
- Do not change the marketplace manifest, plugin manifest, frontmatter, directory layout, install commands or skill descriptions.
- Do not install or publish the skill, register a marketplace, modify agent settings, or test on a user's machine.
- Do not run untrusted scripts or install external packages.
- Do not assess technical correctness, current Android upstream status, device safety, or operational workflow quality.
- Do not assume that Chinese-language metadata is itself a schema incompatibility; classify schema validity separately from language/localisation suitability.
- Do not claim actual runtime installation success from static inspection or a validator alone.
- Do not treat this task as authority to choose a packaging redesign or modify the intended distribution model.

## Permitted changes

- Read-only with respect to repository content and project configuration.
- Append the required eight-field handoff to this task record only.
- After execution and handoff, move this task record unchanged except for the appended handoff to `translation/tasks/completed/`. This records worker execution only and does not imply orchestrator acceptance.

## Stop conditions

Stop and report if:
- any pinned repository file differs from its stated SHA;
- the marketplace or package tree cannot be reconstructed completely;
- the primary specifications materially conflict or do not describe the observed package form;
- a required compatibility conclusion depends on an unavailable validator or actual installation;
- answering would require translation, policy selection, package redesign or scope expansion.

## Validation

- Confirm branch identity and exact blob SHAs for all pinned repository files.
- Confirm the presence/absence of `.claude-plugin/plugin.json` and enumerate the marketplace/plugin source tree.
- Record primary specification URLs, retrieval dates and the exact rules used.
- Trace each documented installation route from command to source directory to selected `SKILL.md` to referenced files.
- Check JSON syntax and frontmatter structure independently; distinguish static conformance from runtime installation.
- Preserve the distinction between valid packaging for the source-language repository and valid distribution of the English edition.
- Record the read-only boundary and any unresolved dependency.

## Required handoff

Append a reconstructable handoff using the eight fields in `translation/EXECUTION-SEGMENTATION.md`:

1. Task identity.
2. Observer position.
3. Observations.
4. Decisions.
5. Unresolved matters.
6. Validation.
7. Change boundary.
8. Next dependency.

State whether the task was completed, stopped for an adequacy reason, or requires additional evidence. The worker must not claim orchestrator acceptance.


---

# Worker handoff — Task 015

**Execution status: COMPLETED — static compatibility review. Worker execution is finished; awaiting independent orchestrator acceptance.** No installation, publication, configuration change or runtime claim is made.

## 1. Task identity

- Repository: `incus-skills/android-mainline-port`.
- Branch: `translation/english-reconstruction`.
- Branch head inspected: `bb4c43f8a03fdc0e5bfded3b77acc2df832105be`.
- Branch tree: `a977c9ce3072d80a8aeb44248bbb7bac4571fabb`; recursive tree complete (`truncated: false`, 172 entries).
- Traversal: Pass 4 — whole-skill English validation; Phase 5; Checkpoint 08.
- Selected operation: Task 015 — installation and skill-discovery metadata compatibility review.
- Governing Checkpoint 01: `reconstruction/zh/continuation/checkpoints/pass-4-checkpoint-01.md`, blob `598722e61e115068af5c90d90dbdf8224dd7c764`.
- Governing Checkpoint 08: `reconstruction/zh/continuation/checkpoints/pass-4-checkpoint-08.md`, blob `4767279b7c191608e20ffc52d00d9346c2347229`.
- All task-pinned repository files were checked against the current recursive branch tree and fetched by path. Every stated SHA matched:
  - `SKILL.md`: `9d9ea8aca4afde6ced79154958864196cc3da433`.
  - `.claude-plugin/marketplace.json`: `abdaaf3d1763db8108b3895b542088f201b3af16`.
  - `.claude-plugin/plugin.json`: absent from the complete tree; contents lookup independently returned 404.
  - `README.en.md`: `f099921d88bc8b5f138250fa546ed4287d92aadb`.
  - `README.md`: `3f268fc2cc6614edc40e90e7852326e6a1c6d385`.
  - `translation/en/SKILL.md`: `f613aa7ef89eb9204c4f9804faf7aebace596a4e`.
  - `translation/README.md`: `f9bf34ae28e838ae881f9e207797f26c1b327baa`.
  - `translation/INVENTORY.md`: `0a8e58e9a6f45fd54c718672dc8d7a3a9056f7ee`.
  - `translation/EXECUTION-SEGMENTATION.md`: `05ee569421b89366bbc0993afe62cb22a0644913`.
- English subtree `translation/en/`: tree `ba8a19206173ccde8fd26b6ae731ca4a0ee8c7cc`; its recursive listing contains 22 blobs and is not truncated.

## 2. Observer position

The question was whether English-facing installation and discovery instructions resolve to a valid English operational skill package, and whether the manifests and package layout satisfy the applicable published formats.

This was a static packaging and route-resolution review. It did not reopen Pass 3, compare Chinese and English operational prose, assess Android technical correctness, or decide a packaging redesign. The root `SKILL.md` was used only for frontmatter, package location, description language and reference/path resolution, as authorised by the task.

## 3. Observations

### Repository identity and branch selection

- The README installation commands use `yzddmr6/android-mainline-port`, not the repository name `incus-skills/android-mainline-port`.
- These are distinct GitHub repository identities. The README target `yzddmr6/android-mainline-port` has repository ID `1427247107` and default branch `main`.
- This is not an unrelated content fork: the `yzddmr6` main tree SHA is `58a97a59ded09e79c15e2e1a081907ef96581a34`, exactly the same tree SHA as `incus-skills/android-mainline-port` main. The root `SKILL.md` and `.claude-plugin/marketplace.json` blob SHAs are also identical in both.
- The reconstruction branch adds `translation/en/`; that directory is absent from the canonical upstream `main` tree. The branch-specific English edition therefore is not what the README's unqualified upstream repository URLs select.

### Package layout and Agent Skills metadata

- The repository root is the source-language skill package: root `SKILL.md`, `references/` and `experience/`.
- Root `SKILL.md` frontmatter has `name: android-mainline-port` and a non-empty Chinese description (186 characters). The name is 21 characters and uses the permitted lowercase letters and hyphens. The README's manual clone destination is explicitly named `android-mainline-port`, matching the metadata name.
- The English entrypoint is `translation/en/SKILL.md`, with the same valid 21-character name and a non-empty English description (525 characters). The YAML frontmatter structure parses. However, the immediate containing directory is named `en`, not `android-mainline-port`. This conflicts with the Agent Skills specification's explicit rule that the `name` must match the parent directory name.
- The English entrypoint's relative references are rooted at `translation/en/`. All 15 distinct referenced Markdown paths under `references/` and `experience/` were checked against the complete English tree; none are missing. The tree has the expected entrypoint and supporting reference/experience files.
- This is a package-directory conformance issue, not a broken-relative-reference finding and not a translation-fidelity finding.

### Claude Code marketplace route

- `.claude-plugin/marketplace.json` is at the documented marketplace location. Its JSON parses successfully.
- The top-level `name`, `owner.name` and `plugins` fields are present. The marketplace name and plugin entry name are both `android-mainline-port`.
- The plugin entry has the required `name` and `source`; its `description` and `skills` fields are recognised marketplace-entry fields.
- `source: "./"` resolves from the marketplace root, which is the repository root, not from `.claude-plugin/`.
- `skills: ["./"]` declares the root directory as the skill component. The root directory contains the root `SKILL.md`; it does not select `translation/en/SKILL.md`.
- The absence of `.claude-plugin/plugin.json` is not, by itself, a schema defect. The current marketplace reference explicitly states that when `plugin.json` is absent, the marketplace entry acts as the plugin manifest, including its component fields.
- The marketplace and plugin descriptions are Chinese. Neither the Agent Skills nor marketplace schema consulted imposes an English-language requirement on these description strings. Their language is therefore not a syntax/schema incompatibility, although it is a localisation and likely English-discoverability concern.
- The README's plugin install identifier, `android-mainline-port@android-mainline-port`, matches the plugin and marketplace names. The route selects the source-language root package, not the English reconstruction.

### skills CLI route

- Pinned `README.en.md` lines 51–61 documents `npx skills add yzddmr6/android-mainline-port -g`. It supplies no branch or subdirectory selector.
- The target repository's default branch is `main`, whose tree contains the root source-language `SKILL.md` and does not contain `translation/en/`. Thus this documented command selects the upstream root skill, not the English edition.
- The CLI command reference documents both repository sources and a direct GitHub path to a skill subdirectory (example form: `.../tree/main/skills/<skill-dir>`). It also documents `--skill` selection and global/project install scope. A direct subdirectory source is therefore a documented selection mechanism in general.
- The English candidate is on the non-default reconstruction branch at `translation/en/`; selecting it requires an explicit branch/subdirectory source rather than the README's unqualified repository command. No actual CLI invocation was run. More importantly, the candidate directory currently fails the Agent Skills parent-directory-name rule described above. The existence of a direct-path mechanism does not establish that this current directory is a fully conforming Agent Skills package.
- The CLI documentation also supports selection by skill name, but this reconstruction branch contains both root and nested `SKILL.md` files with the same frontmatter name. The docs consulted do not specify how a name-only selection resolves that duplicate in this repository; the direct path is the unambiguous documented mechanism to investigate separately.

### Other README routes

- Manual clone instructions (pinned `README.en.md` lines 87–105) clone the repository URL without a branch and place it at `.../android-mainline-port`. That resolves to the upstream default branch and root `SKILL.md`; the directory name matches the root skill metadata. The references and experience directories exist. This is a valid route for the source-language package, not the English edition.
- The “Let your agent install it” prompt (lines 117–124) repeats the same unqualified clone and exact destination name, so it has the same result.
- The Gemini CLI command (line 84) points to the same upstream repository URL. The repository root contains the source-language `SKILL.md`, but Gemini-specific selection/install semantics were not established by the primary specifications permitted for this task. Exact runtime discovery remains unverified.
- The claude.ai/desktop route (lines 110–114) downloads a release ZIP from the upstream repository. The release asset was not inspected; its exact packaged contents and selected entrypoint remain unverified.
- The README explicitly states at line 192: “The skill itself is written in Chinese”. This is consistent with the root package and the upstream distribution routes. It does not describe the English reconstruction branch as the installed edition.

### Primary specifications consulted

All pages below were retrieved on **2026-11-07**. No material conflict affecting the observed package form was encountered.

1. **Specification — Agent Skills** — https://agentskills.io/specification
   - “Directory structure” and “SKILL.md format” / “Frontmatter”: a skill is a directory containing `SKILL.md`; `name` and `description` are required.
   - “name field”: 1–64 characters; lowercase alphanumeric characters and hyphens; no leading/trailing or consecutive hyphens; must match the parent directory name.
   - “description field”: 1–1024 characters, non-empty, describing what the skill does and when to use it.
   - “File references”: references are relative to the skill root.
   - “Validation”: identifies `skills-ref validate` as the reference validator.
2. **Plugins overview — Claude Code Docs** — https://code.claude.com/docs/en/plugins
   - “Understand what a plugin is”: a plugin is a directory of components, usually with `.claude-plugin/plugin.json`; skills are one component type.
   - “Decide whether you need a plugin”: skills can exist independently; plugins package components as one installable unit.
3. **Create a marketplace — Claude Code Docs** — https://code.claude.com/docs/en/plugins/create-marketplace
   - Marketplace directory and `.claude-plugin/marketplace.json` location.
   - Required marketplace fields: `name`, `owner`, `plugins`; each plugin entry requires `name` and `source`.
   - Relative plugin source is written from marketplace root; validation checks JSON syntax and required fields.
4. **Marketplace reference — Claude Code Docs** — https://code.claude.com/docs/en/plugins/marketplace-reference
   - “Marketplace file” and “Top-level fields”: marketplace root and relative path resolution.
   - “Plugin entries”: required `name` and `source`, accepted component fields.
   - “How an entry combines with plugin.json” and “Strict mode”: absent `plugin.json` means the entry is the manifest; when present, manifest/entry component combination follows strict-mode rules.
   - “Plugin sources” / “Relative path plugin source”: `./` paths resolve from marketplace root.
5. **CLI Reference — Skills Documentation** — https://www.skills.sh/docs/cli
   - “Basic usage”: repository owner/name form.
6. **CLI reference — vercel-labs/skills** — https://github.com/vercel-labs/skills/blob/main/docs/commands.md
   - “skills add” / “Source Formats”: repository, full GitHub URL and direct path to a skill subdirectory.
   - “Options”: `--skill`, `--agent`, `--global`.
   - “Installation Scope”: project versus global destinations.

## 4. Decisions

- **Overall result: PARTIALLY COMPATIBLE**, with a clear route-specific distinction:
  - The documented upstream routes are structurally compatible with the source-language Chinese distribution.
  - They are not routes to the English operational edition on `translation/english-reconstruction`.
  - The English candidate has complete relative references but is not currently directory-name-conforming under the Agent Skills specification.
- The Claude Code marketplace JSON is statically schema-compatible for the observed source-language plugin. Missing `plugin.json` is not a defect in this configuration.
- Chinese marketplace and skill descriptions are valid strings, not schema errors. They remain a localisation/discoverability issue for an English-facing distribution.
- The documented `npx skills add` and manual clone commands are valid for the upstream default-branch root skill. They do not select the English reconstruction.
- A documented CLI direct-subdirectory source mechanism exists, but the current English directory's naming discrepancy and the absence of a runtime test prevent a claim that the English package is presently compatible or install-verified.
- No repository content, metadata, installation command or package layout was changed.

## 5. Unresolved matters

- Gemini CLI's exact resolution behaviour for the documented Git URL was not established from the task's authorised primary documentation.
- The contents of the upstream release ZIP used by the claude.ai/desktop route were not inspected.
- No actual CLI installation or Claude Code plugin load was performed; static route reconstruction is not runtime success evidence.
- No `claude plugin validate` or `skills-ref validate` executable was available in the worker environment. Static analysis was used; no tool was installed.
- The CLI documentation supports direct subdirectory selection, but the exact branch-with-slash URL was not exercised. Name-only `--skill` selection with two same-named frontmatter entries was not specified by the docs consulted.
- The README's English-facing install text is a guide to the canonical upstream source-language distribution. Whether the project intends that to remain the sole published distribution or intends a distinct English distribution is a project-level decision outside this task.
- No separate investigation proposal was created; the unresolved items above are evidence limits of the current bounded review.

## 6. Validation

- Confirmed current branch head and complete recursive tree before inspection.
- Confirmed every pinned file SHA, including both governing checkpoints and the segmentation document.
- Confirmed `.claude-plugin/plugin.json` is absent in the complete tree and by independent 404 lookup.
- Parsed `.claude-plugin/marketplace.json` as JSON successfully.
- Parsed both skill frontmatter mappings as YAML data. Both names are 21 characters; descriptions are non-empty and within the 1,024-character limit. Both names satisfy character constraints. The English parent-directory mismatch is recorded separately.
- Verified all 15 distinct `references/` and `experience/` Markdown paths referenced by `translation/en/SKILL.md` exist in the complete 22-blob English subtree.
- Compared the README's actual target repository identity and default branch. Confirmed the upstream `main` tree SHA is identical to the reconstruction repository's `main` tree SHA, while the English subtree exists only on the reconstruction branch.
- Retrieved all six primary specification pages listed above on 2026-11-07 and recorded the sections/rules used.
- Checked for pre-existing `claude`, `skills-ref` and `skills` executables; none were available. No packages were installed and no network-dependent installation was attempted.
- Validation type: static repository, schema and path analysis only. No claim of runtime installation or discovery success.

## 7. Change boundary

- Repository content was read-only during investigation.
- The only authorised write is this appended handoff in the Task 015 record, followed by moving that record to `translation/tasks/completed/`.
- No skill, README, manifest, frontmatter, source document, translation, inventory, checkpoint or project-position file was changed.
- No task-management machinery, claim/lock, or new worker task was created.

## 8. Next dependency

- The orchestrator should independently reconstruct and assess this handoff before accepting Task 015.
- If an English distribution is intended, any decision about branch selection, directory naming, marketplace component path, README commands or release packaging requires a separately bounded project-level task. This worker has made no redesign choice or correction.
- If Gemini CLI or release-ZIP compatibility remains an acceptance requirement, obtain the relevant primary installer documentation or inspect the exact published artifact under a separately authorised boundary.
- Task execution is complete. **Orchestrator acceptance remains pending.**
