# Translation glossary

This glossary is a controlled record of terminology used across the English edition.

Do not add an English equivalent solely because it sounds natural. Read the term in context, identify its operational role, and record distinctions that matter to an agent following the skill.

| Chinese source term | English equivalent | Meaning / operational role | Notes / status |
|---|---|---|---|
| 技术债 | technical debt | Temporary implementation or process compromises that must be recorded and repaid under defined conditions. | Provisional; confirm usage across source documents. |
| 取证 | evidence collection | Gathering device-specific facts and artefacts before making implementation decisions. | Prefer this over literal “forensics” where the context is engineering evidence. |
| 首次点亮 | first light | The first successful visible display output during bring-up. | Preserve as a named milestone. |
| 原生接管 | native takeover | Replacing a temporary firmware-provided display path with the native Linux display stack. | Provisional; verify in display documentation. |
| 完成标准 | completion criteria | Observable conditions that establish a step or task as complete. | Distinguish from a task description. |
| 回退 / 自动回退 | rollback / automatic fallback | Returning to a known-safe boot or system state after a failed operation or timeout. | Choose according to context. |

## Entry format

For each new term, record:

- the exact Chinese term;
- the chosen English equivalent;
- its meaning in the source context;
- nearby terms that must remain distinct;
- whether the choice is provisional or settled.

Keep command names, API identifiers, file paths, configuration keys, error strings, and other technical tokens unchanged.
