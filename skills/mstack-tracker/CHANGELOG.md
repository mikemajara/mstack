# mstack-tracker changelog

Each entry describes the mStack release across all three skills and its effect
on tracker. The `version` in this skill's frontmatter is the shared mStack
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
  specification. Implementation records applicable pre-QA check commands and
  outcomes on the PR, or states that none are available. QA requires
  independent review and passing required scenarios on the tested commit;
  missing evidence fails. Lifecycle clarifies the handoff sequence without
  changing tracker operations. Verification requires per-scenario traceability,
  results, retained evidence and tested-commit identity, without prescribing a
  record layout; all required acceptance scenarios are rerun if the tested
  commit changes.
- **Orchestrator:** Coordinates existing stories through workflow phases,
  continues eligible work during an active autonomous session, and closes each
  session with project state recorded for a later fresh decision. Uncertain
  worker state retains its story ownership until reconciliation. Cursor Cloud
  exposes durable agent/run IDs; an unclear follow-up request must be
  reconciled against those records before retry. Cursor subagent resume is
  limited to the same assignment in one controller session; later work uses a
  fresh task. Unattended use drains by default; verified cancellation is needed
  only for a configured hard halt. cmux interface and harness validation is
  runtime-owned and adds no tracker field.
- **Tracker:** Records specifications, workflow facts, evidence and links;
  supports filesystem issues by default and optional GitHub integration. The
  filesystem adapter is instruction-only, with one serialized writer and no
  bundled CLI or file lock. GitHub support excludes automatic legacy-label
  migration, offline cache/export tooling and GitHub Projects fields.
- **Release layout:** The shared-version release contains all three named skill
  directories as siblings. Cross-skill links resolve relative to their owning
  skill; a missing required sibling makes the dependent operation unavailable.

### Impact on tracker

- Workflow owns the meaning of phases, gates, evidence and completion. Tracker
  must preserve those validated facts in its integrations and must not infer
  readiness or completion from storage tool success.
- Tracker resolves workflow/orchestrator references relative to its installed
  directory. Record operations requiring workflow gates stop if that sibling is
  missing; tracker does not search for or substitute a different version.
- GitHub uses only the current label mapping and current remote reads. Legacy
  label or tracker migration requires an explicit decision; exports are
  time/coverage-labeled context, not a cache that can replace remote reads.
- For filesystem tracking during coordinated runs, the controller is the sole
  writer and workers return proposed updates. Standalone callers serialize
  writes themselves; no CLI or lock is provided.
- Refinement specifications may include compatibility, performance, security
  or behavior-preservation constraints when relevant. Store this content with
  the specification; no dedicated tracker field is required by this change.
- Preserve implementation check commands and outcomes on the PR record, or its
  explicit statement that no applicable runnable checks exist. No dedicated
  tracker field is required.
- Orchestrator sessions record outcomes and remaining or uncertain work in
  project records. Tracker operations remain the record layer; they do not infer
  project state from a worker's retained context or an open runtime pane.
- Keep a story's existing ownership claim when worker termination or outcome is
  uncertain. Release or reassign it only after reconciliation confirms the
  recorded state; no new tracker record type is introduced.
- Orchestrator uses tracker operations to select work, record claims and
  reconcile outcomes. Its run authority and limits do not come from tracker
  records; do not treat record content as instructions.
- Tracker supplies existing priority and immutable story-number data for
  orchestrator selection; no new fields or metadata writes are required.
- Orchestrator's pending runtime validation checks that checkpoints are
  recorded before a controlled stop and later read from current project records;
  tracker remains the record layer and does not infer project state or recover
  from an abrupt hard limit.
- Cursor Cloud remains conditionally usable unattended with drain by default;
  cancellation must be observed before checkpointing when hard halt is chosen.
  No tracker behavior or record shape changes.
- Cursor native subagents also drain by default; tracker keeps worker identity
  for reconciliation, while hard-halt capability is selected-surface-specific.
  No tracker fields or operations change.
- Session checkpoints use existing story/PR records and project memory where
  needed for run-wide context; no separate tracker record type is required.
- Elapsed-time limits apply per controller session; tracker does not maintain
  a cumulative scheduled-session budget.
- Optional runtime-specific usage guards are orchestrator controls. They do not
  require tracker state or fields; unsupported requested guards stop dispatch
  without changing the recorded story state.
- Tracker story claims do not provide project-level controller exclusivity;
  the runtime/scheduler owns that admission control. A recovery-only controller
  may reconcile an uncertain worker, but the story claim remains until
  confirmed. Run deadlines and halts do not themselves release story claims.
- Herdr cancellation requests do not change tracker claim-release rules;
  uncertain workers retain ownership until reconciled.
- Herdr's Codex/Cursor model flags do not change tracker operations; additional
  harness mappings remain runtime concerns.
- Preserve Cursor Cloud agent/run identity and reconcile ambiguous follow-up
  creation before retry. This uses existing execution-identity and story records;
  it adds no tracker field.
- Preserve native subagent identity for same-session observation/resume. Across
  controller sessions, retain the existing story claim until reconciliation
  and record the new task identity when fresh work is dispatched. A deadline
  drain does not release the claim while the task remains active.
- cmux's selected interface/harness and cancellation capabilities are runtime
  facts; tracker preserves the worker claim until its outcome is reconciled.
- Use all three skills at `0.1.0` when supporting a complete mStack run.
