# Translation Review: Build Pipeline and Release

## Source and scope

- Source: `experience/build-and-release.md`
- Source branch: `main`
- Source blob SHA: `05c40c2b974cd14f229db814bc1ef63b1e657c65`
- Architectural/build reference: `references/build-environment.md` (source blob SHA `7cc91c8beda502217a4b43a5ca60550eaa608a1e`)
- Collection index: `experience/README.md`
- Pattern index: `experience/patterns.md`
- Traversal: Pass 1 — initial structural and linguistic reconstruction
- Technical validation: not performed

## Document role and structure

The source explicitly delegates complete build-environment setup to `references/build-environment.md` and says this file records concrete pitfalls only. It contains no general recommended route, no completion criteria and no dedicated unresolved section.

Its entries range from build-time measurement and caching through execution environments, artifact identity, reproducibility, cleanup, release packaging, package versioning, incremental builds, host saturation, version cadence and user-facing documentation.

The translation preserves this as a collection of cases rather than turning it into a build procedure or a formal release lifecycle.

## Translation decisions and terminology

- “构建流水线与发布” is “Build Pipeline and Release”.
- “这里只记具体的坑” is retained as an explicit scope boundary: this document records specific pitfalls only.
- `OUT` remains uppercase as the kernel build output directory concept; it is not translated as a generic output folder.
- “全系统模拟” is “full-system emulation”; “qemu-user chroot” is retained as a distinct execution mode.
- “原生 ARM64” is “native ARM64”; the source's relative performance observations are retained without independent benchmarking.
- “用户命名空间” is “user namespace”; ownership, UID/GID mapping, `newuidmap` and `mknod` are kept explicit.
- “可复现性” is “reproducibility”; path mapping, fixed build identity, normalized metadata, pinned inputs and deterministic archive ordering remain distinct contributing controls.
- “构建输入身份” is “build-input identity”; it is not collapsed into source revision alone.
- “身份记录” is “identity record”; “收据” is “receipt”, preserving their separate roles in cleanup control.
- “发布清单” is “release manifest”.
- “发布标签” is “published tag”; the immutability instruction is preserved.
- “分卷” is “volumes”; the example size is retained as stated.
- “版本推进” is “version-by-version progression”; it is not promoted to a formal release state machine.
- “对外文档” is “external documentation”, contrasted with internal investigation records.
- Commands, environment variables, paths, package names, tool names and numeric values are preserved.

## Reconstructed content

### Measurement and optimization

The opening case describes a 40-minute-plus root-filesystem build optimized blindly. Measurement identified QEMU full-system dracut and generic-kernel purge as the large stages, rather than distribution package installation. The rule is to time every step and split any first-time step exceeding five minutes.

Several other cases reinforce measurement before optimization:
- changing OUT paths invalidates ccache because paths enter compiler arguments/cache keys;
- undersized ccache and path-varying manifests cause repeated cleanup and poor hit rates;
- caching the distribution base layer can make builds slower when it is not the bottleneck;
- lowering dracut compression may have no effect because Ubuntu maps the requested zstd levels together;
- host saturation, storage fullness, parallel builds and Wi-Fi uploads can dominate apparent build performance.

The translation retains reported durations and ratios as source observations, not independently verified benchmarks.

### Execution environment and architecture

The source distinguishes native ARM64, root-privileged qemu-user chroot and full-system QEMU/TCG. It recommends reserving full-system emulation for tests requiring init behaviour and avoiding parallel full-system VMs.

The unprivileged user-namespace case is not merely a speed issue: UID/GID ownership and device-node creation become incorrect. The source's proposed alternatives are real root with narrowly restricted passwordless sudo or native CI.

Mesa is a separate extreme case: qemu-user makes the NIR Python generator take more than 100 minutes versus tens of seconds natively. Cross-compilation or native ARM64 is recommended.

### Cache and build-output identity

The OUT/ccache cases distinguish:
- stable output paths per configuration;
- ccache `base_dir` and manifest growth;
- one writer per OUT, enforced with `flock`;
- recorded build-input identity;
- refusing a build if the current source HEAD does not match;
- never rewriting an OUT whose identity has already been recorded.

