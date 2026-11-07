# Pass 4 — Checkpoint 09: Task 015 accepted; distribution intent is the next dependency

## Position

- Traversal: **Pass 4 — whole-skill English validation**.
- Project phase: **Phase 5 — Whole-skill validation**.
- Previous checkpoint: Pass 4 / Checkpoint 08.
- Accepted operations: Tasks 008–015.
- **Task 015 is ACCEPTED**, limited to static installation, package-layout, metadata-schema and documented-route resolution review.
- Pass 4 remains open. This checkpoint does not establish runtime installation, successful activation, full Phase 5 completion or context-segmentation effectiveness.

## Task 015 — independent reconstruction and acceptance

### Repository and handoff state

- Completed record: `translation/tasks/completed/015-phase5-installation-skill-discovery-compatibility.md`.
- Completed-record blob SHA: `44e21f48a98e9a79c8c9a0ac6506fde756435e0c`.
- Worker-reported move commit: `eace1b2ad38a926719bd5835959acc8c4acf9933`.
- Independently fetched the completed record from `translation/english-reconstruction`; its content and eight-field handoff are present.
- The move commit removes the pending task path. No skill content or metadata change is reported or required by this task.
- Pinned repository files inspected in the handoff include root `SKILL.md`, `.claude-plugin/marketplace.json`, `README.en.md`, `README.md`, `translation/en/SKILL.md`, `translation/README.md`, `translation/INVENTORY.md` and `translation/EXECUTION-SEGMENTATION.md`. The handoff records matching blob SHAs for these pins and absence of `.claude-plugin/plugin.json`.

### Independent evidence reconstruction

I independently retrieved:
- the completed Task 015 record and its eight-field handoff;
- the current-branch root `SKILL.md`;
- `.claude-plugin/marketplace.json`;
- `README.en.md`;
- `README.md`;
- `translation/en/SKILL.md`;
- `translation/README.md`;
- `translation/INVENTORY.md`;
- Checkpoint 01 and Checkpoint 08;
- the canonical upstream `yzddmr6/android-mainline-port` `README.en.md` on `main`.

The retrieved repository files match the pinned blob SHAs recorded by the worker. The current English entrypoint's directory is `translation/en/`; its declared skill name is `android-mainline-port`. Its relative reference paths are rooted within that English tree. The worker's full-tree inspection reports all 15 distinct referenced Markdown paths exist in the pinned 22-blob English subtree.

### Findings confirmed

1. **Repository identity and target branch are material.** The English README's install commands name `yzddmr6/android-mainline-port`, not `incus-skills/android-mainline-port`. These are distinct GitHub repository identities. The handoff records that their canonical `main` trees have the same tree SHA at the inspected baseline, while the reconstruction branch adds `translation/en/`. The unqualified README commands do not select that reconstruction branch or its English subtree.
2. **The existing root package is internally aligned.** Root `SKILL.md` declares `name: android-mainline-port`; the manual-clone destination uses the same directory name. The root package is the source-language distribution.
3. **The English package has a directory-name discrepancy.** `translation/en/SKILL.md` declares `name: android-mainline-port`, but its immediate parent directory is `en`. The Agent Skills specification requires the skill name to match its parent directory. This is a package-layout conformance finding, not a translation-fidelity or broken-reference finding.
4. **The Claude Code marketplace selects the root package.** The marketplace JSON parses and has the documented required fields. `source: "./"` resolves to the repository root and `skills: ["./"]` declares the root skill component. It does not select `translation/en/SKILL.md`.
5. **Missing `plugin.json` is not a schema defect here.** The current Claude Code marketplace reference allows the marketplace entry to act as the plugin manifest when `plugin.json` is absent. The marketplace route is structurally valid for the root/source-language package.
6. **Chinese discovery descriptions are not schema violations.** The applicable schemas do not require English description strings. Their language remains a localisation and likely English-discoverability concern for an English-facing distribution.
7. **The documented skills CLI command selects the upstream default-branch root package.** The README's `npx skills add yzddmr6/android-mainline-port -g` has no branch or subdirectory selector. The consulted CLI documentation supports direct subdirectory sources in general, but the current English directory-name discrepancy remains.
8. **Manual clone and agent-assisted clone instructions also select the upstream root package.** They do not select the English reconstruction.
9. **Some route behaviour remains unverified.** Gemini CLI's exact resolution, the contents of the claude.ai release ZIP, branch-with-slash direct-path behaviour, name-only selection when duplicate skill names exist, and actual runtime installation/plugin loading were not tested.

### Acceptance decision

**ACCEPTED — Task 015, limited to static compatibility analysis and reconstruction of documented route targets.**

The handoff follows the required eight-field structure, identifies exact repository pins and primary specification sections, distinguishes schema validity from language suitability and package layout, and avoids treating static analysis as runtime success. Its central findings are supported by the retrieved repository artifacts and the cited specification requirements. No scope expansion or unauthorised content change occurred.

Acceptance does not establish:
- runtime installation or successful skill activation in any agent;
- Gemini CLI or claude.ai release-ZIP compatibility;
- that the project has chosen to publish the English reconstruction as a separate distribution;
- that the current English package is fully Agent Skills-conforming;
- Pass 4 completion or context-segmentation effectiveness.

## Pass 4 position after Task 015

- Tasks 008–015 are accepted, each within its recorded validation boundary.
- The README's documented distribution routes are valid for the source-language root package but do not select the English reconstruction.
- The English entrypoint's relative references resolve, but its parent-directory name does not match its declared Agent Skills name.
- The current evidence does not establish whether the intended publication model is to keep the upstream Chinese distribution as the only installable package, or to expose a separately installable English edition.
- No packaging redesign, metadata correction, README change or branch-selection policy is authorised by Task 015.
- Task 013's stage/record-contract gaps, Stage 8 completion omission and status-vocabulary discrepancy remain unresolved.
- The stale “Application to the current position” paragraph in `translation/EXECUTION-SEGMENTATION.md` remains unresolved and non-authoritative.
- Pass 3 remains closed. No source-based translation review is reopened.

## Next dependency — distribution intent

Before preparing a corrective packaging or installation task, the project owner/orchestrator must establish whether the intended product includes a separately installable English edition, or whether the branch is currently only a reconstruction/validation workspace while public installation continues to target the upstream source-language package.

This is a project-level distribution decision, not something a worker may infer from the presence of `translation/en/`. Task 015 supplies enough evidence to frame that decision; it does not authorise a package redesign or select among possible layouts.

Until that intent is established:
- do not rename or move `translation/en/`;
- do not alter marketplace component paths or frontmatter;
- do not rewrite README install commands;
- do not claim the English edition is install-ready;
- do not dispatch a worker to implement a guessed distribution model.

## Continuity

- This checkpoint records the separate orchestrator acceptance of Task 015.
- Completed Task 015 record remains at blob `44e21f48a98e9a79c8c9a0ac6506fde756435e0c`.
- The worker's completion/move commit is `eace1b2ad38a926719bd5835959acc8c4acf9933`.
- Task 015's acceptance does not close Pass 4.
