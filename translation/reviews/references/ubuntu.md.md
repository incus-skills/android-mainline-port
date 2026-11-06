# Reconstruction and Translation Record: references/ubuntu.md

## Source and provenance

- Source path: `references/ubuntu.md`
- Source ref: `main`
- Source blob SHA: `2f87d2512753af379615e05a7d15ea773fbafc0f`
- English artifact: `translation/en/references/ubuntu.md`
- Translation branch: `translation/english-reconstruction`
- Translation commit: `a073918b5e008399a075b47ed87886dd46be264d`

## Translation intent

This is a functional English translation of the Ubuntu integration and distribution-decoupling reference. It is not a redesign of the packaging model, a recommendation to adopt Ubuntu as a universal base, or a technical validation of the package hooks and distribution-specific procedures.

The first-pass objective is to recover how the source separates hardware, device, integration and desktop-policy concerns; how it packages and deploys those concerns; and how it describes adaptation to other distributions. The truth or current applicability of individual technical claims is reserved for a separate technical-evidence pass.

Preserve:
- the stated Ubuntu/GNOME baseline and the qualification that the structure should be adjusted to the actual project;
- the four-layer separation and the distribution root-filesystem boundary;
- the package table, package responsibilities and minimum separation rule;
- the kernel ABI, boot-image hook, initramfs, root-filesystem, snap, GNOME, debugging and release guidance;
- the cross-distribution comparison;
- user/device-unique data boundaries and rollback behaviour;
- technical names, package identifiers, paths, commands, configuration keys, interfaces and version labels.

Do not silently generalise Ubuntu-specific details, modernise version-specific interfaces, or turn recommendations into universal requirements.

## Reconstructed document role

This is a distribution-integration reference. It explains how to retain a replaceable distribution/userspace layer while keeping device-specific hardware and support work separate.

The source's central layered model is:

1. **Desktop policy layer** — GNOME defaults, scaling, power-button behaviour, first-boot services and browser replacement; rewritten when changing desktop or distribution.
2. **Integration layer** — boot-image hooks, dracut configuration, disabling distribution boot deployment and switching tools; adapted when changing distribution.
3. **Device layer** — udev, device services, UCM/topology, sensor stack, charging authentication and importing local data; mostly unchanged.
4. **Hardware layer** — kernel image, device tree, modules and firmware; unchanged.
5. **Distribution root filesystem** — official root filesystem, left unmodified except for adding packages.

The fifth item is shown as the base beneath the four project-controlled layers. It is not another project-owned integration layer.

The document then gives concrete implementation guidance across twelve sections: layer boundaries, package split, kernel package identity, boot hooks, dracut, rootfs composition, kernel configuration alignment, snap graphics, GNOME defaults, product debugging channel, release/upgrade and adaptation to other distributions.

The package structure operationalises the layer model, but the source allows package granularity and names to vary by project. It explicitly identifies kernel, firmware and desktop policy as the minimum separation.

## Terminology and translation decisions

- `发行版可替换` → **The Distribution Must Be Replaceable**. Preserves the design objective rather than implying that the current Ubuntu configuration is itself portable without adaptation.
- `桌面策略层` → **desktop policy layer**.
- `集成层` → **integration layer**.
- `设备层` → **device layer**.
- `硬件层` → **hardware layer**.
- `本机唯一数据` → **device-unique data**. Includes calibration, MAC address, Bluetooth address and sensor configuration; kept outside packages.
- `大管家` → **“master manager”**. Retains the source's informal warning against a single package/service controlling everything.
- `元包` → **metapackage**.
- `ABI 账本` → **ABI ledger**. Retains the source's named record of version, kernel commit and configuration hash.
- `启动镜像集` → **boot-image set**. Distinguishes the collection of images from an individual boot image.
- `切换存储` → **switching storage**. The source does not define its physical layout in this document.
- `首启服务` → **first-boot service**.
- `本机数据分区` → **device data partition**.
- `遮蔽 hypervisor 节点` → **mask the hypervisor node**. Retains the source's operational wording; its exact device-specific target is not expanded here.
- `上混` → **upmixing**.
- `首启设置` → **first-run setup / Initial Setup**, depending on whether the source refers generically to first boot or to the GNOME component.

The source's layered ASCII diagram is retained as a text diagram rather than replaced with a different diagram notation. This preserves its visible stack and layer annotations without introducing a new representation model.

