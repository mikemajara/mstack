# Temporary draft review

This temporary authoring note records decisions and remaining validation work;
it is not an mStack product contract. The shared behavior decisions are in
`Resolved during this review`. The remaining `Draft:` markers identify runtime
or packaging behavior without direct validation. Remove or rewrite each marker
when its evidence is obtained, or record a deliberate deferral in project
memory. Remove this queue when its validation items are completed or deliberately
deferred with a clear home in project memory.

## Settled decisions to preserve

- An autonomous controller session should close with a durable checkpoint.
- A later autonomous session starts fresh and makes its decision from recorded
  project state in GitHub or local project files. Do not rely on an open Herdr,
  cmux or harness pane to recover what happened.
- Treat GitHub or local project records as the state of the project. Do not
  assume workers retain current context across autonomous runs; inspect current
  recorded state before continuing work.
- `run.maxDurationMinutes` applies to each controller session independently;
  it does not accumulate across scheduled sessions. There is no universal
  rolling usage threshold; 90% of five hours is an example, not a default.
- A context/usage guard is optional and runtime-specific. Its integration must
  define signal, units, window, polling cadence, threshold and stop effect; if a
  requested guard is unsupported, dispatch is refused. There is no shared guard
  field or 90% default.
- If a worker's outcome is uncertain, do not dispatch replacement work before
  reconciliation. Once the prior controller has ended, a fresh controller may
  take exclusive controller admission in recovery-only mode to inspect,
  observe/cancel and checkpoint; it may not dispatch, continue the uncertain
  worker or reassign the claim until reconciliation.
- Across controller-session boundaries, do not continue a previous worker's
  conversation. After reconciling its outcome, start new work with a fresh
  worker from current project records and the intended branch/workspace.
- A session checkpoint reconciles canonical story/PR records and existing
  project memory; it is not a separate controller state store. It records
  confirmed per-story phase/outcome/PR/worker ownership and run-wide stop reason,
  scope, remaining work and uncertain-worker reconciliation condition where
  those facts are not already represented.

## Discussion queue

Work through one marker at a time. Each item identifies the file with the
marker and the decision needed before changing or removing it.

## Resolved during this review

- [Configuration](../skills/mstack-orchestrator/references/configuration.md)
  applies `run.maxDurationMinutes` independently to each controller session;
  elapsed time does not accumulate across scheduled sessions.

- [Research phase](../skills/mstack-workflow/references/phases/research.md)
  keeps two required gates: establish the requested problem and current facts,
  then determine whether they support a feasible, testable specification.
  Product choices may continue into refinement; non-delivery closure follows
  lifecycle and remains subject to authorization.
- [Refinement phase](../skills/mstack-workflow/references/phases/refinement.md)
  requires constraints affecting implementation or judgment to appear in scope,
  acceptance criteria or verification scenarios, with an explicit none-applies
  statement when relevant constraints are absent. Example constraint types are
  prompts, not a fixed checklist.
- [Implementation phase](../skills/mstack-workflow/references/phases/implementation.md)
  requires applicable project checks to pass and their commands and outcomes to
  be recorded before QA. If no applicable runnable checks exist, the PR states
  that. These checks do not substitute for independent QA.
- [QA phase](../skills/mstack-workflow/references/phases/qa.md) keeps two gates:
  independent QA, and passing required scenarios on the tested commit with
  evidence. Detailed evidence format belongs to the separate verification
  review; missing required evidence fails the QA gate.
- [Workflow lifecycle](../skills/mstack-workflow/references/lifecycle.md)
  documents the phase entry/transition model and blocked-work rules. Its
  diagram now states that implementation gates must pass and independent QA
  must be assigned before entering QA; phase references supply gate detail.
- [Workflow skill](../skills/mstack-workflow/SKILL.md) has no blanket draft
  notice: its phase readiness and transitions are owned by the phase and
  Lifecycle contracts, while Verification's requirements are recorded in its
  own reference.
- [Verification](../skills/mstack-workflow/references/verification.md) requires
  per-scenario traceability and execution facts, while leaving record layout to
  the project. It states the record-completeness checks and requires all
  acceptance scenarios to be rerun after the tested commit changes before merge.
- [Orchestrator selection](../skills/mstack-orchestrator/SKILL.md) respects
  explicit request order; otherwise unclaimed eligible stories rank by high,
  medium, low, then missing priority, with ascending immutable story number as
  the tie-breaker. Selection does not modify priority metadata.
- [Checkpoint boundary](../skills/mstack-orchestrator/SKILL.md) is defined as
  reconciling canonical story/PR records and existing project memory, with no
  separate controller state store. When worker termination or outcome cannot
  be confirmed, ownership remains held and replacement work is not dispatched
  until reconciliation. A fresh controller may take over in recovery-only mode
  after the previous controller ends; runtime detection and enforcement remain
  open.
- [Run controls](../skills/mstack-orchestrator/references/configuration.md)
  apply `maxDurationMinutes` per controller session; no time budget accumulates
  across scheduled sessions. There is no universal rolling usage threshold;
  90% of five hours is an example, not a default.
- [Optional usage guards](../skills/mstack-orchestrator/references/configuration.md)
  are runtime-specific, off by default and allowed only when their adapter
  defines signal, units, window, polling cadence, threshold and stop effect.
  An unsupported requested guard blocks dispatch.