The source also requires build inputs and output identity to remain distinguishable from an ordinary cache. A worktree with commits or an OUT referenced by receipts cannot be casually deleted.

### Reproducibility

The source attributes different results from identical source to several independent inputs:
- debug source paths differ between ccache and direct GCC;
- `KAFLAGS` path mapping is missing;
- umask changes directory permissions;
- APT indexes are not pinned;
- host `dtc` versions differ;
- AVB salt is randomly generated.

Its listed controls include path mapping in both `KCFLAGS` and `KAFLAGS`, fixed kernel build timestamp/user/host, normalized permissions and timestamps, deterministic tar ordering, and fixed AVB salt.

These are source-listed controls; the translation does not claim that the list is exhaustive or that applying it guarantees bit-for-bit reproducibility.

### Release integrity and distribution

The release entries cover:
- the 2 GB per-attachment GitHub limit and splitting root filesystems into volumes;
- user-oriented release notes and a small number of grouped artifacts;
- choosing commit authorship before release;
- a `gh repo fork` side effect when a fork already exists;
- GPL source distribution and unofficial trademark wording;
- package version ordering relative to internal test builds;
- immutability of published tags;
- checking release manifests and receipts before cleanup;
- preserving identity records outside deletion targets.

The source also distinguishes public documentation from internal investigation. External documentation should explain capabilities, limitations, installation and verification, retain safety warnings, and avoid exposing internal investigation details or technical-debt identifiers.

### Incremental work and version cadence

The Mesa case records a build entry point that deletes old output and a resume option that refuses directories already in packaging. The stated remedy is to retain build directories by source/patch hashes and support incremental rebuilds.

The final cadence entry describes a repeated sequence for each version: define scope and acceptance, develop, test, release, clean branches/worktrees/artifacts, then begin the next port. Unreleased candidates should not accumulate and new features wait until the current version is released.

This is a source-described working rhythm, not a formally specified state machine.

## Explicit cross-document relationships

- The source explicitly points to `../references/build-environment.md` for complete build-environment setup.
- `experience/README.md` classifies the file as covering build pipelines, caching, reproducibility and release.
- `patterns.md` explicitly lists `build-and-release.md` under **Pattern D**, residual state that distorts results. Its example is ccache invalidation after changing paths; the same entry also groups old OUT/build state and package-version ordering among residual-state manifestations.

The source body does not itself link to `patterns.md`; this is a reverse reference from the pattern index.

## Observer-reconstructed correspondences

- **Measurement before optimization:** performance claims are tied to measured stages rather than assumptions about which layer is expensive.
- **Execution environment as a variable:** native ARM64, qemu-user and full-system QEMU are not interchangeable test/build contexts.
- **Cache identity versus artifact identity:** caches may be disposable/reusable according to inputs, while recorded OUT directories and release-referenced files have preservation constraints.
- **Path as a build input:** paths can influence cache keys and embedded debug metadata, so directory layout affects both speed and reproducibility.
- **Reproducibility as multiple controlled inputs:** source revision alone does not establish identical outputs; toolchain, indexes, paths, timestamps, permissions and random salts also matter.
- **Artifact provenance and release integrity:** source HEAD, build-input identity, release manifest, receipts, hashes and published tags serve different roles.
- **Internal evidence versus public interface:** technical debt and root-cause analysis belong in internal records; release documentation serves user decisions and safe operation.
- **Version boundaries as work control:** the described version cadence limits unreleased candidates and postpones new features until release completion.

These are observational correspondences, not a source-defined universal pipeline model.

## Unresolved and underspecified boundaries

The source has no dedicated unresolved section. The following boundaries remain unestablished by this document:
- exact build scripts, CI definitions, cache keys and artifact manifest formats;
- whether all reproducibility controls are sufficient for every artifact type;
- the detailed protocol for recording, verifying and retaining build-input identity;
- the precise conditions under which a build directory can be safely resumed or deleted;
- release correction, rollback and artifact-signing procedures beyond the specific cases listed;
- formal entry/exit criteria for each version phase.

The references document supplies general environment guidance, but this pass does not inspect project-specific implementation or validate the reported performance and release behaviours.

