# Android Mainline Port repository restructuring

## Governing objective

Restructure the Android Mainline Port repository to support independently identifiable and operational source-language and English editions, while establishing a sustainable workflow for maintaining their functionality, ingress procedures, provenance and relationship over time.

The English edition is a projection of the upstream project from a particular observer position at a particular point in time. Upstream changes perturb that projection and initiate a new reconstruction traversal when necessary.

The restructuring work must therefore produce both the repository structure and the operational workflow that maintains it.

This objective was approved by the project owner on 2026-11-07. It governs the restructuring workstream and supersedes treating individual inventory or construction-plan tasks as the overall goal.

## Required outcomes

### 1. Independently identifiable editions

- Preserve the source-language edition as an independently identifiable and operational edition.
- Establish a dedicated English-edition branch, with its own independently identifiable and operational package.
- Keep the English operational package at the agreed normal package location on its dedicated branch, subject to the remaining owner decisions and implementation evidence.
- Keep reconstruction history and coordination records separate from operational product content.
- Keep validation tooling and fixtures separate from the operational product, on the separately intended validation-harness branch.

The final English branch name, stable consumer-facing ref, final base commit, and unresolved package/support-file choices must be recorded when decided; do not infer them.

### 2. Maintained reconstruction workflow

Establish and maintain the procedures required to keep the repository and both editions functional over time, including:

- orchestrator and worker ingress procedures;
- bounded task definition, execution, handoff and independent acceptance;
- durable current-position and session-continuity records;
- evidence, provenance and validation requirements;
- ownership boundaries between product editions, reconstruction records and validation material.

These procedures are operational parts of the repository, not one-time documentation. Changes to repository structure or workflow must include corresponding ingress and continuity updates so that a fresh session can enter without rediscovering the whole repository.

### 3. Upstream-to-English update workflow

Establish a repeatable process for maintaining the dedicated English branch as upstream evolves:

1. Identify and pin the new upstream state.
2. Record the change relative to the source state underlying the current English projection.
3. Treat the upstream change as a perturbation of the English reconstruction.
4. Re-enter the reconstruction traversal to identify affected distinctions, relationships, dependencies and previously established decisions.
5. Determine what remains valid, what requires reconsideration and what requires a new English projection.
6. Update the dedicated English edition only through bounded, evidenced work.
7. Validate package integrity, discovery/provisioning behaviour and operational function within the declared validation boundary.
8. Record provenance, decisions and the resulting English projection so the next update can start from a reconstructable position.

This is not a blind merge or an automatic translation pipeline. An upstream update initiates reassessment; it does not by itself authorise mechanical reproduction of changes. The English edition's own experience and continuity must remain identifiable independently of upstream acceptance of contributions.

## Working constraints

- Translation work remains on hold while restructuring tasks take precedence. Do not resume translation merely because restructuring documentation or infrastructure has been created.
- Preserve the source-language product and existing reconstruction history unless a bounded task explicitly authorises a change.
- Do not invent external distribution, provisioning, installation or discovery behaviour. Obtain and pin the actual workflow/configuration and package locator before claiming or changing that behaviour.
- Distinguish observed repository state, owner-approved decisions, reconstructed relationships and unresolved questions.
- Treat task completion and orchestrator acceptance as separate events.
- Do not promote a precedent or current directory arrangement into policy without checking the governing workflow.
- Keep each worker's evidence packet bounded to the task, but sufficient to reconstruct the operation independently.

## Current authoritative entry points

- [Orchestrator ingress](ORCHESTRATOR-INGRESS.md)
- [Worker ingress](WORKER-INGRESS.md)
- [Current restructuring position](current-position.md)

The ingress records define how to resume work. The current-position record identifies the present state and next supportable operation; it is not a substitute for the task's evidence packet.