- [Herdr integration](../skills/mstack-orchestrator/references/herdr.md) maps
  Codex and Cursor model flags, leaves other harnesses unsupported until
  verified, requires project exclusivity from the external scheduler, and
  states that hard halt needs harness-specific process-tree proof.
- [Controller admission](../skills/mstack-orchestrator/SKILL.md) requires
  launcher/scheduler exclusivity and disallows concurrent controller sessions.
  A fresh session may take over in recovery-only mode after the prior controller
  ends, while dispatch and claim reassignment wait for worker reconciliation.
  The shared behavioral protocol is sufficient; no shared lock API is required.
  Runtime-specific enforcement remains a validation item: the launcher must
  deny concurrent admission, permit recovery-only takeover only after the old
  controller ends, and retain admission through checkpointing.
- [Cursor Cloud](../skills/mstack-orchestrator/references/cursor-cloud.md)
  follows the cross-session worker boundary: a later controller observes or
  cancels the previous agent as needed, reconciles its outcome, then uses a
  fresh agent. The API remains public beta and has not had a live runtime trial.
- [Cursor native subagents](../skills/mstack-orchestrator/references/cursor-subagents.md)
  use the actual exposed task schema rather than promising fixed argument
  names; task resume is restricted to the same assignment in one controller
  session. Unattended use follows the shared default `drain` behavior;
  `onDeadline: halt` requires verified cancellation support.
- [Filesystem tracker](../skills/mstack-tracker/references/filesystem.md) is
  instruction-only by design. The controller is sole writer during coordinated
  runs; workers return proposed updates. Standalone use requires one writer and
  serialized writes; no shared lock protocol or bundled file lock is currently
  defined.
- [Cross-skill references](../skills/mstack-tracker/SKILL.md) resolve relative
  to the skill directory. Releases contain all three version-matched skill
  directories as siblings; if a required sibling is missing, stop the dependent
  operation rather than searching for another version.
- [GitHub tracker](../skills/mstack-tracker/references/github.md) deliberately
  excludes automatic legacy-label migration, offline cache/export tooling and
  GitHub Projects fields. Existing record migration requires an explicit
  decision; an export does not replace a current read.
- [cmux](../skills/mstack-orchestrator/references/cmux.md) conditionally
  supports either its macOS CLI or TUI, selected explicitly and checked against
  the installed interface and harness. Default to `drain`; hard halt requires
  a verified stop and confirmation path.

## Open items

### Product decision to confirm

- **Filesystem write coordination:** The current instruction-only contract
  assigns serialization to the caller and stops when a single writer cannot be
  ensured. My recommendation is to keep that boundary rather than prescribe a
  shared lock protocol, because the tracker supplies no executable filesystem
  operations. Confirm whether caller-owned serialization is sufficient.

### Validation items

1. [Autonomous session behavior](../skills/mstack-orchestrator/SKILL.md) —
   run supervised runtime trials that meet the observable pass criteria in the
   skill: actual dispatch after phase handoff, worker wait and a blocked story
   with other eligible work; a configured controlled stop or pre-limit guard
   followed by worker reconciliation and a durable checkpoint; then a fresh
   invocation from current records. This does not assume cleanup after an
   abrupt hard limit. A reported next step alone does not pass.
2. [Configuration](../skills/mstack-orchestrator/references/configuration.md) —
   validate that the external launcher/scheduler prevents concurrent
   controllers, refuses takeover while a prior controller may still be active,
   and permits recovery-only takeover after it ends while workers remain active
   or uncertain.
3. [Cursor Cloud mapping](../skills/mstack-orchestrator/references/cursor-cloud.md) —
   validate conditional unattended use in a live runtime trial: default drain,
   configured cancellation and terminal observation before checkpointing,
   ambiguous follow-up reconciliation, recovery-only observation/cancellation,
   and fresh-agent dispatch after a controller-session boundary. The API is
   public beta and documents no cancellation-latency guarantee.
4. [Cursor subagent mapping](../skills/mstack-orchestrator/references/cursor-subagents.md) —
   validate conditional unattended behavior on the selected Cursor surface:
   completion observation, `drain` behavior, cancellation and stop confirmation
   when exposed, refusal of unsupported hard halt, and checkpoint handling
   when task state is uncertain. The separate Cursor SDK does not establish
   native Task tool capabilities.
5. [cmux mapping](../skills/mstack-orchestrator/references/cmux.md) —
   live-validate the selected installed interface/harness pairing for launch
   readiness, observation, stopping and process cleanup. The macOS CLI and TUI
   remain separate mappings.
6. [Herdr mapping](../skills/mstack-orchestrator/references/herdr.md) —
   live-validate worker observation and cleanup on the selected harness. A
   `ctrl+c` request alone does not prove termination; hard halt requires a
   verified process-tree stop and confirmation. Confirm `drain` remains usable
   when that capability is absent.
7. [Tracker skill](../skills/mstack-tracker/SKILL.md) — validate the documented
   sibling layout when packaging/installation is implemented; no installer has
   yet been validated.
8. [Filesystem tracker](../skills/mstack-tracker/references/filesystem.md) —
   live-validate overlapping write requests: controller-only authoritative
   writes, worker proposals reconciled against current records, and refusal
   without a competing issue write when single-writer control is unavailable.

## Current status

Versioning and per-skill changelogs are documented in `AGENTS.md` and the three
skill changelogs. They are not part of this draft-gap queue.
