# mstack structure

Status: **three abstractions agreed; detailed contents not yet implemented**.
Owner: Miguel. Authoring specification, not distributed skill instructions.

## Agreed package

~~~text
mstack/
  workflow/
    SKILL.md
    references/
      implementation.md
      qa.md
      memory.md
      ...
  orchestrator/
    SKILL.md
    references/
      herdr.md
      ...
  tracker/
    SKILL.md
    references/
      github.md
      ...
~~~

Only planning documents and legacy/backlog currently exist. The earlier four
skills are superseded: no extra mstack router, backlog or delivery skill.
Phase procedures are workflow references, not separate skills.

| Skill | Owns | Must not own |
| --- | --- | --- |
| workflow | Phase specifications, prerequisites, validation/evidence criteria, valid transitions, issue shaping and default project memory | Tracker representations, runtime tools, budgets or execution authority |
| orchestrator | Automated execution: selection, admission, dispatch, limits, permissions, waiting, cancellation, reconciliation and reporting | A competing lifecycle, phase craft or weaker validation |
| tracker | Capture/read/update work specifications, validated workflow facts, evidence and relationships through its adapter | Product requirements, readiness rules or completion inferred solely from a tool status |

A human can follow workflow without automation. The orchestrator reads the
workflow and uses deterministic tool operations through the selected adapter.
The tracker represents validated facts; changing a field does not prove them.

## Workflow: content to draft

SKILL.md provides the process overview, essential rules and conditional
reference routes. Reading project guidance and relevant memory, and capturing
material new information, is default behavior, not an optional final task.

Each phase reference specifies entry prerequisites, procedure/artifacts,
validation criteria, exit evidence and valid next phases. The core defines
the contract; the work item supplies concrete scope, AC and scenarios.

| Reference topic | Content and output |
| --- | --- |
| Lifecycle / intake | Actor-free flow and guards, capture/dedupe, initial classification, rework and blocked/resume paths |
| Research / refinement / specification | Sourced facts, explicit unknowns, resolved product decisions, scoped AC and executable verification brief |
| implementation.md | Execute locked scope; produce a reviewable change and self-check evidence |
| qa.md | Prescribed behavioral verification, expected/observed results, sufficient evidence, tested change/spec identity and verdict |
| Integration | Proof the verified change was integrated; distinct from implementation, QA and deployment |
| Data migrations | Traceable artifacts, named generation/application steps, prerequisites, safe rehearsal and pending-operation handoff |
| memory.md | Default project-memory read/capture; distinction from AGENTS.md and tracker truth; deliberate promotion and compaction |

Topics beyond explicitly named paths remain a drafting inventory, not a
requirement to create one file per row or load every reference at once.

### Completion and verification

- Implementation done means ready for QA, not QA passed, merged or deployed.
- Every stage has explicit entry prerequisites and exit validation criteria.
- Criteria are the same for humans, agents and all harnesses.
- Before implementation, a work item has goal/scope, testable AC and executable
  scenarios with prerequisites: enough to judge the change without chat history.
- Verification maps criteria to scenarios, expected/observed results, retained
  evidence and identified change/specification versions.
- Missing required evidence blocks. Code defects return to implementation;
  specification holes return to refinement.
- Stale or replayed verdicts are not current proof. Metadata alone does not
  establish scenario relevance or correct behavior.
- Integration and deployment are separate facts. A tracker's completion mapping
  reflects validated milestones rather than redefining them.

Exact phase names, generic evidence schema and labels still need drafting.
There are no human-versus-agent strictness levels. Existing prerequisite
evidence may avoid repeated work, not waive the criteria.

### Default memory

Project memory defaults to MEMORY.md, with an explicit project override if
needed. Read relevant existing entries alongside AGENTS.md. Capture material
decisions, observations, open questions and gotchas not already in lasting
guidance; create the file when there is information worth retaining.

Memory is a concise log, not a transcript, second editable backlog or substitute
for current verification. Distinguish facts from proposals, preserve meaningful
entries during conflicts and exclude secrets. Promote lasting guidance to
AGENTS.md deliberately; keep phase procedures in workflow references.
[This repository's memory](../MEMORY.md) is active now; the reusable workflow
memory instructions are not yet authored.

## Orchestrator and tracker

| Item | Content |
| --- | --- |
| orchestrator/SKILL.md | Execution loop, runtime limits/permissions, capability checks, worker briefs and result handling; consult workflow contracts |
| orchestrator/references/herdr.md | Start/wait/stop, pane/session identity, isolation/reuse and reconciliation mechanics only |
| Other automation bindings / outcome protocol | Cursor operations when implemented; receipt/identity/cancellation mechanics as needed, not another definition of phase success |
| tracker/SKILL.md | Work-record and evidence operations, adapter selection and failure handling |
| tracker/references/github.md | Concrete field/label/relationship mappings and operations. For the discussed mapping, close after QA passes and integration completes, not at implementation completion |
| File or other tracker bindings | Concrete storage mappings when implemented, not additional workflows |

Runtime/project policy supplies tools, duration, models, concurrency and
permissions. Safe disposable local/test checks are permitted; a preview is not
safe merely because it is a preview. Reserved migration-generation/application
actions remain human-gated. These are permissions, not alternate QA standards.

## Authoring and migration boundaries

- Skills and references contain actionable instructions, not our dialogue,
  personal setup history, migration rationale or future-adapter aspirations.
  Planning and continuity files stay outside installed skill content.
- Concrete adapters document actual operations and mappings only. The general
  adapter contract belongs to the owning skill, not every concrete adapter.
- Keep legacy/backlog unchanged. Reuse behavior through small reviewed moves.
  Legacy research/refine/implementation/QA/context/memory procedures inform
  workflow; run mechanics inform orchestrator; tracker helpers inform tracker.
  Mixed files must be separated rather than copied wholesale.
- Runtime and its proof regressions stay in server-docker until an explicit
  ownership/cutover decision. Do not recreate it here.
- Preserve applicable source/license attribution for reused pstack material.
- This capture does not install skills, remove the original backlog, merge PRs,
  change labels, enable timers, run workers or deploy changes.

Continue from [HANDOFF.md](HANDOFF.md): draft and review the actor-free phase
contracts and default memory procedure before bulk-authoring adapters or tools.
