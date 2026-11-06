# Reconstruction and Translation Record: references/build-environment.md

## Source and provenance

- Source path: `references/build-environment.md`
- Source ref: `main`
- Source blob SHA: `7cc91c8beda502217a4b43a5ca60550eaa608a1e`
- English artifact: `translation/en/references/build-environment.md`
- Translation branch: `translation/english-reconstruction`
- Translation commits: initial translation `86be4ff72effc0777a5f7b4ebd4fcc418f970962`; timing clarification `ac2054e0a1370159637601e038d02ffd98464c80`

## Translation intent

This is a functional English translation of the source's build-environment and efficiency guidance. It is not an optimisation plan for this repository, a recommendation to adopt these settings elsewhere, or a technical validation of the commands and performance claims.

The first-pass objective is to learn how the skill structures build infrastructure, time and resource management, reproducibility, parallel work and cleanup. The truth or current applicability of individual claims is reserved for a separate technical-evidence pass.

Preserve:
- the stated goal and “measure first, then optimise” principle;
- the ten-section organisation and ordering;
- the recommendations, examples, thresholds and stated exceptions;
- the distinction between routine iteration and release-time clean/reproducible work;
- the safety and provenance constraints around credentials, OUT directories, worktrees, devices and evidence;
- technical tokens, commands, paths, compiler flags, tool names and numerical examples.

Do not silently modernise tool choices, change numerical guidance, or turn context-dependent recommendations into universal requirements.

## Reconstructed document role

This document describes the infrastructure and efficiency conditions for developing the port. It is not a project-stage procedure. It establishes how to make repeated development work fast without sacrificing reproducibility, traceability or safe resource ownership.

Its organisation is:

1. **Host** — operating system, compute, disk and root-permission requirements; credential handling.
2. **Toolchain** — compiler, kernel dependencies, image tools, device tools and QEMU.
3. **ccache** — capacity estimates, configuration, invocation and CI caching.
4. **Kernel OUT directories** — persistent per-configuration outputs, locking, input identity and targeted builds.
5. **Root-filesystem builds** — method comparison, package ordering, initramfs cost and device-side iteration.
6. **Other large components** — Mesa and non-kernel userspace caching.
7. **Reference repositories** — separation, read-only use, provenance and clone strategy.
8. **Disk and cleanup** — thresholds and the distinction between disposable and protected artifacts.
9. **Parallelism** — shared cache/lock, isolated agent worktrees and single device operator.
10. **Unnecessary overhead** — work the source says can be omitted or deferred.

The opening principle is a feedback rule for the build pipeline: measure slow steps when first encountered, cache unchanged inputs, budget each phase and review after releases. The document therefore contains both setup guidance and ongoing operational constraints.

## Terminology and translation decisions

- `构建环境与效率` → **Build Environment and Efficiency**.
- `主机` → **Host**. Refers to the build host, not the Android device.
- `根文件系统` → **root filesystem**.
- `根权限` / `真正的 root` → **root privileges** / **actual root**. The source contrasts actual root with an unprivileged user namespace.
- `OUT` → **OUT directory**. Retained as the kernel build-output convention and technical token.
- `冷构建` → **cold build**. Kept distinct from a clean build at release time.
- `命中率` → **cache hit rate**.
- `直接模式 manifest` → **direct-mode manifest**.
- `输入哈希` → **input hash**.
- `收据` → **receipt**. Retained as the source's record/reference term; its exact repository-wide schema is not defined in this document.
- `参考仓库` → **reference repositories**. This preserves their role as read-only sources for comparison/reuse, not as project working trees.
- `上机` → **on-device** or **real-device** testing according to context.
- `发布物` → **release artifacts**.
- `十几分钟` → **around a dozen minutes**. This preserves the approximate 10–19-minute range implied by the Chinese expression, rather than broadening it to “tens of minutes”.

Commands, paths, compiler flags, environment variables, tool names and numeric values are retained as source tokens.

## First-pass structural observations

1. The document treats efficiency as an engineering discipline, not merely faster compilation. Measurement, cache identity, persistent output directories, time budgets and release review are linked.
2. It distinguishes routine incremental work from clean or complete release work. A fresh path is not automatically a better or cleaner routine build; it can force a cold build.
3. Reproducibility and cache effectiveness are connected through stable build metadata and path mapping. The source does not treat cache hits as independent of build reproducibility.
4. OUT directories are stateful artifacts with identity and provenance. Once an OUT has been identified or referenced by a test/release, it is protected from further writes.
5. Build methods are differentiated by purpose: native ARM64 is preferred; qemu-user chroot is a fallback; full-system TCG is restricted to tests of init/systemd behaviour.
6. Device-side package installation and initramfs generation are presented as the fast routine-iteration path; complete root-filesystem builds are reserved for release.
7. Reference repositories have a distinct role and trust boundary: separate, recorded and read-only, with no dependency on their absolute paths or build outputs.
8. Cleanup rules are selective rather than “delete all generated files”: provenance-bearing OUTs, release references, unmerged worktrees and private evidence are explicitly protected.
9. Parallelism is bounded by shared resources. Kernel builds are serialised around shared ccache; agents are isolated by worktree/branch and do not share OUTs or devices; the device has one operator.
10. The final section identifies avoidable work and deliberately rejects QEMU as a basis for conclusions about real hardware timing.

## Relationships to other source documents

- `references/process.md` describes the project stages; this document supplies build-environment constraints used especially during build preparation and repeated development/release.
- `references/decisions.md` includes build-environment and efficiency decision topics and points here for details.
- `references/verification.md` supplies reproducibility and test-evidence concerns; this document describes how to structure build inputs and outputs so those concerns can be addressed.
- `references/discipline.md` governs records, worktrees, attribution, sub-agent work and device ownership; this document gives build-specific constraints that complement those controls.
- `experience/usb-and-storage.md` is directly referenced for root-filesystem delivery.
- The document also discusses Mesa and userspace components, connecting it to GPU and Ubuntu integration experience.

These are relationships indicated by source references and shared operational subjects; no complete dependency graph is defined here.

## Ambiguities and items to preserve for later review

- The approximate capacity and performance figures for ccache and build methods are empirical guidance from the source. They should not be treated as guarantees for different hardware, compiler versions or source trees.
- The source's phrase `收据` (“receipt”) is not defined here as a formal record format. Translation preserves the reference without inventing a schema.
- The source says a full-system-emulation root build takes more than 40 minutes and native ARM64 takes “十几分钟”; these are approximate comparisons, not benchmarks with a specified host configuration.
- The exact meaning of “the same source built from different paths” is explained through the ccache manifest discussion, but the source does not give a formal cache-key model.
- The cleanup instruction to move identity records outside a directory before deleting it is retained; the document does not specify a particular identity-record format.
- Tool versions and current distribution packaging may change. Translation does not update or validate them.

## Review boundary and status

- Structural fidelity: ten sections, tables, code blocks, examples and ordering retained.
- Operational distinctions: routine iteration/release build, cache/output identity, protected artifacts and bounded parallelism preserved.
- Technical tokens: commands, flags, paths, environment variables, tools and numeric examples retained.
- Source ambiguity: no receipt schema, cache-key model or new validation requirements invented.
- Technical correctness and current applicability: **not reviewed or validated**.
- Current status: **translated**. Semantic review and technical validation remain outstanding.

The next review should compare the English text with the Chinese source and assess its relationship to the other framework references. Build-time claims, ccache behaviour, toolchain compatibility and command correctness require a separate technical review.
