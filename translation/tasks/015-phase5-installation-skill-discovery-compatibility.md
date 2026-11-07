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
