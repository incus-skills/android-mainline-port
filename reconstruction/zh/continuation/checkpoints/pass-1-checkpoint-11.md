# Pass 1 — Checkpoint 11: Build Pipeline and Release

## 1. Position and scope

- Traversal: **Pass 1 — initial structural and linguistic reconstruction**
- Previous checkpoint: [Pass 1 / Checkpoint 10](pass-1-checkpoint-10.md)
- Completed source: `experience/build-and-release.md`
- Source branch: `main`
- Source blob SHA: `05c40c2b974cd14f229db814bc1ef63b1e657c65`
- Build-environment reference: `references/build-environment.md`
- English translation: `translation/en/experience/build-and-release.md`
- Translation review: `translation/reviews/experience/build-and-release.md.md`
- Chinese descriptive reconstruction: `reconstruction/zh/build-and-release.md`
- Next source: `experience/testing-and-process.md`
- Technical validation: not performed

Pass 1 remains the initial structural and linguistic reconstruction. This checkpoint does not validate the technical claims, performance measurements or release practices.

## 2. Reconstructed document role

The source explicitly delegates complete build-environment setup to `references/build-environment.md` and says this file records concrete pitfalls only.

It has no general recommended route, completion criteria or dedicated unresolved section. It records pitfalls across measurement, caching, execution environments, artifact identity, reproducibility, cleanup, release packaging, package versioning, incremental builds, host saturation, version cadence and external documentation.

## 3. Main reconstructed groups

### Measurement and optimization

A root-filesystem build exceeding 40 minutes was optimized without measurement. Stage timing showed that dracut under full-system QEMU and generic-kernel purge dominated, not distribution package installation. The rule is to time every step and split any first-time step exceeding five minutes.

Other entries warn against caching before measuring, changing OUT paths and invalidating ccache, undersizing ccache, attempting ineffective dracut compression changes, and ignoring host resource saturation.

### Execution environment

The source distinguishes native ARM64, root-privileged qemu-user chroot and full-system QEMU/TCG. Full-system emulation is reserved for init-behaviour tests; multiple full-system VMs should not run concurrently.

An unprivileged user namespace can produce incorrect ownership and omit device nodes. Mesa's NIR Python generator is extremely slow under qemu-user; cross-compilation or native ARM64 is preferred.

### Cache and build-output identity

The source distinguishes stable OUT paths per configuration, ccache base directory, single-writer locking, recorded build-input identity, source-HEAD verification and immutable identity-recorded OUT directories.

A recorded OUT or worktree with commits is not disposable cache. Release-referenced files and receipts also constrain cleanup.

### Reproducibility

The source lists debug paths, missing `KAFLAGS` mapping, umask, unpinned APT indexes, host `dtc` versions and random AVB salt as causes of different outputs from the same source.

Controls include mapping paths in both `KCFLAGS` and `KAFLAGS`, fixing build timestamp/user/host, normalizing permissions and timestamps, deterministic tar ordering and fixed AVB salt. The translation does not claim this list is exhaustive or sufficient for all artifact types.

### Release integrity and external documentation

Cases include GitHub's 2 GB per-attachment limit, artifact grouping, commit authorship, existing-fork rename behaviour, GPL source distribution, trademark wording, package version ordering, published-tag immutability and cleanup against release manifests/receipts.

External documentation should explain capabilities, support, installation and verification; retain safety warnings; and avoid internal investigation detail and technical-debt identifiers.

### Incremental work and version cadence

The Mesa case concerns build-directory deletion and a resume option that rejects directories already in packaging. The proposed approach retains directories by source/patch hashes and supports incremental rebuilds.

The version rhythm is scope/acceptance → development → test → release → cleanup → next port. It is a described working rhythm, not a formally defined state machine.

## 4. Explicit cross-document relationships

- The source explicitly points to `../references/build-environment.md` for complete build-environment setup.
- `experience/README.md` classifies the file as build pipeline, caching, reproducibility and release.
- `patterns.md` explicitly lists it under **Pattern D**, residual state that distorts results. The examples include ccache invalidation after changing paths, old OUT/build state and package-version ordering.

The source itself does not link to `patterns.md`; this is a reverse reference from the pattern index.

## 5. Observer-reconstructed correspondences

- **Measurement before optimization:** performance claims are based on timed stages rather than assumed bottlenecks.
- **Execution environment as a variable:** native ARM64, qemu-user and full-system QEMU are distinct contexts.
- **Cache identity versus artifact identity:** reusable cache and identity-recorded build outputs have different preservation rules.
- **Path as a build input:** paths affect cache keys and embedded debug metadata, influencing speed and reproducibility.
- **Reproducibility as multiple controlled inputs:** source revision alone does not establish identical outputs; toolchain, indexes, paths, timestamps, permissions and random salt also matter.
- **Artifact provenance and release integrity:** source HEAD, build-input identity, manifest, receipts, hashes and published tags have distinct roles.
- **Internal evidence versus public interface:** internal technical-debt/root-cause records differ from user-facing release documentation.
- **Version boundaries as work control:** version cadence limits unreleased candidates and delays new features until release.

These are observer reconstructions, not a source-defined universal pipeline model.

## 6. Unresolved and underspecified boundaries

The source has no dedicated unresolved section. It does not establish:
- actual build scripts, CI definitions, cache keys or artifact manifest formats;
- whether listed reproducibility controls suffice for every artifact type;
- a detailed protocol for recording/verifying build-input identity;
- exact safe-resume and safe-deletion conditions for build directories;
- general release correction, rollback or artifact-signing procedures;
- formal entry/exit criteria for each version phase.

The build-environment reference provides general guidance, but project-specific implementation and the reported performance/release behaviour have not been examined or validated in this pass.

## 7. Bounded sufficiency

For initial structural and linguistic reconstruction, the document's scope, pitfall groups, explicit build-environment reference, reverse Pattern D reference, and main distinctions around measurement, execution context, cache/artifact identity, reproducibility, release integrity and external documentation have been recovered.

No technical claims have been validated. Further investigation would require actual build scripts and CI workflows, cache/manifest definitions, artifact receipts, release records or targeted measurements.

## 8. Position change

Pass 1 remains unchanged in purpose and scope. The document position advances from `experience/ubuntu-userspace.md` (Checkpoint 10) to `experience/build-and-release.md` (Checkpoint 11).

The next source in `experience/README.md` is `experience/testing-and-process.md`.

Pass number, document position and session checkpoint number remain separate coordinates.

## 9. Re-entry sequence

1. Read `reconstruction/zh/continuation/current-position.md`.
2. Read this checkpoint, then Checkpoint 10 and earlier checkpoints as needed.
3. Read `reconstruction/zh/README.md` and `translation/INVENTORY.md`.
4. Revisit `references/build-environment.md`, `experience/patterns.md` (Pattern D), and relevant earlier experience documents as needed.
5. Independently inspect `experience/testing-and-process.md`; distinguish its own claims from expectations formed by previous documents.