## Bounded sufficiency for Pass 1

For initial structural and linguistic reconstruction, the document's scope, pitfall groupings, explicit build-environment reference, reverse Pattern D reference, and main distinctions around measurement, execution context, cache/artifact identity, reproducibility, release integrity and public documentation have been recovered.

No technical claims have been validated. Further investigation would require actual build scripts and CI workflows, cache and manifest definitions, artifact receipts, release records, or targeted measurements.

## Position change

Pass 1 remains the same declared traversal. The document position advances from `experience/ubuntu-userspace.md` (Checkpoint 10) to `experience/build-and-release.md` (Checkpoint 11).

The next source in `experience/README.md` is `experience/testing-and-process.md`.

Pass number, document position and session checkpoint number remain separate coordinates.


---

## Pass 3 — Technical and semantic review

### Evidence baseline

- Chinese source: `experience/build-and-release.md`, `main`, blob SHA `05c40c2b974cd14f229db814bc1ef63b1e657c65`.
- English target: `translation/en/experience/build-and-release.md`, `translation/english-reconstruction`, blob SHA before review `411a9d962428bff35f6a8c66116ce789290028e2`.
- Pass 1 review preserved above; its blob SHA before this append was `6d3352f43645d60f4e2329fd53b88e488c935d35`.
- Governing criteria, current position/checkpoint, completed preceding task, glossary, translation instructions, segmentation record, both experience indexes, and the named build-environment reference/counterpart were fetched at the exact revisions specified in Task 006. All fourteen task evidence-baseline blob SHAs matched.
- External technical correctness, current upstream behaviour, benchmarking, build-infrastructure inspection and release-operation validation were not performed.

### Observer position and scope

This is the Pass 3 source-to-English semantic and technical translation review of the complete build-pipeline and release experience document. The question is whether the English preserves the Chinese source's operational meaning and force across the seven declared review dimensions. It is not a build audit, reproducibility assessment, release redesign or technical validation.

The source explicitly delegates complete build-environment setup to `../references/build-environment.md` and limits this file to specific pitfalls. The source and English reference files were inspected only to verify the named reference/counterpart and the boundary; their technical contents were not reviewed.

### Observations

- **Imperative force:** requirements, recommendations, prohibitions, cautions and examples remain distinguishable. In particular, measurement precedes optimization; native ARM64 is preferred, root-privileged qemu-user chroot is the next option, and full-system emulation is reserved for tests that need init behaviour. The restrictions on concurrent full-system VMs, writes to identity-recorded OUT directories, cleanup, published tags and release documentation are retained.
- **Conditional scope:** applicability labels and qualifiers (including General, Kernel builds, Ubuntu 26.04, Mesa, Debian packaging, GitHub and the 2026 qualifiers) are retained. The source's reported performance figures remain presented as observations, not independently verified benchmarks.
- **Decision logic:** alternatives and their stated conditions are preserved, including native ARM64 / qemu-user / full-system QEMU; real root / native CI; cross-compilation / native ARM64; cleanup checks; release artifact grouping; and version-by-version progression. The progression remains a working sequence, not a formal state machine.
- **Technical tokens:** commands, paths, identifiers, environment variables, package/tool names, values and units were compared and retained, including `OUT`, `ccache -s`, `ccache --show-compression`, `base_dir`, `unshare -r`, `newuidmap`, `mknod`, `flock`, `KCFLAGS`, `KAFLAGS`, `KBUILD_BUILD_TIMESTAMP/USER/HOST`, `git describe --match 'v[0-9]*.[0-9]*'`, `gh repo fork`, `~dev`, `--resume` and `tar --sort=name`.
- **Structure:** the document remains a collection of discrete pitfalls with symptom/cause/resolution/applicability distinctions where supplied. No general pipeline, cache policy, reproducibility guarantee or release lifecycle has been added.
- **Cross-document references:** the explicit build-environment reference and its English counterpart resolve to the corresponding files. Both experience indexes classify this document as build pipeline, caching, reproducibility and release material, and their order places it after Ubuntu userspace. This is document-order evidence, not a technical dependency. The Pass 1 record's Pattern D reverse-reference and observer-reconstructed correspondences remain provenance observations; they are not presented as source-body links or source-defined universal relationships.
- **Uncertainty:** the source has no dedicated unresolved section. The translation does not invent one or resolve omissions. Reported timings, tool behaviour, release-platform limits and other technical claims remain source claims whose external correctness is outside this pass.

