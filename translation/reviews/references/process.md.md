# Reconstruction and Translation Record: references/process.md

## Source and provenance

- Source path: `references/process.md`
- Source ref: `main`
- Source blob SHA: `799e94e94a591e6d16caf68a5a30686c6b35171a`
- English artifact: `translation/en/references/process.md`
- Translation branch: `translation/english-reconstruction`
- English artifact blob SHA at review: `3e770386533dda9f0b5162043c08e9ffea454467`
- Translation commit: `30d7e9b669463621e3eb4e0e28c852bfc3f34380`

## Translation intent

This is a functional English translation of the nine-stage project process. It is not a revised workflow, an endorsement of the Android procedures, or technical validation of the steps.

This record is being added retrospectively so the earlier process translation has the same durable reconstruction record now adopted for subsequent documents. The first-pass objective is to recover the sequence, dependencies, parallel work, stage outputs and completion criteria expressed by the source. The validity of individual technical claims is reserved for a separate technical-evidence pass.

Preserve:
- all nine stages, their order, goals, actions, outputs and completion criteria;
- the explicit parallel-work rules and the limits on real-device concurrency;
- the distinction between initial RAM bring-up, persistent installation, peripheral adaptation and release;
- the stated version rhythm and maintenance activities;
- links to related discipline, decision and experience documents;
- commands, paths, interface names, configuration terms and technical identifiers;
- omissions and boundaries, including the absence of a formal Stage 8 completion criterion.

Do not turn the stage list into an exclusively linear process, move persistent installation behind completion of all peripherals, or invent lifecycle exit criteria.

## Reconstructed document role

This document is the detailed project procedure. It expands the nine-stage overview in `SKILL.md` into goals, ordered actions, outputs, completion criteria and related experience.

The stages are:

0. **Project scoping** — workspace, source pool, reuse research, assessment and user decisions; explicitly no device contact.
1. **Build environment** — toolchain, cache, reproducible community baseline and build scripts.
2. **Stock-device evidence and backup** — read-only access, root, partition/GPT backups, device evidence and recovery rehearsal.
3. **First boot** — bootloader behaviour, minimal board DTS, RAM initramfs, shell, debugging channel and automatic recovery.
4. **First display and RAM desktop** — display route, parallel native takeover preparation, touch, RAM root, GNOME and desktop automation.
5. **Install to storage** — partitioning, rootfs delivery, dual boot, debugging channel, cold boot and in-system kernel update path.
6. **Adapt peripherals individually** — prioritised, isolated peripheral investigation, real-device evidence, integration and cold-boot regression.
7. **Package, clean-install acceptance and release** — feature freeze, packaging, clean installation, first-hour audit, documentation and user-approved publication.
8. **Maintenance** — upstream replacement, technical-debt reduction, cold-boot checks and experience updates.

The source explicitly permits:
- Stage 1 to overlap with the latter part of Stage 0.
- Native display takeover preparation to run in parallel with other Stage 4 work.
- Offline preparation for Stage 6 peripherals to run in parallel, while real-device testing remains one peripheral at a time.
- Stage 5 to begin as soon as the desktop runs in RAM; it must not wait for all peripherals to be completed.
- Stages 6 and 7 to repeat by release version.

The process is therefore staged but not strictly serial. Its explicit dependencies and concurrency constraints are part of the procedure.

## Terminology and translation decisions

- `立项` → **Project scoping**. Retains the stage's assessment and user-decision role rather than implying only administrative project initiation.
- `原厂取证与备份` → **Stock-device evidence and backup**. Preserves evidence acquisition from the original system as well as backup.
- `首次启动` → **First boot**.
- `首次点亮与内存桌面` → **First display and RAM desktop**. Retains both visible display and the RAM-resident desktop milestone.
- `安装到存储` → **Install to storage**. Distinguishes persistent installation from RAM operation.
- `外设逐个适配` → **Adapt peripherals individually**. Preserves the one-at-a-time real-device test constraint.
- `还技术债` → **Pay down technical debt**.
- `真机通过` → **Passes on real device**, consistent with the evidence vocabulary in `verification.md`.
- `原生接管` → **native takeover**, consistent with the glossary and decision catalogue.
- `标准答案` → **reference answers** / **reference state** according to context. It refers to stock-device evidence used as a comparison baseline, not an assertion that the stock implementation is universally correct.
- `上机` → **on-device** or **real-device** testing according to context.

