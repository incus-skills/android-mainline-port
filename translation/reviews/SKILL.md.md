# Translation record: `SKILL.md`

## Source

- **Source path:** `SKILL.md`
- **Source ref:** `main`
- **Source blob:** `9d9ea8aca4afde6ced79154958864196cc3da433`
- **Translation artifact:** [`translation/en/SKILL.md`](../en/SKILL.md)

## Translation intent

This is a functional English translation, not a rewrite. It aims to preserve the source's requirements, recommendations, conditions, sequencing, exceptions and unresolved points.

The translation retains the original file structure and technical identifiers. Terminology choices are intended to preserve operational meaning rather than reproduce Chinese wording literally.

## Terminology decisions

| Source expression | Translation used | Rationale / review consideration |
| --- | --- | --- |
| `立项` | Project scoping | Includes feasibility, scope and authorisation, not merely project initiation. |
| `取证与备份` | Evidence acquisition and backup | Preserves the investigative and evidentiary role of the work. |
| `点亮与内存桌面` | First display and RAM desktop | Refers to achieving visible display output and a usable desktop in memory, rather than simply powering on the device. |
| `原生接管` | Native takeover | Preserves the transition from borrowed firmware output to native display control. |
| `还技术债` | Pay down technical debt | Preserves the deliberate removal of temporary compromises. |
| `真机通过` | Passes on a real device | Keeps the distinction between actual-device evidence and simulated or offline results. |
| `第一小时审计` | First-hour audit | Retained literally because the source does not define a more specific audit procedure. |

These are working translation choices, not a general-purpose glossary mandate. Revisit them if later source documents establish a more precise or consistent usage.

## First-pass review observations

- The Stage 5 row lists several operations in compressed form. The translation retains that compression rather than turning it into a detailed installation procedure. The detailed procedure belongs in `references/process.md` and the relevant experience documents.
- The translation preserves the Stage 8 completion criterion as `—`; the source does not state one.
- The translation does not attempt to resolve source ambiguities by adding operational detail.
- The source's technical procedures have not been independently validated by translating them.

## Current assessment

- **Structural fidelity:** the source headings, tables, ordering and file references are retained.
- **Operational requirements:** translated.
- **Technical identifiers and paths:** retained.
- **Unresolved source ambiguities:** not silently resolved.
- **Technical correctness of the underlying Android porting procedures:** not independently validated by this translation.

## Status

`translated` — committed to the English reconstruction branch.

Semantic and technical review remain outstanding. This record documents translation decisions and review boundaries; it does not certify the underlying porting procedures.


## Pass 3 — technical and semantic review

Source/translation pair rechecked against `SKILL.md` at blob `9d9ea8aca4afde6ced79154958864196cc3da433`.

### Corrections made

1. The source says the experience comes from the process of porting Qualcomm tablets to Ubuntu 26.04 + GNOME. The translation's singular "on a Qualcomm tablet" unnecessarily narrowed the source's plural/general wording. It now reads "porting Qualcomm tablets to Ubuntu 26.04 + GNOME".
2. The source says the hardware and kernel sections are "基本通用" (broadly applicable/general). "Broadly reusable" could imply reuse of concrete components rather than applicability across devices. It now reads "broadly applicable".

### Review findings

- Frontmatter name and trigger description are preserved in meaning; technical tokens and product name remain unchanged.
- The five task-entry routes remain distinct: new project, takeover, technical decision, troubleshooting and assessment only.
- The nine-stage table preserves its stated completion criteria and per-stage references, including the blank Stage 8 completion criterion.
- The eight sequencing warnings, thirteen mandatory discipline rules, technical selection principles and eight user decision categories are present in the same order.
- File-index links remain relative to the translated skill location and point to the English `references/` and `experience/` trees.
- No source-defined stage, state machine or additional completion criterion has been introduced.

### Boundaries

- Source-based semantic/structural review: completed for this document.
- Current technical correctness of kernel, boot, hardware, distribution and agent behaviour: not assessed.
- External compatibility of the frontmatter trigger description: not assessed.
- This document's review does not close Pass 3; the remaining 21 operational documents are still in scope.
