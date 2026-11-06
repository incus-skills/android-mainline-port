# Pass 3 — Checkpoint 08: `references/ubuntu.md` Semantic Review

## 1. Position

- Pass: **Pass 3 — technical and semantic review of the operational English translation set**
- Document: `references/ubuntu.md`
- Source branch: `main`
- Source blob SHA: `2f87d2512753af379615e05a7d15ea773fbafc0f`
- English translation: `translation/en/references/ubuntu.md`
- Translation review record: `translation/reviews/references/ubuntu.md.md`
- Previous document: `references/verification.md`
- Next document: `experience/README.md`
- Technical correctness validation: not performed.

## 2. Review result

The source and English translation were compared section by section. One narrow fidelity correction was made in the release-attachments list: `软件包包` is rendered as **software-package bundle**, preserving the source's reference to a bundled release artifact rather than flattening it to the plural “software packages”. No other source-driven semantic correction was required.

The review confirmed preservation of:
- the Ubuntu 26.04 LTS + GNOME baseline and the qualification that layer/package granularity and naming should follow the actual project;
- the four project-controlled layers and the separate official distribution root-filesystem base;
- layer-specific change responsibilities and the minimum separation of kernel, firmware and desktop policy;
- package names, package contents, optional consolidation/removal conditions and ownership/update-frequency rationale;
- ABI increment/no-reuse rules, ledger contents, version examples and kernel packaging constraints;
- boot-image hook locations, optional active-slot write/readback, rollback retention, missing-prerequisite exit-0 behaviour and build-chroot protection;
- dracut configuration, early-boot driver/firmware requirements, hostonly implications and the stated alternative;
- the six root-filesystem composition steps, kernel-configuration list, snap GPU-provider distinction, GNOME defaults, product debugging-channel conditions and release/upgrade sequence;
- the cross-distribution adaptation table and the conclusion about which layers remain stable or require rewriting.

Technical identifiers, package names, paths, commands, configuration keys, interface names, version labels and examples were retained. The source's layered ASCII diagram remains a text diagram; no new dependency graph or runtime flow was inferred.

## 3. Distinctions and boundaries preserved

- Ubuntu 26.04 LTS + GNOME remains the stated experience baseline, not a universal requirement for every port.
- The official distribution root filesystem remains distinct from the four project-controlled layers.
- Hardware and device layers are not treated as identical: one is stated unchanged, the other mostly unchanged.
- Package names and package boundaries remain adjustable to project circumstances, except for the explicit minimum separation of kernel, firmware and desktop policy.
- Optional package consolidation and distribution-version-dependent removal conditions are not turned into mandatory package rules.
- Missing boot-hook prerequisites retain the explicit successful exit and no-write/no-delete behaviour.
- The source's layered drawing is not treated as a formal dependency graph or runtime flow.
- The cross-distribution table remains a high-level adaptation map, not a complete procedure.

## 4. Pass 3 disposition

- Source-based semantic review: **completed**.
- Translation correction: **software packages** → **software-package bundle** in the release-attachments list.
- Technical correctness, current package/hook behaviour, distribution compatibility and real-device behaviour: **not validated**.
- Whole-skill validation: **outstanding**.
- Next document: `experience/README.md`.

The source's Ubuntu-specific baseline remains scoped as stated. Technical claims require a separate evidence-based review and must not be treated as validated by this comparison.
