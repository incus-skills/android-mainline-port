# Reconstruction and translation review: `experience/README.md`

## Source

- Repository: `incus-skills/android-mainline-port`
- Source branch: `main`
- Source path: `experience/README.md`
- Source blob SHA: `b8b4b9110215280c35316df33d540e292c09356a`
- English artifact: [`translation/en/experience/README.md`](../../en/experience/README.md)
- Review status: `reviewed` in Pass 3; technical validation remains outstanding.

## Translation intent and boundary

This is a functional English translation of the source document. It preserves the source's organisation, navigation instructions, entry schema, severity markers, maintenance rules, examples, technical identifiers and stated applicability boundaries.

The translation does not validate the Android procedures or the technical correctness of the experience recorded elsewhere in the library. It does not add new categories, entry fields, thresholds, or maintenance requirements.

## Reconstructed document role

The document performs several related functions:

1. **Defines the collection's scope and origin.** The experience library contains concrete, domain-organised experience from the Qualcomm tablet port to Ubuntu 26.04 with GNOME on Wayland, across mainline kernel versions 6.17 to 7.2. It distinguishes mostly distribution-independent hardware/kernel material from userspace material that requires distribution-specific adaptation.
2. **Provides navigation.** It identifies `patterns.md` as the cross-domain abstraction of recurring problem classes, then indexes the domain files and their subject matter.
3. **Defines retrieval routes.** New faults are classified through `patterns.md` before consulting a domain file; work on a stage or subsystem begins with its recommended route; log terms are used for direct searching.
4. **Defines a reusable entry shape.** An entry connects a concise symptom/cause statement with markers, symptoms, cause, confirmation, remedy, prevention and applicability.
5. **Defines collection maintenance.** Similar entries are extended rather than duplicated; only confirmed root causes enter the pitfalls collection; unresolved cases remain separately recorded with ruled-out possibilities; sensitive or identifying project details are excluded; upstream-fixed entries are retained with a fix annotation; changed recommended routes are updated in place with supersession noted.

It is therefore not merely a directory index. It is the collection's usage and maintenance contract.

## Structural relationships

- `SKILL.md` and `references/` are described as the relatively stable outer process/method layer; the experience library is expected to grow as projects contribute experience.
- `patterns.md` abstracts recurring problem classes across the domain files. It is a diagnostic routing aid, not a replacement for the detailed domain entries.
- Domain files contain concrete routes, completion criteria and pitfalls.
- `references/decisions.md` is explicitly connected to the entry format by a shared conditional-reasoning pattern: conditions, action, rationale and applicability boundary. This is a conceptual correspondence, not an assertion that the two documents have identical schemas or authority.
- The final section of `references/ubuntu.md` is identified as the distribution-adaptation reference for userspace material.

The README establishes navigation and maintenance relationships, but does not specify a complete dependency graph between every domain file and every framework reference.

## Terminology and translation decisions

| Source expression | English rendering | Reason |
|---|---|---|
| 经验库 | Experience Library | Names the evolving collection of domain experience without implying a formal database. |
| 推荐路线 | Recommended route | Preserves the source's practical, navigational meaning. |
| 完成标准 | Completion criteria | Keeps the acceptance-oriented meaning distinct from a general description of completion. |
| 踩过的坑 / 坑 | pitfalls | Idiomatic English for encountered failure modes and lessons; not rendered literally. |
| 问题模式 | problem patterns | Retains the cross-domain abstraction role of `patterns.md`. |
| 未解决 | Unresolved | Preserves the separate location/status for problems whose root cause is not confirmed. |
| 高频 / 耗时 / 致命 | Frequent / Time-consuming / Critical | Preserves the three source-defined severity/attention markers and their distinct criteria. |
| 适用范围 | Applicability | Retains the explicit boundary on where an entry's rule or observation applies. |
| 上游已修复 | Fixed upstream | Preserves the source's instruction to retain the entry and annotate the upstream fix. |

The severity markers are descriptive categories with source-defined meanings; they are not translated into numeric priorities or treated as a ranked scale.

