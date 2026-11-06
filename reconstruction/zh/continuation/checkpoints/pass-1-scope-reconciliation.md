# Pass 1 — Scope Reconciliation and Closure

## 1. Pass identity

- Pass: **Pass 1 — initial structural and linguistic reconstruction**
- Final document checkpoint: [Pass 1 / Checkpoint 12](checkpoints/pass-1-checkpoint-12.md)
- Scope reconciliation checkpoint: **Checkpoint 13**
- Source branch: `main`
- Reconstruction branch: `translation/english-reconstruction`
- Purpose: recover the operational skill's structure, language, document roles, relationships and evidence boundaries. This is not technical validation or whole-skill operational validation.

## 2. Declared operational-document scope

The translation workflow in `translation/README.md` defines the Phase 1 exit condition as being able to explain the framework, experience library and their relationship without relying on translated prose alone.

The operational core consists of:

- `SKILL.md`: skill entry point and navigation contract.
- Six framework references under `references/`: process, discipline, decisions, build environment, verification and Ubuntu integration.
- Fifteen experience-library documents under `experience/`, including the collection index and problem-pattern index.

The source tree and its indexes were checked against this set. Each operational document has an English counterpart in `translation/en/`; translation review records are maintained separately under `translation/reviews/`. The experience-library traversal concluded with `experience/testing-and-process.md` at Checkpoint 12.

The initial structural work also recovered the broad relationship between the entry point, framework references, experience library, stage process, task-entry routes, preservation records, verification evidence and cross-document pattern references. Source-explicit relationships remain distinguished from observer-reconstructed correspondences in the document reviews and checkpoints.

## 3. Other repository materials and their treatment

The original repository has 28 files in the recorded baseline. The remaining six are not part of the operational-document set above:

| Material | Observed role | Pass 1 treatment | Deferred work |
|---|---|---|---|
| `README.md` | Chinese repository-facing overview, installation instructions, use examples, source-project context and high-level content map | Read to establish its orientation role and relation to the operational core; not treated as an operational procedure document | Detailed user-facing translation/localisation and consistency review |
| `README.en.md` | Existing English orientation and installation guide | Read as an existing English aid, not treated as authority over the Chinese operational documents | Whole-document consistency and current installation compatibility review |
| `.claude-plugin/marketplace.json` | Claude plugin marketplace and skill-registration metadata | Inspected; its descriptions are Chinese metadata, not operational skill instructions | Localisation and integration compatibility decision |
| `LICENSE` | MIT legal notice and copyright attribution | Preserve unchanged; no translation required for this pass | Verify preservation in any later distribution/package work |
| `docs/assets/gzh-qr.png` | WeChat/community reference image | Identified as non-text supporting material | Decide retain/exclude treatment for an English-facing distribution |
| `docs/assets/xhs-qr.jpg` | Xiaohongshu/community reference image | Identified as non-text supporting material | Decide retain/exclude treatment for an English-facing distribution |

This distinction follows the workflow's own separation between the operational framework/experience documents and "supporting examples and any remaining user-facing material" in the later translation phase, followed by integration metadata checks during whole-skill validation.

## 4. Pass 1 exit assessment

**Pass 1 is complete for its declared scope: the operational skill core and its structural relationships.**

The completion basis is:

1. The entry point, six framework references and fifteen experience documents have been traversed.
2. English counterparts and separate source-based review records exist for the operational documents.
3. The framework/experience relationship and major operational relationships can be described without relying on English prose alone.
4. Explicit source links, reverse index references, observer-reconstructed correspondences and source absences have been kept distinct.
5. The final experience-library document has been examined and recorded.
6. Repository-facing, integration, legal and non-text materials have been classified rather than silently treated as translated operational content.

This is not a claim that every repository file has been translated, reviewed for current correctness, or validated as an integrated English skill.

## 5. Work explicitly not completed by this Pass

- Detailed localisation and consistency review of `README.md` and `README.en.md`.
- Marketplace metadata localisation or integration compatibility validation.
- Final distribution treatment of the two community QR assets.
- Technical validation of Android, kernel, firmware, hardware, build or release claims.
- Whole-skill English link, navigation, activation and representative-workflow validation.
- A later traversal from a changed observer position, including comparison of new, changed or disappearing distinctions.

These are separate work items and must not be silently folded into Pass 1's completion claim.

## 6. Next traversal boundary

No Pass 2 is started by this checkpoint. A later traversal must declare its purpose and scope before beginning. It may be a technical/semantic validation pass, a whole-skill integration pass, or another explicitly scoped traversal; these are not interchangeable and should not be conflated.

The next activity is therefore a planning decision, not an automatically selected next source document. Preserve the current Pass 1 records as the baseline against which a later traversal can compare its observer position.

## 7. Source and validation boundary

- Source inventory baseline: `translation/INVENTORY.md`, based on the recorded `main` tree.
- Workflow phase definitions: `translation/README.md`.
- Repository-facing sources inspected: `README.md`, `README.en.md`, `.claude-plugin/marketplace.json`, `LICENSE`.
- Technical validation: not performed.
- Whole-skill operational validation: not performed.
- Pass 1 closure: structural and linguistic scope only.
