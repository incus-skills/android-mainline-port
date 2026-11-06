# Build Environment and Efficiency

Goal: when starting a new project from scratch, configure the environment once; make routine iteration take minutes; do not waste time on builds.

Core principle: **measure first, then optimise**. The first time any step takes more than 5 minutes, time its sub-steps. Cache unchanged parts by input hash; set a time budget for each phase; review the pipeline after every release.

## 1. Host

| Item | Recommendation | Explanation |
|---|---|---|
| System | Linux x86_64 or ARM64 | With native ARM64 hardware (or an ARM64 CI runner), root-filesystem and Mesa builds are an order of magnitude faster |
| CPU / memory | At least 8 cores / 16 GB | Memory is tight when building all kernel modules, compiling Mesa and running QEMU at the same time |
| Disk | At least 300 GB free | Root-filesystem images, multiple OUT directories, backups and release artifacts fill the disk quickly; builds fail partway through when free space falls below approximately 30 GB |
| Permissions | Builds require actual root | An unprivileged user namespace cannot create a correct root filesystem (ownership and device nodes). The test machine can provide passwordless sudo; a more cautious approach is to allow only a fixed build script to run passwordlessly |

Pass host passwords and other credentials to scripts only through environment variables; do not write them to files or logs.

## 2. Toolchain

- Cross-compiler: the distribution's `gcc-aarch64-linux-gnu` (or LLVM), with the version pinned and recorded.
- Kernel build dependencies: make, bc, bison, flex, libssl-dev, libelf-dev, cpio, kmod, dtc and python3.
- Image tools: AOSP `mkbootimg`/`avbtool` (fixed versions), `img2simg`, e2fsprogs and GNU tar.
- Device tools: `android-tools` (adb, fastboot) and `sgdisk`.
- QEMU: `qemu-system-aarch64` (only for tests that must verify init behaviour), `qemu-user-static` + binfmt (for chroot builds).

## 3. ccache

ccache is critical to kernel iteration speed. Estimate its capacity from measurements first: a complete arm64 kernel build with approximately 1,300 modules (without debug information) occupies approximately 0.3 GB in compressed ccache storage, of which compiled results account for approximately 0.1 GB and direct-mode manifests approximately 0.2 GB (about 5× compression). Manifests keep growing when “the same source is built from different paths”, so if `base_dir` is not set and OUT paths are changed frequently, the cache grows quickly and the hit rate is low.

Recommended capacity: 10 GB is sufficient for kernel-only work (retaining approximately 30 complete builds); increase to around 20 GB when also using ccache for large C++ projects such as Mesa. With a limit that is too small (for example, 4 GB), entries are repeatedly evicted and the hit rate can fall below half.

Suggested configuration (`~/.config/ccache/ccache.conf`):

```ini
max_size = 10G
# Share cache between source and OUT directories at different paths (together with -ffile-prefix-map)
base_dir = /path/to/workspace
hash_dir = false
# Identify the compiler by its contents; invalidate automatically when its path changes or it is upgraded
compiler_check = content
```

For kernel builds:

```bash
make O=$OUT ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- \
     CC="ccache aarch64-linux-gnu-gcc" -j$(nproc) Image dtbs modules
```

Key points:

- Fix `KBUILD_BUILD_TIMESTAMP`, `KBUILD_BUILD_USER` and `KBUILD_BUILD_HOST`; add `-ffile-prefix-map=$SRC=/linux -ffile-prefix-map=$OUT=/kbuild` to both `KCFLAGS` and `KAFLAGS`. This makes artifacts reproducible and keeps the cache independent of paths.
- Disable `LOCALVERSION_AUTO`. Let packaging determine the version through the ABI, avoiding widespread recompilation because every commit changes the version string.
- Run `ccache -z` before each build and `ccache -s` afterwards to inspect the hit rate. If it drops suddenly, first check whether the OUT path, compiler arguments or compiler changed.
- Cache the ccache directory in CI (for example, GitHub Actions cache, keyed by kernel branch and configuration).

## 4. Kernel OUT Directories