## First-pass observations

1. **The library distinguishes confirmed knowledge from unresolved investigation.** A confirmed root cause may be represented as a reusable pitfall; an unresolved case remains separate and records what has already been ruled out. This is an explicit knowledge-admission boundary.
2. **Entries are intended to support retrieval and reuse.** Searchable symptoms, log text, error codes and function/configuration names are part of the entry design, not incidental prose.
3. **Applicability is part of the knowledge.** The entry must indicate whether it is general, Qualcomm-specific, tied to an SoC generation, or tied to a distribution version, and include a year. The source does not define a more detailed expiry or review cadence.
4. **The collection is cumulative but not purely append-only.** Similar entries are extended; recommended routes may be edited in place; superseded entries are annotated; upstream-fixed pitfalls are retained. Different content types therefore have different maintenance behaviours.
5. **Privacy and provenance are bounded deliberately.** The source prohibits recording identifying device/project details and experiment IDs, while permitting technical identifiers needed for matching. The document does not define a separate provenance schema for each experience entry.
6. **The stated empirical basis is specific.** The README attributes the experience to one Qualcomm tablet port and a stated Ubuntu/GNOME and kernel-version range. It also says hardware/kernel experience is largely distribution-independent, while userspace requires adaptation. This translation preserves those claims without independently validating their generality.

## Ambiguities and limits preserved

- The source says entries should be added only when the root cause is confirmed, but does not define a formal evidentiary threshold for confirmation. No threshold has been invented.
- The source says unresolved cases belong in an "Unresolved" section at the end of the file. It does not define a uniform unresolved-entry template beyond stating what has already been ruled out.
- The markers 【Frequent】, 【Time-consuming】 and 【Critical】 have separate qualitative definitions. The source does not define ranking, weighting, or mutually exclusive use; the translation does not add these.
- The phrase "the same reasoning" as `references/decisions.md` identifies a conceptual relationship, not a claim that every experience entry must contain every decision-catalogue field.
- The README calls out 13 domain files, but does not define whether every future experience item must fit one of those files or how a new domain file is approved. No new filing rule is inferred.
- The source describes the library as growing with each new project, but gives no formal review cadence, ownership model, or expiry policy for old entries.

## Review boundary

This record reconstructs the README's stated role, structure and relationships. It does not establish that:
- the recorded Android procedures are technically correct or current;
- the nine categories in `patterns.md` are complete or mutually exclusive;
- every domain entry conforms to the described format;
- the source's empirical claims generalise beyond the stated project context.

## Pass 3 source-based semantic review

The source and English translation were compared section by section. No source-driven semantic correction was required; the English artifact remains unchanged.

The comparison confirmed preservation of:
- the library's stated empirical origin, platform/kernel scope and distribution-dependence qualification;
- the relationship between the relatively stable outer process/method layer and the evolving experience collection;
- the instruction to start with `patterns.md`, the domain-file index and its stated subject boundaries;
- the three retrieval routes for new faults, stage/subsystem work and log-based searching;
- the entry format, its field meanings, applicability boundary and year;
- the three source-defined markers and their separate qualitative meanings;
- the maintenance rules for extending similar entries, admitting only confirmed root causes to pitfalls, recording unresolved cases, excluding identifying information, retaining upstream-fixed entries and marking superseded routes.

The source's explicit relationship to `references/decisions.md` remains a shared reasoning pattern, not an identical schema. The README's navigation role is not expanded into a complete dependency graph. No root-cause confirmation threshold, unresolved-entry schema, review cadence, expiry policy or new domain-admission rule was added.

### Pass 3 disposition

- Source-based semantic review: **completed**.
- Translation changes: **none required**.
- Technical correctness and generality of the empirical claims: **not validated**.
- Conformance of every domain entry to the README schema: **not validated here**.
- Whole-library validation: **outstanding**.
- Next document: `experience/patterns.md`.

The Chinese source remains authoritative. This review establishes translation fidelity only; it does not establish that the recorded Android experience is current, technically correct or generalisable.
