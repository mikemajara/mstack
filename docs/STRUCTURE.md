# mstack structure

Exactly three skills. Draft files exist; incomplete contracts are marked in
place. Maintain behavior in the owning skill and use this document as a map.

| Skill | Owns | Must not own |
| --- | --- | --- |
| [workflow](../skills/workflow/SKILL.md) | Phase contracts, readiness and verification criteria, valid transitions, work shaping and default project memory | Runtime tools, execution authority or tracker mappings |
| [orchestrator](../skills/orchestrator/SKILL.md) | Selection, admission, dispatch, limits, permissions, waiting, cancellation, reconciliation and reporting | A competing lifecycle or weaker validation |
| [tracker](../skills/tracker/SKILL.md) | Specifications, validated workflow facts, evidence and relationships through concrete integrations | Product decisions, readiness rules or completion inferred from tool status |

## Files

```text
skills/
  workflow/
    SKILL.md
    references/
      attributes.json     # Work-item type, priority, phase and status vocabulary
      lifecycle.md        # Flowchart, entry points, gate model, blocked/resume
      phase/
        research.md       # Facts and handoff to refinement; current review focus
        refinement.md     # Scope, acceptance criteria and verification brief
        implementation.md # Reviewable change and self-check handoff
        qa.md             # Behavioral judgment and rework
        integration.md    # Integration proof; incomplete contract
        deployment.md     # Separate deployment fact; phase applicability unresolved
      verification.md     # Shared evidence and version traceability
      memory.md           # Default read/capture and deliberate promotion
  orchestrator/
    SKILL.md
    references/
      herdr.md            # Execution integration placeholder
  tracker/
    SKILL.md
    references/
      github.md           # Work-record integration placeholder
```

Phase references are not additional skills. Cross-cutting data-migration
requirements remain a later drafting topic; there is no additional skill root.

## Optional integrations

Herdr belongs to orchestrator; GitHub belongs to tracker. An integration is a
bounded set of instructions for using a concrete system. It need not be a
deterministic program. A skill may have or use no integration where its
capabilities allow.

The owning skill defines the general integration contract and handles
availability/failure policy. Concrete references describe that system's actual
operations and constraints. Runtime/project policy determines execution and
decision authority; workflow supplies validation standards for every actor.
Installation/fallback behavior and installed cross-skill resolution are pending.

## Authoring boundaries

Keep design discussion and migration context in [HANDOFF.md](HANDOFF.md) and
[MEMORY.md](../MEMORY.md), rather than distributing them in skill instructions.
Read [AGENTS.md](../AGENTS.md) for repository constraints.
[PROVENANCE.md](PROVENANCE.md) records the unchanged archive and import history.
No skills have been installed or automation activated by this authoring work.