Package names, paths, commands, configuration keys, systemd properties, snap interfaces and distribution names are retained as technical tokens.

## First-pass structural observations

1. The document has an explicit portability objective: hardware/device concerns should not become coupled to package-manager behaviour, a display manager, fixed user accounts or a desktop.
2. The four project-controlled layers are not equivalent. Hardware is intended to remain unchanged across distribution changes; the device layer mostly remains unchanged; integration and desktop policy are expected to be rewritten or adapted.
3. The official distribution root filesystem is a base boundary. The source says not to modify distribution files, and provides `dpkg-divert` as the Ubuntu/Debian replacement mechanism where replacement is necessary.
4. Device-unique data has a separate boundary from package content. It is imported from the local device data partition during installation, and absence is an explicit error rather than an invitation to bundle or guess the data.
5. Package splitting follows maintenance ownership, update frequency and layer. The source allows some consolidation (for example, audio into device-support) but insists that kernel, firmware and desktop policy remain separate at minimum.
6. Boot-image hooks are designed to fail safely when prerequisites are absent: report the condition, exit successfully and avoid writing slots or deleting rollback images. The source explicitly links hook failure to dpkg configuration failure during unrelated package updates.
7. The rootfs procedure aims to control initramfs generation count and keep routine iteration separate from release assembly.
8. snap GPU integration is distinct from system Mesa; the browser is preferred as a deb so it uses system Mesa directly.
9. The debugging channel is a product feature with explicit enablement and cold-boot checks, not an always-on default.
10. The final comparison table locates distribution-specific substitutions. The source's conclusion is that hardware/device layers need little or no change, while integration and desktop policy are rewritten.
11. The document is a concrete Ubuntu implementation reference, not a claim that every Android port must use GNOME, snap or Ubuntu.

## Relationships to other source documents

- `references/decisions.md` links to this document for Ubuntu integration details and contains related choices about initramfs, snap and browser packaging.
- `references/build-environment.md` is referenced for device-side initramfs generation and build efficiency.
- `references/process.md` describes the project stages in which package construction, installation, release and maintenance occur.
- `references/discipline.md` supplies broader constraints around records, technical debt, user data, work and release handling.
- `experience/ubuntu-userspace.md` is directly referenced for detailed GNOME defaults.
- `experience/usb-and-storage.md` and other domain experience documents provide details behind particular device, boot and storage operations.

These links and shared subjects establish relationships, not a complete formal dependency graph.

## Ambiguities and items to preserve for later review

- The opening states Ubuntu 26.04 LTS + GNOME as the experience baseline. The translation preserves that version exactly; it does not update the release label or assess its current status.
- The source says the layer/package structure is the result of actual project convergence but does not identify which project or decision history produced every boundary.
- “Switching storage” is not defined in this document. Its exact partitioning or storage mechanism should be recovered from related process/experience records rather than inferred here.
- The phrase “mask the hypervisor node” is retained without inventing a node path or implementation.
- The package table includes some optional consolidation and removal conditions. These are not a mandatory package manifest for every device.
- The cross-distribution table is a high-level adaptation map, not a complete porting procedure for Fedora, Arch or postmarketOS.
- The source's diagram communicates a layered stack and change responsibility. It does not specify all dependencies, runtime flows or interfaces between layers; those should not be inferred from the drawing alone.

## Review boundary and status

- Structural fidelity: layered model, twelve sections, package table, code/configuration blocks and cross-distribution table retained.
- Scope: Ubuntu-specific implementation details remain scoped to the stated baseline.
- Technical tokens: package identifiers, paths, commands, configuration keys, interfaces and versions retained.
- Representation: source's layered diagram preserved as a text diagram; no new topology introduced.
- Source ambiguity: storage mechanism, hypervisor-node target and full cross-layer dependencies remain unspecified.
- Technical correctness and current applicability: **not reviewed or validated**.
## Pass 3 source-based semantic review

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

### Pass 3 disposition

- Source-based semantic review: **completed**.
- Translation correction: **software packages** → **software-package bundle** in the release-attachments list.
- Technical correctness, current package/hook behaviour, distribution compatibility and real-device behaviour: **not validated**.
- Whole-skill validation: **outstanding**.
- Next document in the declared framework-reference sequence: `experience/README.md`.

The source's Ubuntu-specific baseline remains scoped as stated. The cross-distribution table is not treated as a complete porting procedure, and the layered drawing is not treated as a formal dependency graph.