- **One persistent OUT directory per configuration** (for example, product configuration and RAM diagnostic configuration). Do not create a new path every time just to make the build “clean”: changing paths means a cold build (tens of minutes), while an incremental build takes only one or two minutes. Perform one clean build before a formal release.
- Use `flock` to ensure that only one build uses a given OUT directory at a time. Record build inputs inside OUT (source path, commit, toolchain and compiler arguments); refuse to build if they do not match.
- Do not mix builds with modules and builds without modules in the same OUT directory.
- Build only the required targets: use `make dtbs` when only the device tree changes; use `make drivers/.../foo.ko` or `make M=` when only one driver changes. Do not run `modules_install` into a new directory every time.
- Do not write to an OUT directory whose identity has already been recorded or which has been referenced by a test or release.

## 5. Root-Filesystem Builds

Methods, from fastest to slowest:

| Method | Speed | Use |
|---|---|---|
| Native ARM64 (host or CI ARM64 runner) | Fastest; a complete root build takes around a dozen minutes | Preferred |
| Root privileges + qemu-user chroot (binfmt) | Approximately twice as fast as full-system emulation | When no ARM64 machine is available |
| QEMU full-system emulation (TCG) | Slowest; the same build takes more than 40 minutes | Only to verify init and systemd behaviour |

Approach:

- Base layer comes from the distribution's official image (for example, the SquashFS from an Ubuntu Desktop ISO); cache it by ISO hash.
- Package installation order: install distribution updates and project userspace packages first, purge the generic kernel last, then install the project kernel, so that initramfs is generated only once.
- dracut is often the largest individual time cost (`hostonly=no` requires packaging all modules). Do not rebuild the entire root filesystem during routine iteration: **install new packages on the device and generate initramfs on the device**; this takes tens of seconds. Build the complete root filesystem only for releases.
- Do not run multiple full-system-emulation virtual machines at the same time; they slow one another down and distort timing measurements.
- Calculate hashes only for final release files, not raw images of tens of GiB.
- Deliver the root filesystem as tar (or a sparse image encoding only used blocks); see `../experience/usb-and-storage.md`.

## 6. Other Large Components

- **Mesa:** cross-compile or compile natively; do not compile under qemu-user (code generators can be a hundred times slower). Pin source and patch versions.
- **Userspace components outside the kernel** (sensor stack, authentication daemons, etc.): cache slow build artifacts by “input commit + build-script hash”; recheck the hashes before reuse.

## 7. Reference Repositories

- Keep them in separate directories in the workspace (for example, `sources/`), use them read-only, and record their URLs and commits.
- For large repositories, use partial clone: `git clone --filter=blob:none`; use `--depth` when only history is needed.
- Keep complete upstream history in kernel repositories (third-party authorship and attribution); a local upstream mirror can serve as the object source and reduce downloads.
- Do not depend on absolute paths or build outputs from reference repositories.

## 8. Disk and Cleanup

- Check `df` regularly; when free space falls below approximately 30 GB, clean up before starting a large build.
- Safe to delete: partial images from failed experiments, superseded release directories, intermediate tar files, and OUT directories no longer in use (after confirming that no receipt references them).
- Do not delete: OUT directories with recorded identities; files referenced by the release process (for example, GPL source packages); worktrees with unmerged commits; or private evidence.
- Before cleanup, move identity records outside the directory to be deleted.

## 9. Parallelism

- Kernel builds share ccache and are serialised with one lock, to prevent two cold builds from slowing each other down.
- Each sub-agent works in its own worktree and branch; do not share OUT directories or devices.
- A device always has only one operator.

## 10. Unnecessary Overhead (Can Be Omitted)

- BTF and full debug information: require pahole and slow builds significantly; leave them disabled unless eBPF CO-RE is needed.
- Rebuilding the complete root filesystem after every change: install packages on the device instead.
- Running all KUnit or QEMU tests after every change: run only those affected.
- Hashing raw images and repeatedly performing “reproducibility” double builds: do this once before release.
- Creating a complete worktree and OUT directory for every experiment.
- Verifying snap, AppArmor or real hardware timing inside QEMU: conclusions cannot be extrapolated to real hardware; test on the real device instead.
