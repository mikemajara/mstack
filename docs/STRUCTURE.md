# mstack structure

This is a development guide for this repository. It maps where contributors
maintain mstack's source files; it is not a target-project requirement or part
of the installed skills' operating instructions.

Exactly three skills. Draft files exist; incomplete contracts are marked in
place. Maintain behavior in the owning skill and use this document as a map.

## Repository files

| File or directory | Purpose |
| --- | --- |
| `AGENTS.md` | Instructions for developing mstack itself |
| `README.md` | Project entry point and source pointer |
| `MEMORY.md` | Current review context and unresolved design questions |
| `docs/STRUCTURE.md` | File and responsibility map |
| `skills/` | Draft product instructions and references |
| `legacy/backlog/` | Frozen historical input for comparison |

Repository guidance and continuity are separate from the product's workflow and
memory contracts. See [AGENTS.md](../AGENTS.md) for the authoring rules.

## Skill ownership

| Skill | Owns | Must not own |
| --- | --- | --- |
| [mstack-workflow](../skills/mstack-workflow/SKILL.md) | Phase contracts, readiness and verification criteria, valid transitions, work shaping and default project memory | Runtime tools, role scope or tracker mappings |
| [mstack-orchestrator](../skills/mstack-orchestrator/SKILL.md) | Selection, admission, dispatch, limits, role scope, waiting, cancellation, reconciliation and reporting | A competing lifecycle or weaker validation |
| [mstack-tracker](../skills/mstack-tracker/SKILL.md) | Specifications, validated workflow facts, evidence and relationships through concrete integrations | Product decisions, readiness rules or completion inferred from tool status |

## Files

```text
skills/
  mstack-workflow/
    SKILL.md             # Shared mStack version in frontmatter
    CHANGELOG.md         # Stack-wide release changes and workflow impact
    references/
      attributes.json     # Work-item type, priority, phase and status vocabulary
      lifecycle.md        # Flow across phases: entry, gates, blocked, header fields
      phases/             # One contract per phase
        research.md       # Facts and handoff to refinement
        refinement.md     # Scope, acceptance criteria and verification brief
        implementation.md # Specified features implemented in code
        qa.md             # Behavioral judgment and rework
      verification.md     # Shared evidence, tested commits and invalidation
      memory.md           # Default read/capture and deliberate promotion
  mstack-orchestrator/
    SKILL.md             # Shared mStack version in frontmatter
    CHANGELOG.md         # Stack-wide release changes and orchestrator impact
    references/
      configuration.md    # .mstack/config.json, execution settings and tracker selector
      herdr.md            # Herdr operation mapping and capability limitations
      cursor-cloud.md     # Cursor Cloud API agent/run mapping
      cursor-subagents.md # Native Cursor session delegation
      cmux.md             # macOS/TUI command mappings and capability limitations
  mstack-tracker/
    SKILL.md             # Shared mStack version in frontmatter
    CHANGELOG.md         # Stack-wide release changes and tracker impact
    references/
      filesystem.md       # Default local issues, format, identity and operations
      github.md           # Issues/PR operations, mappings and merge confirmation
      github-labels.json  # GitHub label names and display metadata
    assets/
      issue.md            # Markdown issue template with YAML frontmatter
    scripts/
      github/
        setup-labels.mjs  # Check, preview or apply the GitHub label mapping
```

Phase references are not additional skills. The memory reference defines
target-project behavior; root `MEMORY.md` records development of mstack itself.

## Optional integrations

Runtime and delegation integrations belong to orchestrator; filesystem and
GitHub integrations belong to tracker. Workflow points to tracker for its
default issue storage; tracker owns selection and concrete operations. An
integration is a bounded set of instructions for using a concrete
system. It need not be a
deterministic program. A skill may have or use no integration where its
capabilities allow.

The owning skill defines the general integration contract and handles
availability/failure policy. Concrete references describe that system's actual
operations and constraints. The current request and project instructions define
role scope; workflow supplies validation standards for every actor.
Tracker defines missing-access and partial-operation handling; orchestrator
defines capability checks and execution controls. The shared release and
installed sibling layout are defined in `AGENTS.md` and `README.md`; actual
packaging validation and complete runtime/harness mappings remain pending.

Current review context and unresolved design questions live in
[MEMORY.md](../MEMORY.md).