Particular distinctions were checked: stage measurement versus bottleneck assumptions; stable OUT paths, ccache base directory/manifests and artifact identity; native ARM64 versus root-privileged qemu-user versus full-system QEMU emulation; UID/GID mapping and device-node privileges; dracut compression and Mesa NIR generation; single-writer OUT discipline and source HEAD/build-input identity; independent reproducibility inputs; manifest/receipt/source-archive/package/hash/cleanup roles; release attachment limits and artifact grouping; fork behaviour, GPL source and trademark wording; package version ordering and `~dev`; immutable published tags; sparse image preparation; Mesa incremental rebuild identity; host saturation; version cadence; and public documentation versus internal investigation.

### Decisions

No material, source-justified translation correction was identified. The English target is left unchanged.

The target's phrasing is sometimes idiomatic rather than structurally literal, but the compared passages preserve the source's distinctions and operational force. No style-only rewrite is warranted. In particular, no claim has been strengthened into a guarantee, no source omission has been silently resolved, and no observer-reconstructed relationship has been promoted to a source-explicit dependency.

### Unresolved matters

The source does not specify exact build scripts, CI definitions, cache-key formats, artifact-manifest/receipt schemas, a complete identity-record protocol, general safe-resume/deletion criteria, or formal entry/exit criteria for version phases. These omissions are preserved, not filled in.

Whether the reported QEMU, dracut, Mesa, ccache, GitHub Release, `gh repo fork`, APT and packaging behaviours remain technically accurate is not established here. Current infrastructure, upstream behaviour, benchmark results, release operations, signatures and reproducibility guarantees remain unvalidated and outside this task.

### Validation and result

- Compared the complete Chinese source and complete English target at the exact stated revisions.
- Verified all fourteen evidence-baseline revisions specified by Task 006; all blob SHAs matched.
- Checked the explicit build-environment reference and its English counterpart, plus both experience-library indexes.
- Confirmed the target requires no correction; therefore its blob remains `411a9d962428bff35f6a8c66116ce789290028e2`.
- Confirmed the Chinese source remains unchanged at `05c40c2b974cd14f229db814bc1ef63b1e657c65`.
- This review append preserves the complete Pass 1 record above; no Pass 1 content was intentionally changed.
- External technical validation and real-device validation were not performed.

### Change boundary

Only this Pass 3 review section is appended to this review record. The English target, Chinese source, build-environment references, criteria, continuation records, glossary, indexes and unrelated files are not modified by the review decision.

### Next dependency

No additional document is required to complete this bounded translation comparison. The explicit build-environment reference boundary and counterpart have been checked. Any external technical question would require a separately scoped operation and must not be inferred from this translation review.

**Worker result: COMPLETED.** The target is unchanged because no material source-supported correction was warranted. This is a translation-review result only, not external technical acceptance.


### Corrective record — bounded accuracy repair after independent rejection

An independent reconstruction rejected acceptance because this review contained two unsupported technical details. This correction repairs only the review record; it does not change the completed worker's original execution status or the English translation.

- Removed `LOCALVERSION_AUTO` from the technical-token inventory. It is not present in the Chinese source or English target and therefore must not be reported as compared and retained.
- Replaced `full-system QEMU/TCG` with `full-system QEMU emulation`. The source/target pair distinguishes full-system QEMU from qemu-user but does not specify TCG; the review must not add that implementation detail.

The corrected review was re-read in full. The technical-token list no longer contains `LOCALVERSION_AUTO`, and the execution-mode description no longer contains `TCG`. The source, target and original Pass 1 review content remain unchanged. No translation correction is introduced.

**Correction boundary:** only the two unsupported phrases in this Pass 3 review record were changed. No other review conclusions, scope statements or claims were revised.

**Worker corrective status:** completed; original Task 006 worker status remains COMPLETED. Independent orchestrator acceptance remains pending.
