# mstack-orchestrator changelog

Each entry describes the mStack release across all three skills and its effect
on orchestrator. The `version` in this skill's frontmatter is the shared mStack
version.

## 0.1.0

Initial versioned baseline for the three-skill mStack set. There is no earlier
version to migrate from. The skills remain drafts; see their contracts for
known limitations.

### Stack changes

- **Workflow:** Defines story phases, lifecycle, readiness and completion
  requirements, verification and project memory. Research findings can support
  an authorized non-delivery disposition under the lifecycle contract.
  Refinement records relevant implementation and judgment constraints in the
  specification. Implementation runs applicable project checks before QA and
  records their commands and outcomes. QA requires independent review and
  passing required scenarios on the tested commit; missing evidence fails.
  Verification requires per-scenario traceability, results, retained evidence
  and tested-commit identity, without prescribing a record layout; all required
  acceptance scenarios are rerun if the tested commit changes.
  Lifecycle's phase overview reflects the implementation handoff gates.
- **Orchestrator:** Coordinates existing stories through workflow phases,
  continues eligible work during an active autonomous session, and closes each
  session with project state recorded for a later fresh decision. Backlog runs
  honor explicit request order, then priority and an immutable-number
  tie-breaker. Validation requires observed dispatch after phase handoffs,
  worker waits, eligible-story changes, controlled runtime stops or pre-limit
  guards, and checkpoint-based fresh starts; no live trial has yet been run.
  The trial requires reconciliation and checkpointing before the controlled
  stop, without promising recovery after an abrupt hard limit. Session
  checkpoints reconcile canonical story,
  PR and existing project-memory records rather than create a separate state
  store. Only one controller session may run per project; the launcher/scheduler
  must enforce exclusive admission. After a controller ends, a fresh controller
  may take over in recovery-only mode while workers remain uncertain; dispatch
  and claim reassignment stay paused until reconciliation.
  `maxDurationMinutes` applies per session and does not accumulate across
  scheduled sessions. No universal rolling usage threshold is set; 90% of a
  five-hour window is an example. Optional guards are runtime-specific and
  require a defined signal, units, window, polling cadence, threshold and stop
  effect; unsupported requested guards block dispatch. Deadlines and halts close work dispatch while the controller retains
  admission through reconciliation and checkpointing. Herdr's documented `ctrl+c` is a cancellation request, not confirmed
  process-tree termination; hard cancellation requires harness-specific proof,
  and project exclusivity must come from the launcher/scheduler. Herdr model
  flags are documented for Codex and Cursor; other harnesses still need mapping.
  Cursor Cloud v1 is public beta and is conditionally usable unattended with
  `drain` by default; hard halt requires confirmed cancellation and terminal
  observation before checkpointing. Reconcile ambiguous follow-up requests
  from agent/run records before retry; its follow-up endpoint has no
  caller-supplied run ID, and cancellation has no documented latency guarantee.
  Cursor native subagents resume only for the same assignment within one
  controller session; a later session starts a fresh task from project records.
  The exposed task schema must be inspected. Unattended use follows `drain`;
  deadline `halt` requires cancellation and stop confirmation from the selected
  native task surface. The separate SDK does not establish that capability, and
  live validation remains pending. cmux keeps
  separate macOS and TUI mappings; select and validate the installed
  interface/harness pairing. Default to `drain`; hard halt requires verified
  stop and confirmation.
  When worker state is uncertain, retain ownership, record a reconciliation
  condition and do not dispatch a replacement. Runtime-specific detection and
  recovery-only admission remain open.
- **Tracker:** Records specifications, workflow facts, evidence and links;
  supports filesystem issues by default and optional GitHub integration. The
  filesystem adapter is instruction-only: the controller writes records in a
  coordinated run, workers return proposed updates, and standalone use requires
  one serialized writer; no CLI or lock is supplied. GitHub integration does
  not migrate legacy labels or provide offline cache/export or Projects-field
  mappings; current reads remain authoritative.
- **Release layout:** The shared-version release contains all three named skill
  directories as siblings. Cross-skill links resolve relative to their owning
  skill; a missing required sibling makes the dependent operation unavailable.

### Impact on orchestrator

- Orchestrator depends on workflow's current phase, validation and completion
  contracts; follow those contracts at every handoff and reconciliation.
- Orchestrator requires the same-release workflow and tracker siblings for its
  linked contracts; it must stop a dependent operation if either is missing.
- With filesystem tracking, the controller is the sole record writer during a
  coordinated run. Workers return proposed updates; if exclusive controller or
  single-writer operation cannot be ensured, stop tracker mutations.
- GitHub issue attributes use the current label mapping. Do not infer or
  automatically migrate legacy labels; read current remote records rather than
  relying on offline exports.
- For backlog selection, preserve explicit request order; otherwise use
  workflow priority and ascending immutable story number without changing
  tracker metadata.
- Elapsed-time admission is session-scoped. An optional runtime-specific usage
  guard must define its signal, units, window, polling cadence, threshold and
  stop effect; unsupported requested guards block dispatch. No universal 90%
  threshold is imposed.
- The scheduler/runtime must enforce one controller per project. It may admit
  a fresh recovery-only controller after the previous controller ends while
  workers remain active or uncertain, but must hold dispatch and claim changes
  pending reconciliation. Story claims remain the tracker's responsibility.
- Cursor Cloud continuation carries forward the agent's conversation and
  workspace. A new controller makes its decision from project records and uses
  a fresh agent after reconciling the prior one; it does not continue the old
  agent conversation across controller sessions.
- Cursor native subagent resume is likewise limited to the same assignment in
  the current controller session; cross-session work starts a fresh task from
  project records. Runtime-specific task cancellation remains unverified.
- Refinement specifications include relevant constraints that affect the work.
  Pass the current specification and verification scenarios to each worker.
- Start independent QA only when the implementation checks pass and their
  commands and outcomes are recorded, or the PR states that no applicable
  runnable checks exist. This does not replace the QA acceptance verdict.
- Each controller session closes with project state recorded. A later session
  reads GitHub or local project records and decides afresh; it does not recover
  project truth from an open pane or retained controller context.
- If worker termination or outcome is uncertain, retain ownership and pause
  dispatch and claim changes. A fresh controller may take exclusive controller
  admission to reconcile current records and runtime state; do not start
  replacement work while the worker may still be active.
- Orchestrator uses tracker for issue and evidence operations. Tracker records
  outcomes but does not determine workflow readiness or grant execution scope.
- Use all three skills at `0.1.0` when coordinating a complete mStack run.
