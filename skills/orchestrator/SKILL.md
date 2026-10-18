---
name: orchestrator
description: Coordinate software work execution, including autonomous runs, dispatch, runtime limits and permissions. Use when asked to orchestrate work through workflow phases.
---

# Orchestrator

Draft: runtime/harness mappings and durable run-state storage are incomplete.
Concrete limitations are marked in the integration references.

Follow workflow phase contracts for readiness, validation and transitions.
Own selection, admission, dispatch, limits, permissions, waiting, cancellation,
reconciliation and reporting. Execution mode does not weaken workflow gates.
Receive recommendations for decisions within the authority granted by project
policy and the current request.

Use [tracker](../tracker/SKILL.md) for work-record operations. Read the selected
[workflow phase contract](../workflow/SKILL.md#phase-contracts) for prerequisites,
required outputs and validation. This skill supplies execution policy and
mechanics; configuration does not change workflow requirements.

## Project configuration

Read [configuration](references/configuration.md) and the target project's
`.mstack/config.json` before dispatch. Resolve runtime, agent harness, role,
model tier, configured model candidates and run limits. Explicit run
instructions override project settings; project settings override skill
defaults. Configuration supplies execution choices within granted authority.

Select and launch configured models automatically, including unattended runs.
Resolve the role's model tier through its selected harness. Use configured
fallbacks when needed; do not invent a model, cross-tier escalation or account
switch. Verify runtime/harness/model compatibility before starting work.

## Execution integration contract

Runtime means the execution mechanism, such as Herdr, Cursor Cloud or cmux.
Agent harness means the client running the agent, such as `agent-cursor`,
`codex` or `fx`. Model tiers are project-configurable selections within those
harnesses. A runtime may constrain which harnesses it supports. Cursor native
subagents are a capability of a Cursor session, whose operation mapping must
be supplied for the execution context in use.

An integration declares supported combinations and maps the following
operations to available tools. Shared operation names do not imply that every
runtime supports every operation.

| Operation | Required result |
| --- | --- |
| Inspect | Identify the target runtime and existing workers; report available launch, model-selection, observation, cancellation and isolation capabilities. |
| Launch | Start the admitted task with its resolved harness/model and workspace; return the actual worker/session identity and startup outcome. Record the actual harness/model when observable and disclose any uncertainty. |
| Observe or wait | Return observations for that worker, distinguishing activity, input requests, errors and unknown state. Explain timeout and notification semantics. |
| Continue | Address the same confirmed worker/session with additional input when continuation is supported and admitted. |
| Halt | Request cancellation of the run's workers and confirm what stopped, what remains active and what is uncertain. |
| Reconcile and release | Match runtime observations with retained outputs and tracker records before retry, handoff or resource reuse. Release only resources owned by this run. |

Warn actors about unsupported configured combinations. Automatically select
another compatible candidate only when configuration permits it. If a required
capability is unavailable, stop the affected dispatch and report the missing
capability. Having integration instructions does not establish tool availability.

## Run procedure

1. Resolve configuration, authorized story scope and limits. Retain the run
   identity, start time, effective settings, story references, worker identities,
   workspace/branch locations and observed results so execution can be reconciled.
2. Inspect existing work before dispatch or resume. Resolve an uncertain prior
   launch before starting another worker for that task. Select within the
   requested story series or authorized backlog scope and check prerequisites
   using workflow. Exact backlog ranking is not yet specified.
3. Admit the next action only while the run is active and its limits permit it.
   Recheck before launches, continuations, fallback attempts and delivery actions.
   Supply the worker with the story reference, assigned phase, specification
   reference, relevant workflow/tracker contracts, workspace, chosen model and
   execution authority. Workers inherit the same run limits.
4. Launch through the selected integration and retain the returned identity.
   Observe or wait using a mechanism that leaves configured stop controls
   usable. A wait timeout returns control; it does not establish worker exit.
5. Reconcile the worker's outputs through tracker and validate them against the
   applicable workflow contract. Runtime idle/done, process exit and an agent
   summary alone do not establish a phase result. A runtime input dialog also
   does not itself establish workflow [blocked][status:blocked] status.
6. Record validated results through tracker. Use workflow's
   [lifecycle](../workflow/references/lifecycle.md) for the next destination and
   [completion contract](../workflow/references/lifecycle.md#story-completion)
   for delivery. Merge authority follows project policy and the current request.
   Continue only within the run's authorized scope and remaining limits.

Keep execution identity separate from story identity. A pane or reused agent
name alone is insufficient to identify a previous task. Parallel dispatch
requires known workspace isolation and admission controls; a concrete
concurrency/locking procedure is not yet specified.

## Stopping and reconciliation

The [run controls](references/configuration.md#run-controls) define the stop
conditions. The first reached condition closes admission.

On deadline expiry, drain by default: stop new dispatches and let admitted
workers finish. A project may request cancellation at the deadline only when
the execution integration can enforce it. An explicit user halt closes
admission immediately and requests cancellation of this run's workers.

Observe stop/deadline signals through the integration's available mechanism,
including while waiting. Before starting an unattended run, disclose which
controls can be enforced and any observation delay. If a required control is
unsupported, report it before dispatch rather than promise an unenforceable
limit.

Confirm worker state after cancellation. If work may still be active, retain
its ownership and report the uncertainty; do not start a replacement for it.
Reconcile late outputs before reuse or resume. Stopping execution does not
roll back completed work or establish a workflow transition. Record a workflow
hold only under workflow's own rules; a run limit alone is not a product blocker.

Report the stop reason, confirmed story/PR outcomes, active or uncertain workers,
remaining authorized work and the condition needed to resume. A new invocation
does not silently clear an explicit halt.

## Integrations

| Runtime/context | Reference or limitation |
| --- | --- |
| Herdr | [Execution mapping](references/herdr.md), including current capability limits |
| Cursor Cloud | [API agent/run mapping](references/cursor-cloud.md) |
| Cursor native subagents | [Current-session delegation mapping](references/cursor-subagents.md) |
| cmux | [macOS and TUI command mappings](references/cmux.md), selected for the installed interface |

Integration helpers, when needed, belong under `scripts/<integration>/` within
this skill. Runtime references describe tool operations and constraints; they
reference workflow requirements and the shared configuration policy.
