# Temporary draft review

This temporary authoring note records decisions and remaining validation work;
it is not an mstack product contract. The shared behavior decisions are in
`Resolved during this review`. Remaining `Validation pending:` markers mean a
runtime or filesystem-lock behavior lacks direct validation; they do not
identify an open product decision. Clear a marker when evidence is obtained, or
record a deliberate deferral in project memory. Remove this queue when its
validation items are completed or deliberately deferred with a clear home in
project memory.

## Settled decisions to preserve

- An autonomous controller session should close with a durable checkpoint.
- A later autonomous session starts fresh and makes its decision from recorded
  project state in GitHub or local project files. Do not rely on an open runtime
  pane or harness session to recover what happened.
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

## Validation queue

Review these together when planning a supervised validation pass. Ask for a
product decision only if the evidence exposes an actual contract gap.

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
  states that hard halt needs harness-specific process-tree proof. It is the
  supported v1 runtime based on its published CLI documentation; no live trial
  has been run.
- [Controller admission](../skills/mstack-orchestrator/SKILL.md) requires
  launcher/scheduler exclusivity and disallows concurrent controller sessions.
  A fresh session may take over in recovery-only mode after the prior controller
  ends, while dispatch and claim reassignment wait for worker reconciliation.
  The shared behavioral protocol leaves enforcement to the runtime mapping.
  Runtime-specific enforcement remains a validation item: the launcher must
  deny concurrent admission, permit recovery-only takeover only after the old
  controller ends, and retain admission through checkpointing.
- Cursor Cloud and native Cursor subagents are deferred and unsupported in
  mstack 0.1.0. Their reference files preserve researched behavior for a future
  release and must not be selected in this release.
- [Filesystem tracker](../skills/mstack-tracker/references/filesystem.md) is
  instruction-only by design and now requires a short-lived lock scoped to the
  issue file being changed. It is released after the confirmed write; a crash
  must not leave it locked indefinitely. It does not lock the project or
  controller session, and the skill does not provide a lock implementation.
- [Cross-skill references](../skills/mstack-tracker/SKILL.md) resolve relative
  to the skill directory. Releases contain all three version-matched skill
  directories as siblings; if a required sibling is missing, stop the dependent
  operation rather than searching for another version.
- [GitHub tracker](../skills/mstack-tracker/references/github.md) deliberately
  excludes automatic legacy-label migration, offline cache/export tooling and
  GitHub Projects fields. Existing record migration requires an explicit
  decision; an export does not replace a current read.
- The fx CLI mapping follows the published `fx ask` contract for its flags,
  structured output and exit handling. Runtime-level evaluation is follow-up
  work; native fx subagents are unsupported.

## Open items

### Validation items

1. [Autonomous session behavior](../skills/mstack-orchestrator/SKILL.md) —
   run supervised runtime trials that meet the observable pass criteria in the
   skill: actual dispatch after phase handoff, worker wait and a blocked story
   with other eligible work; a configured controlled stop or pre-limit guard
   followed by worker reconciliation and a durable checkpoint; then a fresh
   invocation from current records. This does not assume cleanup after an
   abrupt hard limit. A reported next step alone does not pass.
2. [Controller admission](../skills/mstack-orchestrator/references/configuration.md) —
   retain one-controller-per-project and recovery-only behavior. Runtime
   enforcement remains an external launcher/scheduler responsibility; the
   contract does not promise that Herdr supplies project-level admission.
3. [Filesystem tracker](../skills/mstack-tracker/references/filesystem.md) —
   validate the selected lock provider's file-scoped lock, release after a
   confirmed write, and recovery after owner crash. Exercise overlapping
   updates and worker proposals against current records, confirming that the
   latest record is reconciled and no competing write is lost. If real logs
   show the short-lived lock is insufficient, tighten the rule based on that
   evidence.

Cursor Cloud, Cursor native subagents and cmux are excluded from 0.1.0 rather
than pending release validation. The package has no installer requirement.
Herdr's documented operation mapping is accepted for v1 without a live trial;
autonomous-session and filesystem-lock reliability belong in the follow-up
evaluation story.

## Current status

Versioning and per-skill changelogs are documented in `AGENTS.md` and the three
skill changelogs. They are not part of this draft-gap queue.
