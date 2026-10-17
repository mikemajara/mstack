---
name: workflow
description: Define and follow software work phases, readiness gates, verification requirements and project memory. Use when researching, refining, implementing or validating a work item. Use when adding tasks or stories, anything that has to do with the software development lifecycle is captured in this skill.
---

# Workflow

Draft: phase contracts are incomplete.

Own phase definitions, prerequisites, required outputs, exit validation and valid
transitions. Apply the same standards to every actor. Execution permissions
belong to orchestrator; concrete work-record operations belong to tracker.

Main source of truth for the project is the 1. the codebase and 2. `AGENTS.md`. Read project `AGENTS.md` and relevant project memory before work. 

Material decisions, facts, questions and gotchas as they arise are captured in [memory](references/memory.md).

Read [lifecycle](references/lifecycle.md) to establish the current phase and valid
destinations. Use [work-item attributes](references/attributes.json) for
[type][attribute:type], [priority][attribute:priority], [phase][attribute:phase] and
[status][attribute:status] definitions. `label` names an attribute or value; `children`
lists its values. Concrete tracker mappings belong to tracker. Attribute values do not
establish readiness or completion; phase contracts still apply, including their draft
limitations.

## Phase contracts

Read the reference for the phase being performed:

| Phase or concern | Reference |
| --- | --- |
| [Research][phase:research] — Establish facts | [Contract](references/phase/research.md) |
| [Refinement][phase:refinement] — Specify scope and acceptance criteria | [Contract](references/phase/refinement.md) |
| [Implementation][phase:implementation] — Produce a change ready for [QA][phase:qa] | [Contract](references/phase/implementation.md) |
| [QA][phase:qa] — Judge the specified behavior | [Contract](references/phase/qa.md) |
| [Integration][phase:integration] — Validate [integration][phase:integration] | [Contract](references/phase/integration.md) |
| [Deployment][phase:deployment] — Establish [deployment][phase:deployment] facts | [Contract](references/phase/deployment.md) |
| Define scenarios or judge evidence | [Verification](references/verification.md) |

[Implementation][phase:implementation] complete means ready for [QA][phase:qa].
[QA][phase:qa] complete, [integration][phase:integration] complete and
[deployment][phase:deployment] are separate facts. Required evidence establishes
completion; a tracker field alone does not.

## Definitions

An actor: is a human or agent that works on a project. They both should comply with the rules stated in this workflow lifecycle.

Attribute references use `[name][attribute:value]`, e.g.
`[implementation][phase:implementation]` or `[blocked][status:blocked]`.
Use `[phase][attribute:phase]` for an attribute itself. These are semantic
markers, not navigation links; look up the labels in
[attributes.json](references/attributes.json) with grep or rg.
