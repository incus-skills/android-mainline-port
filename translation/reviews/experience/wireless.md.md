# Translation review: Wi-Fi and Bluetooth

## Source and scope

- Source: experience/wireless.md
- Source branch: main
- Source blob SHA: c8ae92e9d20207fd686398412da8df3e49216768
- Translation: translation/en/experience/wireless.md
- Traversal: Pass 1 — initial structural and linguistic reconstruction
- Review scope: route, completion criteria, failure records, terminology, cross-document relationships and unresolved boundaries
- Technical validation: not performed

## Document role and structure

The source is a domain experience record for Qualcomm Wi-Fi (WCN, ath11k/ath12k over PCIe) and Bluetooth (the combo-chip UART path using hci_qca). It is organised into separate recommended routes, completion criteria, concrete pitfalls and three unresolved issues.

The document does not define a unified wireless architecture or a formal state machine. The two routes share a combo-chip context in places, but retain distinct buses, drivers, setup requirements and acceptance sequences.

## Translation decisions

- “无线” is rendered as “Wi-Fi and Bluetooth” because those are the two domains explicitly covered by the source.
- “原厂” is rendered as “vendor” or “vendor system/configuration”, not “factory” where the intended referent is the device's original vendor implementation.
- “只读取证” is rendered as “read-only evidence” or “read-only collection”, preserving the restriction against modifying the device while gathering baseline evidence.
- “board 数据” is rendered as “board data”; it is not collapsed into generic firmware.
- “功率上限” in the Wi-Fi route and SAR case is rendered as “transmit-power limit”; “SAR” is retained.
- “监管区域/国家码” are rendered as “regulatory region/country code”. The translation preserves the source's distinction between configured country code, regulatory database loading and 802.11d information advertised by the access point.
- “使能脚” is rendered as “enable GPIO”; PMIC ownership and GPIO numbering remain separate checks.
- “父串行引擎包装器” is rendered as “parent serial-engine wrapper”.
- “对端主机” is rendered as “peer” or “peer computer”, retaining that Bluetooth testing can fail because of the other endpoint.
- “回程” in the ACL issue is rendered as “return path”; the source does not identify which side owns the stall.
- Technical identifiers and names are retained rather than translated or generalised.

## Structural observations

### Wi-Fi route

The recommended route is evidence-led: establish the vendor baseline without writes; recover power dependencies from the vendor device tree; preserve IOMMU isolation; import driver changes with commit-level provenance; use a version-matched firmware set; set the device-specific MAC before NetworkManager; and reproduce the vendor transmit-power limit.

The PCIe PHY supply is singled out as a difficult evidence problem. The source permits candidate inference from references, but requires read-only evidence followed by a single-variable real-device decision. Shared supply consumers make casual voltage changes unsafe.

### Bluetooth route

The Bluetooth route requires enabling UART and its parent serial-engine wrapper, establishing PMIC ownership of power sequencing and the enable GPIO, matching firmware to the vendor version and making it available at the requesting boot stage, then setting the device-specific address before bluetooth.service rather than hard-coding it in the device tree.

### Completion criteria

Wi-Fi acceptance is explicitly layered: PCIe link, complete firmware board match, interface appearance, scan, association, network configuration/reachability, throughput, stability, reconnection and MAC stability. It also specifies actual 5 GHz frequencies and 80 MHz channels. Scanning is explicitly insufficient to establish connectivity.

Bluetooth acceptance moves from cold-boot controller availability and correct address through discovery, pairing, an actual user-facing peripheral function, reconnection and suspend/resume. An administratively up HCI device is explicitly insufficient.

These criteria share a progression from lower-level availability to user-facing operation, but the source does not name a common acceptance framework.

## Failure-record structure and distinctions

Wi-Fi cases cover high transmit power/SAR behaviour; two distinct causes of missing 5 GHz scan results; incorrect stream-ID parsing under normal IOMMU isolation; missing direct evidence for the PCIe PHY supply; unsafe driver reload/recovery behaviour; device-specific MAC recovery; interacting regulatory mechanisms; and PCIe link retention combined with power-domain shutdown during s2idle.

Bluetooth cases cover serial-engine wrapper and firmware availability; incorrect PMIC ownership for an enable GPIO; a systemd-only btmgmt failure caused by standard-input handling after an earlier incorrect startup-order diagnosis; public-address absence and the unconfigured state; missing UHID; a peer-side ERTM defect; and first-boot address injection.

The source varies the fields included in individual entries. The translation does not impose a uniform confirmation/prevention structure where the source omits one.

## Cross-document relationships

The wireless source itself contains no Markdown links to other experience documents. experience/patterns.md explicitly lists wireless cases under:

- **Pattern C — timing and lifecycle:** PCIe link retention with power-domain shutdown and btmgmt standard input set to /dev/null.
- **Pattern F — low-level success is not user usability:** scan visibility without connectivity and hci0 up without successful pairing.
- **Pattern G — upstream or distribution has already fixed it:** wireless frequency calculation and scan-during-recovery community patches.
- **Pattern H — another implementation cannot be copied wholesale:** wireless board-data and device-specific calibration concerns.

These are explicit collection-level references. They do not mean every wireless issue belongs exclusively to one pattern.

## Observer-reconstructed correspondences

- **Identity and calibration provenance:** PCI subsystem identity, firmware version, board-data key and device-specific calibration are coupled. A firmware experiment is not interpretable unless the full match is visible in logs.
- **Hardware ownership and reference recovery:** supplies, PMICs, GPIOs, PCIe PHY and serial-engine wrappers must be grounded in target-device vendor evidence; reference devices provide candidates, not authority to copy values.
- **Lifecycle and recovery boundaries:** driver reload, s2idle power-domain handling, Bluetooth service startup and controller initialisation are distinct contexts.
- **Regulatory state is not a single setting:** regulatory.db availability, configured country code, AP-advertised 802.11d country and driver band calculation interact, but the source does not formalise them as a state model.
- **Endpoint attribution:** Bluetooth packet evidence may require capture at both ends; a peer kernel defect can imitate a local failure.
- **User-facing acceptance:** scan results, HCI administrative state and controller appearance are intermediate observations, not final success.
- **Private device identity:** Wi-Fi MAC and Bluetooth address are recovered from device-local data and must not be embedded as fixed public-image data.

## Unresolved and underspecified areas

The source explicitly leaves three issues unresolved:
- Bluetooth initialisation reports -84 frame reassembly on every attempt, with no identified cause.
- The vendor's approximately eight-second boot-time radio calibration has no corresponding mainline implementation.
- After repeated Bluetooth reconnections, the ACL return path stalls despite pairing, file transfer and audio working; responsibility is not assigned to either endpoint.

The source does not establish a general remedy for these issues, a device-independent wireless configuration, or the current upstream status of the community patches mentioned.

These boundaries are retained rather than filled from external wireless documentation.

## Bounded sufficiency for Pass 1

For the current structural and linguistic reconstruction purpose, the two recommended routes, their distinct completion criteria, the failure mechanisms recorded by the source, the collection-level pattern references and the three unresolved issues have been recovered.

The unresolved items require additional device, implementation, peer-side or author evidence. They do not prevent moving to the next document. Reopen a specific issue only if a new discriminating source, relevant observer-position change, translation conflict or concrete consequence test becomes available.

No driver, firmware, regulatory, IOMMU, power-sequencing, BlueZ, Bluetooth protocol or real-device claim has been technically validated in this pass.
