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


---

## Pass 3 — Technical and semantic translation review

### Review identity and evidence

- Pass: Pass 3 — technical and semantic review of the operational English translation set.
- Source: `experience/wireless.md`
- Source branch / blob SHA: `main` / `c8ae92e9d20207fd686398412da8df3e49216768`
- English target: `translation/en/experience/wireless.md`
- Target baseline before this review: `b033277ac0b59b81060adc54d2922c191037c27f`
- Target after the source-justified correction: `8bf6c27e2eb3a88fd0b55c8bd02dd7b10c1ee087`
- Pass 3 criteria: `0870d3ea261a6354649379b96891cda4ca2a0ba6`
- Current position: `f28ee4ccac568bb283e13a4c1d40bb65c9076969`
- Current checkpoint: `55a7fbd7ef2c3ac922282ff6c53e9e31c51a79b0`
- Glossary: `7cd456b51223fd2cd3cf408cb14bae7f7d167254`
- Translation instructions: `translation/README.md`, Phase 4; blob SHA `f9bf34ae28e838ae881f9e207797f26c1b327baa`
- Existing Pass 1 review: retained unchanged as provenance; blob SHA before this append `f0ac2791ea27b600678b2ed5b87f9cefc2eaae1d`.

### Observer position and scope

Compare the complete Chinese source and English target for semantic fidelity, imperative force, conditional scope, decision logic, technical tokens, structure, cross-document references and uncertainty. This is a translation review only. Current upstream behaviour, technical correctness and real-device validation are excluded.

### Observations

**1. Imperative force**

The route instructions preserve the source's requirements and cautions: collect vendor evidence read-only; preserve normal IOMMU isolation and do not enable passthrough; import patches with provenance; use a version-matched firmware set; set device identity before the relevant service; and apply the vendor transmit-power limit. The Bluetooth route retains the requirement not to hard-code the address in the device tree.

The failure remedies retain their original force, including prohibitions against using passthrough to conceal the stream-ID defect, treating fixed 20 dBm as a production solution, copying another device's calibrated board data, reloading the driver within one boot, or changing the regulatory country code at runtime. Recommendations and conditional remedies remain distinct from prohibitions.

**2. Conditional scope**

The translation retains the distinct conditions attached to the cases: 5 GHz versus 2.4 GHz; the 40 MHz observation and its HE confounder; regulatory regions without 6 GHz rules; board-data lookup by full key; firmware needed during initramfs; multiple-PMIC platforms; first-boot address injection; and suspend/resume in s2idle.

The two causes grouped under “5 GHz Scan Returns No Results” remain separate cases. The source's applicability labels are retained where supplied and are not generalised to all Qualcomm devices.

**3. Decision logic**

The Wi-Fi and Bluetooth routes and their completion criteria remain separate. The Wi-Fi progression retains the distinction between PCIe link, firmware board match, interface appearance, scan, association, network reachability, throughput, stability, reconnection and MAC stability. Bluetooth retains cold-boot availability, correct address, discovery, pairing, actual peripheral function, reconnection and suspend/resume. Neither scan visibility nor `hci0 up` is promoted to completion.

The translation preserves the diagnostic branches for the two 5 GHz scan failures, the requirement to separate channel width from HE when testing the 40 MHz workaround, and the two-ended attribution requirement for the Bluetooth peer-side ERTM issue.

**4. Technical tokens**

The complete pair was checked for commands, paths, identifiers, kernel interfaces, configuration names, error strings, units and version references. Tokens remain textually intact, including `qcom,wcnXXXX-pmu`, `ath11k`, `ath12k`, `hci_qca`, `iommu.passthrough=1`, `board-2.bin`, `board.bin`, `bdencoder`, `iommu-map`, `config_sid`, `rmmod`, `insmod`, `regulatory.db`, `btmgmt`, `epoll_ctl`, `EPERM`, `systemctl show`, `strace`, `btmgmt public-addr`, `hci0 up`, `UHID`, `s2idle`, `kprobe`, `l2ping`, `btsnoop`, `WLAN SMMU`, `RTNL`, `802.11d`, `BlueZ 5.8x`, and the approximately 20 dBm and 4.9–5.9 GHz values.

One material omission was found in the SAR-power failure description. The source says the vendor limit means transmit power never reaches the approximately 20 dBm failure threshold, whereas the mainline driver does not apply that limit. The baseline English text stated that the vendor driver applied the limit but omitted this explicit consequence. The target was corrected to state that transmit power therefore never reaches the threshold and that mainline does not apply the limit.

**5. Structure**

The recommended routes, completion criteria, individual failure records and unresolved section remain in their source order and retain their separate roles. The source varies the fields present in each failure record; the translation does not impose a uniform record template or add missing confirmation/prevention sections.

The two 5 GHz scan-result causes are presented as separate cases, preserving their different mechanisms and remedies. The three unresolved items remain grouped under the unresolved heading.

**6. Cross-document references**

The Chinese source contains no Markdown links or named dependencies on other project documents. The English target has not introduced any. No additional pattern or experience documents were needed to assess a source-to-target claim, so the collection-level Pattern C/F/G/H associations noted in the Pass 1 record were not promoted to source-established dependencies or revalidated in this bounded operation.

**7. Uncertainty**

All three source-unresolved matters remain unresolved in the translation: recurring Bluetooth `-84 frame reassembly`; the vendor's approximately eight-second boot-time radio calibration without a mainline counterpart; and the ACL return-path stall after repeated reconnections, whose responsible side is unknown. No cause, remedy or ownership was inferred.

### Decisions

- Corrected the omitted SAR-threshold consequence in the English target. This is a source-justified semantic correction, not an external technical correction.
- No other target change was justified by the source comparison.
- Retained the separate Wi-Fi and Bluetooth routes, completion criteria, failure mechanisms and unresolved boundaries.
- Did not consult external technical documentation or validate any implementation claim.
- Did not modify the Pass 1 review content; this Pass 3 record is appended.

### Unresolved matters

The three unresolved source matters listed above remain unresolved. The source does not establish a unified wireless architecture or common acceptance model. The current upstream status of cited community patches and all real-device behaviour remain outside this review.

### Validation

- Verified the exact source and target baseline revisions stated in Task 001 before review.
- Verified the task-specified Pass 3 criteria, current position, checkpoint and glossary revisions.
- Read the translation README's Phase 4 instructions.
- Compared the complete source and target across all seven Pass 3 dimensions.
- Re-read the corrected target passage after the single source-justified change.
- Source blob remains `c8ae92e9d20207fd686398412da8df3e49216768`; no source write was made.
- No external technical validation or real-device test was performed.

### Change boundary

- Changed: `translation/en/experience/wireless.md` — one sentence-level semantic correction concerning the SAR limit and threshold.
- Changed: this review record — appended this Pass 3 section.
- Deliberately unchanged: Chinese source, Pass 1 review section, Pass 3 criteria, current-position/checkpoint records, glossary, task definitions and all unrelated files.

### Next dependency

No additional document is required to complete this bounded source-to-target review. The orchestrator may independently integrate and evaluate this handoff. The next project-level operation remains an orchestrator decision; this worker does not alter the continuation position.
