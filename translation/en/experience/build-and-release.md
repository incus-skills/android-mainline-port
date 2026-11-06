# Build Pipeline and Release

The complete build-environment setup is described in `../references/build-environment.md`. This document records specific pitfalls only.

## Pitfalls

### Builds Are Slow, but No Stage Has Ever Been Timed

Tags: **[Frequent] [Time-consuming]**

- **Symptom:** each root-filesystem build takes more than 40 minutes; trying several “speed-up” methods makes no difference.
- **Cause:** guessing at the bottleneck without measurement. Stage-by-stage timing showed that the largest costs were dracut under full-system QEMU emulation (about 15 minutes) and purging the generic kernel, not installing distribution packages.
- **Resolution:** record the duration of every step. The first time a step takes more than five minutes, split it into timed sub-stages. Choose optimizations from measured results.
- **Applicability:** General.

### Changing the OUT Directory Invalidates the Entire ccache

- **Symptom:** an incremental build that should take one minute becomes a 40-minute cold build.
- **Cause:** a new OUT path was created for the sake of a “clean” build; the path is included in compiler arguments and cache keys.
- **Resolution:** keep one long-lived OUT directory per configuration; set ccache `base_dir`; do one clean build before an official release.
- **Applicability:** Kernel builds; General.

### ccache Capacity Is Too Small

- **Symptom:** `ccache -s` reports a full cache, more than a thousand cleanups, and a hit rate below half.
- **Cause:** the limit is only 4 GB and `base_dir` is unset. Building under a new path each time appends manifest entries (measured at more than 60% of cache usage).
- **Resolution:** use a 10 GB limit (sufficient for kernel-only work, about 30 complete builds); set `base_dir` and keep the OUT path stable; monitor the hit rate periodically. `ccache --show-compression` and per-suffix statistics (`M` for manifests, `R` for results) reveal where space is used.
- **Applicability:** General.

### Building an ARM64 Root Filesystem with Full-System QEMU Emulation on an x86 Host

- **Symptom:** package installation takes more than 30 minutes; running two VMs at once doubles the elapsed time.
- **Resolution:** prefer native ARM64 (for example, an arm64 GitHub Actions runner; the same build takes around ten-plus minutes); next use a root-privileged qemu-user chroot (about twice as fast); reserve full-system emulation for tests that must verify init behaviour. Do not run multiple full-system VMs in parallel.
- **Applicability:** General; 2026.

### An Unprivileged User-Namespace chroot Cannot Produce a Correct Root Filesystem

- **Symptom:** ownership of `/etc/shadow` changes from `0:42` to `0:0`; `mknod` fails and initramfs lacks device nodes.
- **Cause:** `unshare -r` maps only one UID/GID; without `newuidmap`, multiple IDs cannot be mapped; `mknod` is also unavailable in the namespace.
- **Resolution:** use real root (give the build host passwordless sudo restricted to a fixed script), or use native CI.
- **Applicability:** General.

### Lowering the dracut Compression Level Has No Effect

- **Cause:** Ubuntu's dracut uses 3cpio packaging; `zstd -1` and `-19` are both mapped to the same level.
- **Resolution:** do not spend time on this. For daily iteration, install packages on the device and generate initramfs there (tens of seconds).
- **Applicability:** Ubuntu 26.04; 2026.

### Building Mesa Under qemu-user Is Extremely Slow

- **Cause:** the NIR Python code generator takes more than 100 minutes under qemu-user, compared with tens of seconds natively.
- **Resolution:** cross-compile or build natively on ARM64.
- **Applicability:** Mesa; 2026.

### The Wrong Layer Was Cached

- **Symptom:** hundreds of lines of code are added to cache the distribution base layer, but the build becomes slower.
- **Cause:** caching was added without first measuring; the bottleneck was not in that layer.
- **Resolution:** measure first, then cache only outputs that are genuinely slow and whose inputs do not change. Key the cache by input hashes.
- **Applicability:** General.

### Hashing a 60 GiB Raw Image

- **Resolution:** calculate hashes only for files intended for final release (for example, sparse images or tar archives).
- **Applicability:** General.

### Another Task Overwrites the Output Directory, or the Wrong Source Revision Is Built

- **Symptom:** a build artifact whose identity has already been recorded is replaced mid-process; the build actually uses a commit from another branch.
- **Resolution:** allow only one writer to an OUT directory at a time (`flock`); never write again to an OUT whose identity has been recorded; automatically verify the source HEAD before building; record build-input identity and refuse to proceed if it does not match.
- **Applicability:** General.

### The Same Source Builds Different Results

