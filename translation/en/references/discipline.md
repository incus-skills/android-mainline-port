# Working Discipline

Porting projects take a long time, fail often, and are frequently handed from one person or agent to another. Sustained progress depends primarily on records being timely and accurate, rather than on how clever any one debugging session is.

This document specifies **what must be done and what must be recorded**. The exact document formats are up to you. They need only be clear, easy to find, and sufficient for the next person to take over directly.

## 1. Required Documents and What They Must Record

| Document | What to record |
|---|---|
| `AGENTS.md` (or the project's agent-instructions file) | Device and target; user-confirmed decisions (with dates); authorisation boundaries (which operations may run unattended); device-safety rules; commit-attribution rules; what must not enter public repositories. Do not record current progress here. |
| Status table | A one-sentence status for each feature (using only the status terms); a link to its detailed document; the **single next step**; current blockers; the device's present state, who is operating it, and whether any tasks are running. |
| Work log | Changes in conclusions, recorded by date; not a chronological activity log. |
| One detailed document per feature | Approach and sources for reuse; evidence (with version information for every item); investigation process and earliest failing layer; rejected hypotheses (preserve their original wording and append the scope in which they were rejected); completion criteria; next experiment. |
| **One technical-debt table** | See the next section. |
| Abandoned-approach record | What was abandoned, the evidence, the conditions under which the decision holds, and what new evidence would justify reconsideration. |
| Experiment records | What was changed; what each expected outcome would indicate; decision criteria; results (machine evidence and human confirmation kept separate); evidence locations. |
| External documentation | Written for someone encountering the project for the first time: risks, installation, upgrades, returning to Android, and the support table. Do not include internal codenames or progress details. |

Use only these status terms:

- 待调研 — Research pending
- 有源码线索 — Source-code lead available
- 离线通过 — Passed offline
- 真机启动 — Boots on a real device
- 真机通过 — Passed on a real device
- 真机失败 — Failed on a real device

## 2. Technical Debt: Register Shortcuts

To get the display working or establish a working path quickly, temporary approaches may be used. Examples include borrowing the firmware display, fixing transmit power, using conservative gain settings, debugging parameters, global ignore switches, application-level workarounds, and manual steps.

This is acceptable, but **every temporary approach must be registered in one shared technical-debt table** and paid down later.

Each entry must state at least:

- What temporary measure was introduced and where (file, package, configuration item).
- Why it is temporarily necessary and what the correct approach should be.
- Its effects and risks (functionality, power consumption, security, and maintenance).
- The conditions for removing it: what must be completed, and how removal will be verified.
- The related feature document.

Rules:

- Introduce the temporary measure and register its technical debt in the same commit.
- A feature working end to end does not mean its debt has been repaid. Return to the debt after the path works and bring it to a proper resolution.
- Ordinary features that have not yet been implemented do not belong in the technical-debt table; record them in the status table.
- Review the table regularly, ordered by impact, and select the next set of debts to repay.

## 3. When to Write Records

- **Before starting a feature:** record its goal, approach, and completion criteria.
- **Before each device session:** record the expected result and decision criteria for the experiment. After the session, record the result and evidence location on the same day.
- **When a conclusion changes:** update the work log and status table.
- **When introducing a temporary measure:** register the technical debt.
- **When abandoning an approach:** record the abandonment.
- **When the user makes a decision:** record it in `AGENTS.md`.
- **When the same failure occurs for a second time:** stop and write an analysis covering the earliest failing layer, hypotheses already ruled out, and the next experiment capable of falsifying an explanation.
- **Before ending a session or handing work over:** update the status table's next step, blockers, and device state. The next person or agent must be able to continue by reading the documents alone.
- **When a sub-agent completes a task:** it must update the records for which it is responsible in the same way. The primary agent checks its work.

Writing rules:

- Record conclusions and evidence, not a running diary. Put raw logs in `private/`; include only their paths in the documents.
- Record each conclusion in one place. Link to it from elsewhere rather than duplicating it.
- Append new records for failures and rollbacks; do not rewrite old records.
- Every real-device conclusion must include the source commit, image hash, test scenario, and evidence location.
- When adding a new rule to `AGENTS.md`, add an automated check at the same time where possible; otherwise the rule will soon be forgotten.

## 4. Directory Structure

A private workspace is recommended, containing two independent public repositories nested inside it. The workspace's `.gitignore` should ignore those repositories:

```text
<project>-workspace/                 Private workspace; local Git only
├── AGENTS.md
├── docs/                             Status table, work log, technical debt, abandoned approaches,
│                                     feature documents, research and experiment records
├── tools/                            Internal scripts: device orchestration, collection, analysis
├── sources/                          Reference repositories (read-only; record source and commit)
├── private/                          Git-ignored: raw logs, backups, data unique to this machine
├── out/                              Git-ignored: build outputs and caches
├── kernel/                            Kernel repository (independent Git; public; preserve upstream history)
└── public/<project>/                 Integration repository (independent Git; public)
    ├── README.md                     Risks, quick start, "ask AI to help you install" prompt, support table
    ├── docs/                          Installation, upgrades, build and development documentation
    ├── kernel/                        Locked kernel commit and configuration information; ABI ledger
    ├── device/                        Device userspace
    ├── packaging/                     Packages
    ├── firmware/                      Vendor firmware (only if the user agrees to distribution)
    ├── tools/                          Build, package, install, switch and test tools
    └── tests/                          Tests; test data comes from real devices
```

- Keep each piece of code in one place: kernel changes belong in the kernel repository, integration code in the integration repository, and the private workspace contains only records and internal scripts. Do not keep long-lived synchronised copies.
- Serial numbers, MAC addresses, Bluetooth addresses, calibration data, backups, passwords, and logs containing such information belong only in `private/`; do not commit them even to a private repository. Pass passwords only through environment variables.
- Fixes must enter an exportable patch series (commits in the kernel repository). They must not exist only in a temporary working directory.

## 5. Commits and Attribution

Agree with the user on the first day:

- Commit author.
- Repository account.
- Repository names.
- Licences.

Changing these after publication requires rewriting history.

- Commits completed by AI must add only `Co-Authored-By:`; do not fabricate human attribution or add `Signed-off-by` or `Tested-by` on someone else's behalf.
- Preserve the original author, date, and signature when importing third-party commits. Record the source repository and commit.
- Ask the user before each of the following: pushing, creating a repository, making a release, changing visibility, or rewriting history.

## 6. Rules for Working

- Change only one variable at a time. Decide in advance how the result will be judged.
- When the same failure occurs for the second time, stop and find the earliest failing layer; do not write a third patch.
- Do not mask problems with global switches, disabled modules, fixed delays, or unconditional retries. If a temporary workaround is genuinely needed, register it as technical debt.
- Only one person or agent may operate the device at a time. Include the device serial in every device command.
- Do not ask the user to perform measurements that can be automated; do not replace read-only measurements with questions.
- The user should do only what requires a human. Give advance notice of when their participation is needed, and arrange to complete those actions in one session.

## 7. Workload Estimation

Do not estimate in person-days. Estimate a range using this method:

1. For each subsystem, list the layers that remain uncertain: power and clocks, bus protocol, kernel driver, remote-processor firmware and protocol, userspace, and output. Do not count a layer already verified on a real device with the same SoC. A claim by someone else that it is supported does not count as verification.
2. Estimate 2–5 real-device test rounds for each uncertain layer. Use the upper end for closed protocols, remote processors, and display paths; use the lower end when vendor source code is available to inspect.
3. Each real-device test round takes approximately 10–30 minutes (build, boot, observe, recover, and wait for the battery). Where the user must be present, estimate against the time they can make available.
4. Add costs that are almost certain to occur: problems in host-side scripts, periods when the device is not available, and revisiting known pitfalls. Reading `experience/` first can avoid most of the latter.
5. Output a time range, the three largest sources of uncertainty, and the conditions under which the range could narrow.

The real bottleneck is usually the number of available real-device test sessions, not the amount of code. Display, audio, and Wi-Fi often take the most time. The most efficient situation is one in which "the device is online, a candidate is ready, and authorisation is clear". Prepare for those windows rather than accumulating untested candidates while the device is unavailable.

## 8. Sub-Agent Responsibilities

- The primary agent is responsible for planning, acceptance, and communication with the user. It personally checks critical evidence (hashes, diffs, and real-device results).
- Sub-agents perform specific tasks with clearly defined scope: applying patches, building, running tests, and organising information. Their output is a candidate result. If a sub-agent says something was tested, check whether the test method matches real use (cold boot or live switching; real device or emulator).
- Task instructions must specify: goal; the one permitted approach; files allowed to change; one verification command and its pass criteria; stop conditions (completion, the same error occurring twice, timeout, need for a device, or need for a decision); and records that must be updated.
- Do not assign open-ended tasks. Check direction shortly after work begins; stop it immediately if it is going in the wrong direction.
- Sub-agents must not touch the device by default. Two sub-agents must not share a source directory, output directory, or device.
- Before a device session, have an independent sub-agent specifically look for faults. This often catches serious problems before the device is used.