Technical identifiers and commands are retained as source strings.

## First-pass structural observations

1. Stage 0 is a decision and research gate, not a device-execution stage. The source explicitly says not to touch the device before user confirmation.
2. Stage 1 establishes a reproducible baseline so failures in later new code can be attributed more reliably.
3. Stage 2 establishes recovery capability and captures stock-device evidence before persistent writes.
4. Stage 3 is a minimal boot-and-access milestone: kernel in RAM, host shell access and automatic return to the original system.
5. Stage 4 combines user-visible acceptance with hardware metrics and begins native display takeover work even if a borrowed display is used first.
6. Stage 5 begins once the RAM desktop works. It does not wait for all peripheral adaptation to finish.
7. Stage 6 has an explicit per-peripheral loop: inspect experience, research reuse, compare with stock evidence, isolate one peripheral in a RAM image, test on hardware, integrate, cold-boot regress, add self-checks and record temporary work as debt.
8. Stage 7 treats clean installation from public documentation as a test of the documentation itself. Missing or incorrect steps are findings, not user error.
9. The first-hour audit is a distinct user-experience check after installation, covering language, keyboard, timezone, scaling, networking, browser, sound, brightness, power button, hostname and APT.
10. The release step requires user confirmation before pushing and publishing.
11. Stages 6 and 7 form a versioned loop, followed by cleanup before the next version begins.
12. Stage 8 lists maintenance activities but does not define a terminal state or completion criterion. That absence is preserved.

## Relationships to other source documents

- `SKILL.md` gives the nine-stage overview and directs the reader to this detailed process.
- `discipline.md` governs records, workspace, provenance, technical debt, user collaboration and handoff.
- `decisions.md` supplies conditional technical choices used during scoping and execution.
- `build-environment.md` expands Stage 1 and informs repeated build work.
- `verification.md` supplies evidence standards, automation and acceptance criteria across stages.
- `ubuntu.md` informs persistent installation, packaging, boot integration and release.
- The `experience/` library provides domain-specific routes and prior failure knowledge at the points where the process calls for it.

The source uses direct links and shared concepts to connect these documents; it does not define a single complete dependency graph.

## Ambiguities and items to preserve for later review

- Stage 0 and Stage 1 overlap, but the exact activity boundary is not specified. Do not invent one.
- Native display takeover starts in parallel with other Stage 4 work, but the source does not enumerate every task that may overlap.
- Stage 6 offline preparation can be parallel, while real-device testing is serial by peripheral. These are different concurrency scopes.
- Stage 5 begins once the RAM desktop works, not after all Stage 6 work. The process does not specify a universal point at which every Stage 5 activity must finish before any Stage 6 activity begins.
- Stage 8 maintenance activities are listed without a formal completion criterion. Do not infer that maintenance is either a formally perpetual state or a terminal stage from this document alone.
- “Reference answer” language is retained as a comparison concept; it does not establish that stock behaviour is always the correct product outcome.
- The translation does not technically validate the commands, kernel interfaces, device procedures or acceptance thresholds.

## Review boundary and status

- Structural fidelity: nine stages, ordering, goals, actions, outputs, completion criteria and related-experience links retained.
- Concurrency: Stage 0/1 overlap, Stage 4 native takeover, Stage 6 offline preparation and one-at-a-time hardware testing, Stage 5/6 relationship and version loop preserved.
- Terminology: stage names and recurring technical language aligned with the glossary and other translations.
- Source ambiguity: exact overlap boundaries and Stage 8 exit criterion remain unspecified.
- Technical correctness: **not reviewed or validated**.
- Current status: **translated**. Semantic review and technical validation remain outstanding.

This retrospective record preserves the reconstruction rationale for the already committed process translation. The next review should compare the English text against the Chinese source and assess its fit in the whole-skill context; technical procedure validation remains a separate pass.
