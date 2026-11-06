# Translation Review — Repository README

## 1. Source and counterpart

- Source: `README.md`
- Source branch: `main`
- Source blob SHA: `3f268fc2cc6614edc40e90e7852326e6a1c6d385`
- Existing English counterpart: `README.en.md`
- English counterpart blob before review: `664eed85965dc1325b900b5d16033c112a2f8800`
- Pass: **Pass 2 — repository-facing English material**
- Review scope: source-to-counterpart fidelity, reader orientation, installation guidance, risk and contribution boundaries.
- Technical validation: not performed.

The English counterpart already exists in the upstream repository. This task is a source-based review and limited correction, not a new translation from an empty file.

## 2. Document role

The Chinese README is the repository-facing introduction and user entry surface. It:
- identifies the project and its intended audience;
- explains the origin and scope of the experience;
- names the three source projects;
- presents the principal uses and selected lessons;
- gives installation routes for supported agent environments;
- provides examples of requests;
- maps the repository's framework and experience-library layout;
- summarises the nine-stage porting process;
- states platform/freshness boundaries, risks, contribution rules and licence.

It is not the operational skill entry point. The README points readers toward the skill; it does not replace `SKILL.md` or the framework references.

## 3. Source-to-English correspondence

The existing `README.en.md` preserves the source's section order and covers all major content groups:

| Source section/content | English counterpart | Review |
|---|---|---|
| Project identity, audience and purpose | Opening paragraphs | Meaning preserved |
| Three Qualcomm tablet source projects | Source-project table | Device, SoC and project links preserved |
| Native Ubuntu GNOME dual-boot context | Source-project explanation | Meaning preserved |
| WeChat/Xiaohongshu communication channels and QR assets | Follow and discuss | Preserved; English adds the clarification that both channels are in Chinese |
| Five principal use cases | What it helps with | Meaning and distinctions preserved |
| Six selected time-saving lessons | A few lessons that save the most time | Preserved; one wording narrowed the source's evidence term and was corrected |
| npx skills installation | Recommended: npx skills | Commands and Node.js requirement preserved |
| Claude Code plugin installation/update | Claude Code plugin | Commands preserved |
| Gemini CLI installation | Gemini CLI | Command preserved |
| Manual clone and agent-specific directories | Manual git clone | Table and directory guidance preserved |
| claude.ai/desktop app upload | claude.ai and the desktop app | Steps preserved |
| Agent-assisted installation prompt | Let your agent install it | Full prompt and safety constraints preserved |
| Installation check and usage examples | Check that it works / Examples | Preserved |
| Framework/experience layout and nine stages | Layout / process table | Document names, descriptions and stage order preserved |
| Scope, freshness and risks | Scope and freshness / Risks | Preserved |
| Contribution and licence | Contributing / License | Preserved |

## 4. Translation correction

The source says that remote-processor messages can wake the system from deep sleep and do not appear in the **唤醒记录** ("wake-up records").

The previous English counterpart said "do not show up as wakeup sources". "Wakeup sources" can be read as a more specific kernel concept than the source wording establishes. The sentence has therefore been changed to:

> On Qualcomm, default to s2idle: messages from the remote processors fully wake the system from deep sleep and do not appear in the wake-up records.

This is a translation-fidelity correction only. It does not validate the underlying power-management claim.

## 5. Terminology and presentation observations

- **操作手册与经验库** is rendered as "a handbook and experience library"; this preserves the combined procedural and accumulated-experience role.
- **Agent Skill** remains the established product/format term.
- **取证与备份** is rendered as "evidence collection and backup", consistent with the controlled glossary's preference over literal "forensics".
- **首次点亮** is rendered as "first light", preserving the named milestone.
- **原生接管** is rendered as "native takeover", consistent with the glossary.
- **技术债** is rendered as "technical debt".
- The existing English text uses "owner" for the person who makes project decisions. This is a reasonable contextual rendering of the user-facing decision responsibility; no alternative is imposed.
- Proper names, command lines, paths, repository names and URLs are retained.
- The English counterpart adds minor explanatory wording in places (for example, that both social channels are in Chinese). These additions clarify the source presentation without changing the operational scope.

## 6. Boundaries and questions not resolved here

- The README contains technical examples and time-sensitive statements (kernel versions, Ubuntu release, Node.js requirement, agent installation commands, kernel behaviour and ccache sizing). Their technical/current validity is outside this translation review.
- The README describes supported or commonly used agent installation routes, but does not define a formal compatibility matrix or guarantee for every version of each agent.
- The two QR images remain source assets. This pass does not decide whether they should be retained, replaced or omitted in a future English-facing distribution.
- The root README and README.en use the same image paths; no asset transformation is required for the textual counterpart.
- Plugin marketplace metadata is outside this document's scope and remains for a separate integration/localisation decision.
- This review does not assess whether the public README's examples and installation guidance match current external product interfaces.

## 7. Bounded sufficiency

For the stated purpose, all major README content groups have a corresponding English expression, the existing English structure tracks the Chinese source, and the identified terminology narrowing has been corrected.

No additional translation change is presently indicated by the source comparison. Remaining issues require either external/current compatibility evidence, an explicit distribution decision about assets, or a separate integration scope.

## 8. Validation boundary

- Full source and counterpart comparison: performed.
- Translation correction: one wording adjustment.
- Technical validation: not performed.
- Current external installation compatibility: not checked.
- Whole-skill integration validation: not performed.
