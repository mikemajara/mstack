# mstack-workflow changelog

Each entry describes the mStack release across all three skills and its effect
on workflow. The `version` in this skill's frontmatter is the shared mStack
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
  acceptance scenarios are rerun if the tested commit changes. Lifecycle's
  phase overview reflects the implementation handoff gates. The
  shared Workflow skill summary points to the remaining Verification limits.
- **Orchestrator:** Coordinates existing stories through workflow phases,
  continues eligible work during an active autonomous session, and closes each
  session with project state recorded for a later fresh decision. If worker
  termination or outcome is uncertain, it retains ownership and pauses
  dispatch pending reconciliation. A fresh controller may take over in
  recovery-only mode. Deadlines and halts stop dispatch while the controller
  reconciles and checkpoints; this does not change workflow phase rules.
  Herdr is the supported runtime for 0.1.0, and fx `ask` is a documented
  harness mapping used through a supported runtime. Cursor Cloud and native
  Cursor subagents are deferred; cmux is excluded. These runtime-scope choices
  do not change workflow phase gates.
- **Tracker:** Records specifications, workflow facts, evidence and links;
  supports filesystem issues by default and optional GitHub integration. For
  filesystem tracking, the controller writes records during a coordinated run;
  workers return proposed changes. Standalone mutations use a short-lived lock
  scoped to the changed issue file; no lock implementation is bundled. GitHub
  integration excludes automatic legacy migration and offline caches; neither
  changes workflow gates.
- **Release layout:** The shared-version release contains all three named skill
  directories as siblings. Cross-skill links resolve relative to their owning
  skill; a missing required sibling makes the dependent operation unavailable.

### Impact on workflow

- Orchestrator must follow workflow's phase gates and lifecycle when dispatching
  and reconciling work. Its session checkpoint rule adds no phase or transition;
  no workflow adaptation is required for this release.
- Workflow rules can be used without the sibling skills. Operations that rely
  on tracker storage or orchestration require the matching same-version sibling;
  a missing link does not change the phase rules.
- Controller-only filesystem writes do not change phase ownership or validation;
  workflow results still require the same gates before the controller records them.
- Filesystem per-file write locks serialize record mutations but do not change
  workflow gates or require workflow adaptation.
- GitHub legacy label migrations remain explicit user decisions, and cached or
  exported records do not establish current workflow state.
- Tracker must record workflow facts and evidence without treating tool status
  as proof that a gate passed. No separate workflow adaptation is required.
- Orchestrator must apply the lifecycle transition and blocked-work rules; the
  diagram clarification adds no new gate beyond phase contracts.
- Orchestrator uses workflow's priority vocabulary when ordering eligible
  backlog stories; workflow's phase readiness rules remain unchanged.
- Orchestrator's pending runtime validation must observe real dispatch across
  phase handoffs and holds, then reconcile and checkpoint before a controlled
  runtime stop; this does not change workflow phase criteria. No recovery after
  an abrupt hard limit is promised.
- Cursor Cloud and native Cursor subagents are unsupported in 0.1.0; cmux is
  excluded. No workflow adaptation is required for these deferred integrations.
- fx CLI output and Herdr observations remain execution evidence, not proof a
  workflow gate passed. Workflow's existing phase criteria apply unchanged.
- Orchestrator's elapsed-time limit is per session; workflow has no cumulative
  scheduled-session time gate.
- No universal rolling usage threshold is defined. Any guard is
  runtime-specific and does not change workflow gates; unsupported requested
  guards stop dispatch but not phase-validity rules.
- Exclusive controller admission is an orchestrator runtime policy; it does
  not change workflow phase validity or transitions.
- An uncertain worker retains ownership while dispatch and claim changes pause
  pending reconciliation; workflow state changes still require workflow's own
  gates.
- Herdr's unverified hard-cancellation limit does not change workflow's blocked
  or completion criteria.
- Herdr model launch flags affect orchestrator dispatch only; workflow's
  assignment and phase requirements remain unchanged.
- Orchestrator must give workers the current refinement specification, including
  relevant constraints. No separate orchestration rule or tracker schema is
  required by this addition.
- Orchestrator dispatches QA only after implementation checks pass or the PR
  states that no applicable runnable checks exist. Tracker preserves these
  results with the PR record; no new tracker field is required.
- Use all three skills at `0.1.0` when coordinating a complete mStack run.
