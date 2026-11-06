# Task 006 — Pass 3 review of experience/build-and-release.md

## Purpose and observer position

Perform the Pass 3 technical and semantic translation review of the build pipeline and release experience document. Compare the complete Chinese source with its English translation. Preserve source meaning and operational force; do not validate technical claims against current upstream implementations, actual build infrastructure or release operations.

This is a bounded document-level operation within Pass 3, not a new pass, build audit or release-process redesign.

## Position and governing criteria

- Pass: Pass 3 — technical and semantic review of the operational English translation set.
- Previous operation: Task 005 — Pass 3 review of `experience/ubuntu-userspace.md`; its worker handoff and independent acceptance are recorded in [completed Task 005](https://github.com/incus-skills/android-mainline-port/blob/translation/english-reconstruction/translation/tasks/completed/005-pass3-experience-ubuntu-userspace.md) and [Pass 3 / Checkpoint 21](https://github.com/incus-skills/android-mainline-port/blob/translation/english-reconstruction/reconstruction/zh/continuation/checkpoints/pass-3-checkpoint-21.md).
- Current position: Pass 3 / Checkpoint 21; `experience/ubuntu-userspace.md` is accepted and `experience/build-and-release.md` is next.
- Next-document ordering is corroborated by the source experience-library index and its English counterpart. This is document-order evidence, not proof that Ubuntu userspace findings determine the build/release analysis.
- Pass 3 scope and review dimensions: `reconstruction/zh/continuation/checkpoints/pass-3-checkpoint-01.md`.
- Applicable translation principles and Phase 4 review instructions: `translation/README.md`.
- Controlled terminology: `translation/GLOSSARY.md`.
- Execution boundary and handoff fields: `translation/EXECUTION-SEGMENTATION.md`.

The Chinese source explicitly says complete build-environment setup is described in `../references/build-environment.md`, and that this experience document records specific pitfalls only. Preserve that boundary and the reference. The corresponding source and English build-environment reference files are included below only to verify the named reference/counterpart and its boundary; do not turn this task into a review or technical audit of the reference.

## Exact evidence baseline

Fetch these exact files and verify the revisions before relying on them:

| Role | Repository path | Ref | Blob SHA |
|---|---|---|---|
| Chinese source | `experience/build-and-release.md` | `main` | `05c40c2b974cd14f229db814bc1ef63b1e657c65` |
| English target | `translation/en/experience/build-and-release.md` | `translation/english-reconstruction` | `411a9d962428bff35f6a8c66116ce789290028e2` |
| Existing review (Pass 1) | `translation/reviews/experience/build-and-release.md.md` | `translation/english-reconstruction` | `6d3352f43645d60f4e2329fd53b88e488c935d35` |
| Pass 3 criteria | `reconstruction/zh/continuation/checkpoints/pass-3-checkpoint-01.md` | `translation/english-reconstruction` | `0870d3ea261a6354649379b96891cda4ca2a0ba6` |
| Current position | `reconstruction/zh/continuation/current-position.md` | `translation/english-reconstruction` | `eb6d266e7661c74a14afd1ac99cf7e5bbf6c4a26` |
| Current checkpoint | `reconstruction/zh/continuation/checkpoints/pass-3-checkpoint-21.md` | `translation/english-reconstruction` | `55694ab10197f62e80f2e7532df1a98993054e59` |
| Completed preceding task | `translation/tasks/completed/005-pass3-experience-ubuntu-userspace.md` | `translation/english-reconstruction` | `fa6f8f447ced559cfda11a3079fae49d66aec665` |
| Glossary | `translation/GLOSSARY.md` | `translation/english-reconstruction` | `7cd456b51223fd2cd3cf408cb14bae7f7d167254` |
| Translation instructions | `translation/README.md` | `translation/english-reconstruction` | `f9bf34ae28e838ae881f9e207797f26c1b327baa` |
| Segmentation arrangement | `translation/EXECUTION-SEGMENTATION.md` | `translation/english-reconstruction` | `412268a0c065a628d65c0c2c1799830f718e92d1` |
| Source experience-library index | `experience/README.md` | `main` | `b8b4b9110215280c35316df33d540e292c09356a` |
| English experience-library index | `translation/en/experience/README.md` | `translation/english-reconstruction` | `a7a28190615253246749909b380ddcba17268769` |
| Explicitly referenced build-environment source (boundary context only) | `references/build-environment.md` | `main` | `7cc91c8beda502217a4b43a5ca60550eaa608a1e` |
| English counterpart of build-environment reference (boundary context only) | `translation/en/references/build-environment.md` | `translation/english-reconstruction` | `0b1e27b7939d6e6062d9abe567ba565ed8042bb8` |

The existing review is Pass 1 provenance, not a substitute for comparing the complete source and target afresh. Retain its distinctions between source-explicit references and observer-reconstructed correspondences. Do not load additional domain documents unless a concrete translation claim requires them.

## Scope of work

Review the complete source/target pair against all seven Pass 3 dimensions:

1. Imperative force: requirements, prohibitions, recommendations, defaults, examples and cautions.
2. Conditional scope: applicability, exceptions and limits.
3. Decision logic: conditions, branches, alternatives and owner decisions.
4. Technical tokens: commands, paths, identifiers, package names, environment variables, tools, versions, numeric values and configuration.
5. Structure: headings, lists, examples and relationships.
6. Cross-document references: direct links and named dependencies, distinguishing source-explicit relationships from review-record or observer-reconstructed correspondences.
7. Uncertainty: preserve ambiguity and missing source information rather than silently resolving it.

Pay particular attention to preserving distinctions among:
- the document's specific-pitfall role and its explicit delegation of complete build-environment setup to `references/build-environment.md`;
- measurement of build stages versus assumptions about bottlenecks, and source-reported performance observations versus independently verified benchmarks;
- stable OUT paths, ccache `base_dir`, cache manifests, build outputs and recorded artifact identity;
- native ARM64, root-privileged qemu-user chroot and full-system QEMU/TCG, including their different uses and constraints;
- user-namespace UID/GID mapping, `newuidmap`, root privileges and device-node creation;
- dracut compression behaviour, Mesa NIR generation and environment-specific performance;
- one-writer OUT discipline, `flock`, source HEAD verification, build-input identity and recorded OUT immutability;
- reproducibility inputs: compiler path mapping, `KCFLAGS`/`KAFLAGS`, build timestamp/user/host, permissions, timestamps, APT indexes, host `dtc`, AVB salt and deterministic archive ordering;
- release manifests, receipts, source archives, package artifacts, hashes, cleanup and identity records;
- GitHub release attachment limits, split rootfs volumes, grouped artifacts and user-facing release notes;
- fork behaviour of `gh repo fork`, GPL source obligations and distribution trademark wording;
- release package versions versus already-installed internal versions and `~dev` suffixes;
- published-tag immutability, sparse image preparation, Mesa incremental rebuild identity and host saturation;
- version-by-version scope/acceptance, development, testing, release and cleanup sequence without inventing a formal state machine;
- public documentation versus internal investigations, technical debt and root-cause material.

Do not combine these into a universal build pipeline, reproducibility guarantee, cache policy or release state machine. Preserve each pitfall, condition, cause, remedy, applicability and acceptance criterion as the source presents it. Do not infer that a source omission has been resolved merely because the source has no dedicated unresolved section.

## Permitted changes

- Modify only `translation/en/experience/build-and-release.md` where a material, source-justified correction is required.
- Append a clearly labelled Pass 3 section to `translation/reviews/experience/build-and-release.md.md`, preserving the existing Pass 1 record.
- Do not modify the Chinese source, build-environment reference, Pass 3 criteria, current-position/checkpoint records, glossary, inventory, indexes or unrelated files.
- Do not rewrite for style alone.
- If no translation correction is justified, leave the target unchanged and record that result.

## Stop conditions

Stop and report rather than broaden the task if:
- the source, target or governing criteria revision differs from the stated baseline in a material way;
- a required source/target passage cannot be obtained;
- a translation decision depends on current build infrastructure, external technical correctness, benchmarking or actual release operations;
- a cross-document relationship cannot be distinguished from a source-explicit link or review-record observation;
- the task cannot be completed within the stated scope.

Do not resolve external technical questions or silently expand the task.

## Validation

- Re-read the complete changed target and review record.
- Confirm the Chinese source is unchanged.
- Confirm every proposed target change is supported by the Chinese source.
- Confirm the explicit `references/build-environment.md` boundary and other source-explicit references remain accurately represented.
- Confirm source-explicit references and review-record correspondences remain distinguished.
- Record the exact revisions examined, changed files, checks performed and unresolved matters.
- Keep the Pass 1 review content intact.

## Required handoff

Append the reconstructable handoff to this task record, following the eight fields in `translation/EXECUTION-SEGMENTATION.md`: task identity; observer position; observations; decisions; unresolved matters; validation; change boundary; next dependency.

The handoff must state whether the task was completed, stopped for an adequacy reason, or requires further evidence. Do not move this task to `completed/` until worker execution and handoff are complete. Moving it does not constitute orchestrator acceptance.