- **Cause:** debug-information source filenames differ between ccache and direct GCC; missing path mapping in `KAFLAGS` leaks local paths into assembly debug information; umask changes directory permissions; APT indexes are not pinned; host `dtc` versions differ; AVB salt is random by default.
- **Resolution:** add `-ffile-prefix-map` to both `KCFLAGS` and `KAFLAGS`; fix `KBUILD_BUILD_TIMESTAMP/USER/HOST`; normalize permissions and timestamps; use `tar --sort=name`; fix the AVB salt.
- **Applicability:** General.

### A Version Tag Makes the Packaging Script Select the Wrong Version

- **Cause:** the packaging script uses `git describe` to find the kernel version tag, but a project release tag takes precedence.
- **Resolution:** restrict matching, for example with `git describe --match 'v[0-9]*.[0-9]*'`.
- **Applicability:** General.

### Disk Cleanup Deletes Files Still Needed by the Release Process

- **Symptom:** midway through release, a GPL source archive or a deb package is missing and must be downloaded or repackaged (changing its hash).
- **Resolution:** before cleanup, check which paths are referenced by the release manifest and receipts; do not treat a worktree with commits as disposable cache; move identity records outside the directory scheduled for deletion.
- **Applicability:** General.

### Build Checks Can Be Bypassed Through Environment Variables

- **Resolution:** do not leave backdoors in checks. Branches must be created from the product HEAD, otherwise retired parameters can be brought back.
- **Applicability:** General.

### Release Pitfalls

- GitHub Release has a 2 GB limit per attachment: split the root filesystem into volumes (for example, 1900 MB each), and include checksums and merge instructions.
- Internal codenames, experiment numbers and piles of hashes in release notes and README files are not useful to users. Release notes should say what works, what does not, how to install and how to verify.
- Too many attachments: group them into categories such as boot-image package, tools package, software-package bundle, root-filesystem volumes, checksums and manifest.
- Decide the commit author on the first day; rewriting history after release is expensive.
- When using `gh repo fork`, if an account already has a fork of the same upstream, the existing fork may be renamed.
- Ship source for GPL components (for example, BusyBox) with the release. Describe distribution trademarks as “based on [distribution], unofficial”.
- Put a “Let AI help you install” prompt on the README home page so users can copy it into an AI assistant and follow the documentation.
- **Applicability:** GitHub; 2026.

### A Package Version Is Lower Than an Internal Version Already Installed on the Test Device

- **Symptom:** release version 0.1.1 is lower than internal build counter 0.1.76 already installed on the test device, so APT treats the upgrade as a downgrade.
- **Resolution:** release version numbers must exceed every internal version already installed; use a suffix such as `~dev` for internal builds.
- **Applicability:** Debian packaging; 2026.

### A Published Tag Is Moved Afterwards

- **Symptom:** a published tag is moved to a new commit to rewrite the commit author.
- **Resolution:** never move a published tag; issue a new version to correct it.
- **Applicability:** General.

### Creating Full-capacity ext4 Filesystems for Empty and Root Partitions, Then Converting Them to Sparse

- **Symptom:** image preparation is slow and consumes large amounts of disk space.
- **Resolution:** format empty partitions on the device; deliver the root partition as tar or encode only used blocks.
- **Applicability:** General.

### Mesa Builds Do Not Support Incremental Resume

- **Symptom:** the build entry point deletes the old build directory by default, while `--resume` rejects a directory where packaging has already started.
- **Resolution:** retain build directories keyed by source and patch hashes, and support incremental rebuilds.
- **Applicability:** Mesa self-build; 2026.

### The Host Itself Becomes the Bottleneck

- **Symptom:** image processing reaches only about 10 MB/s and release uploads are slow.
- **Cause:** the host disk (for example, QLC) is more than 90% full; the desktop environment permanently consumes substantial memory; several large builds run in parallel; uploads use Wi-Fi.
- **Resolution:** keep free disk space; stagger large builds; use wired networking for uploads.
- **Applicability:** General.

### Version-by-version Progression

- **Method:** define the scope and acceptance method for each version → develop → test → finish and release → clean branches, worktrees and old artifacts → begin the next port. Do not accumulate unreleased candidates; start new features only after the current version is released.
- **Applicability:** General.

### External Documentation Contains Internal Problems

- **Symptom:** README and release notes contain investigation details, technical-debt IDs and root-cause analysis that users neither understand nor need.
- **Resolution:** external documentation should say what is complete, what is supported and how to use it. Retain safety warnings (data erasure, backups, slot rules); mention unsupported items briefly; list at most a few user-visible known issues, without explaining their root causes.
- **Applicability:** General.
