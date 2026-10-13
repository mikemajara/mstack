# mstack structure for alignment

Status: **draft, not implementation approval**. Owner: Miguel.

Review boundaries and item specifications here before authoring new skills.
Only the legacy archive and bootstrap documents currently exist. Proposed
skills, references, adapters and tests below are not implemented.

## Outcome and applicability

One bundle supports direct issue work or a bounded orchestrator run through Herdr or
Cursor. It advances eligible work, leaves verified PRs for review and reports
durable outcomes. A blocked issue does not monopolize the active lane.

Current input: backlog mixes lifecycle, phase craft, scheduling, policy,
GitHub operations and Herdr mechanics. Apply questioning, deletion and
simplification now. Acceleration and activation wait for aligned contracts
and a supervised pilot. No additional parallelism, framework or installation
machinery is authorized by this draft.

## Proposed skill boundaries

Four entry skills, not one skill for every phase. Names are provisional.
Miguel chose `mstack-orchestrator` over `mstack-pm` for explicit role naming.

| Skill | Trigger / input | Owns / output | Must not own |
| --- | --- | --- | --- |
| `mstack` | Request needing routing or stack setup | Select the relevant skill and applicable project policy; expose missing capabilities | Issue lifecycle, phase craft, scheduling or actual tool calls |
| `backlog` | Capture, triage, research, refine, catch-up or issue-state audit | Canonical issues, product specifications, lifecycle classification and factual context | Implementation, QA execution, worker spawning, merge authority or budget policy |
| `mstack-orchestrator` | Explicit orchestration request with scope and run limits | Select eligible work, claim/release according to policy, dispatch, reconcile and report | Research/refine/implement/verify craft, inventing readiness rules or merging without authority |
| `mstack-delivery` | Implement or verify a specified unit | Reviewable change or honest verification verdict with evidence | Prioritization, product decisions, invented acceptance criteria or implicit production authority |

Every SKILL.md states its trigger, essential constraints and conditional
reference routes. It does not repeat the contents of those references.
Research/refine remain backlog modes; implement/verify remain delivery modes.
An implementation worker and an independent verifier can use different modes of the same
skill in separate sessions; skill count is not worker count.

## Proposed reference specifications

Paths below are relative to `skills/<owning-skill>/references/`.

| Owner / item | Content and output | Read when / references |
| --- | --- | --- |
| `mstack/execution-policy.md` | Resolve permitted actions, transition writers, human gates, review/merge authority, model/WIP/budget limits and project overrides. Missing permission never becomes implied permission. | Before actions; adapters consume it, lifecycle does not define it |
| `mstack/capabilities.md` | Tool-neutral dispatch/wait/stop/identity, tracker and evidence capability contracts; missing-capability behavior and adapter selection | Choosing execution tools; points to the matching adapter only |
| `backlog/lifecycle.md` | One actor-free flowchart plus guarded state/transition table: entry requirements, completion evidence, valid destinations, rework and blocked/resume conditions | Classifying state/readiness; engineering facts come from phase artifacts |
| `backlog/intake.md` | Capture, deduplicate, group related reports and choose initial classification without inventing scope | New work; uses lifecycle and tracker adapter |
| `backlog/research.md` | Gather facts and sources, explain uncertainty, leave a research brief | Insufficient facts; references lifecycle instead of repeating gates |
| `backlog/refine.md` | Shape one issue's product contract, scope, testable AC and executable checks; expose unresolved product decisions | Shaping work; references spec and lifecycle |
| `backlog/spec.md` | Canonical issue-body product-spec fields and minimum executable check description; proportionate detail for small changes | Reading/writing a spec; no implementation sequence or authority rules |
| `backlog/context.md` | Read-only catch-up, local-vs-shared evidence and explicit memory-to-project-guidance promotion | Context requests; no automatic triage, claim or permanent priority gates |
| `mstack-orchestrator/run.md` | Admission, selection, claims, worker briefs, completion handling, lane release, retries on changed signals and terminal reporting | Orchestration mode; uses lifecycle, policy, protocol and chosen adapters |
| `mstack-orchestrator/outcome-protocol.md` | Structured phase/verification outcomes, registered identity, evidence bindings, cancellation, stale/replayed results and report reconciliation | Scheduled workers/adapter validation; transport and attestations, not a second lifecycle |
| `mstack-delivery/implement.md` | Execute locked scope, use project conventions, reproduce defects or preserve refactor baselines, self-check and prepare a linked reviewable PR | Implementation; uses spec, verification recipe and policy |
| `mstack-delivery/verify.md` | Independent behavioral checks where policy requires them, exact AC coverage, current head/base/spec, retained evidence and honest failure classification | Verification; uses spec, policy and outcome protocol when scheduled |
| `mstack-delivery/data-migrations.md` | Traceable migration artifacts, named generate/apply commands, target/environment identification, safe rehearsal evidence and explicit pending-operation handoff | Data/schema migration only; commands and human reservations come from project policy |

Lifecycle validity is not execution authority. Scheduling may add identity
and evidence attestations; it must not silently redefine readiness. Reaching
review-ready is not evidence of integration or production deployment.

## Adapters: tools, not lifecycle or permission

Proposed paths: `skills/mstack/references/adapters/<name>.md`.

| Adapter | Content | Exclusions |
| --- | --- | --- |
| `herdr.md` | Start/wait/stop commands, pane/session identity, isolation/reuse and safe reconciliation mechanics | No phase gates, prioritization, browser/model defaults or merge decisions |
| `cursor.md` | Available native delegation/wake mechanisms, separate-session execution, completion/cancellation and capability limitations | No mandatory Cursor tools in shared skills; no alternative lifecycle |
| `github.md` | Issue/label/PR read/write mapping, links, expected check/review metadata and receipts | Does not decide that work is ready or authorized |

Project configuration is separate from the distributed bundle. It declares
the harness, tracker, tools, scope, allowed environments and reserved actions.
The exact config filename/schema and installer are deferred until boundaries
are agreed. All referenced files must ship together; no reliance on a file
that exists only in GitHub or only in another harness's installation.

Migration example: the playbook describes artifacts and steps; project policy
reserves the named database generation/application commands for Miguel; the
adapter cannot grant itself that permission. An explicit completion/unblock
signal resumes the pending step. This is not a reason to weaken verification
or mark deployment done. Other eligible issues remain available.

## Existing material: proposed disposition, no edits yet

All paths in this table are under `legacy/backlog/`.

| Existing item | Proposed destination / treatment |
| --- | --- |
| `SKILL.md` | Split its existing responsibilities across the four entry skills; preserve useful behavior, not duplicated prose |
| `references/workflow.md`, `drift.md` | `backlog/lifecycle.md`; state classification and drift reconciliation share one definition |
| `references/research.md`, `refine.md`, `plan.md` | Backlog research/refine/spec refs, with gates referenced rather than copied |
| `references/catch-up.md`, `long-term-memory.md` | `backlog/context.md`; keep read-only and explicit promotion modes distinct |
| `references/pm.md` | Orchestrator run mechanics; move permission/model/WIP decisions to execution policy |
| `references/herdr.md` | Herdr adapter mechanics only; move non-harness rules to their owners |
| `references/implement.md` | Delivery implement mode; move tracker hygiene to intake/GitHub operations and authority to policy |
| `references/qa.md`, `verification.md` | Delivery verify mode; retain risk-proportionate real-surface proof and project recipes |
| `references/scheduled-proof.md` | Split outcome transport into orchestrator protocol, semantic gates into lifecycle/spec/verify, permissions into policy |
| `references/github.md` | GitHub adapter and helper usage; no lifecycle or merge-authority prescriptions |
| `references/vercel-toolbar.md` | Optional intake-source reference later; not a required fifth skill or initial rewrite target |
| `references/spawn.md` | Retire the obsolete routing shim after its callers are checked; do not reproduce the Herdr-only restriction |
| `references/labels.json` | Backlog-owned tracker mapping; no label migration until the lifecycle is agreed |
| `references/version.json`, `README.md` | Historical metadata; a coordinated mstack version replaces it only at release |
| `scripts/backlog-setup.mjs`, `backlog-sync.mjs` | Backlog/GitHub helpers; preserve behavior and audit callers before moving |
| `scripts/backlog-status.mjs`, `backlog-issue-audit.mjs` | Backlog state-evidence helpers; check them against the canonical lifecycle |
| `scripts/backlog-dedupe.mjs`, `backlog-refinement-candidates.mjs` | Intake/readiness helpers; selection policy remains with the orchestrator |
| `scripts/catch-up-gather.mjs`, `backlog-lib.mjs` | Context gatherer and shared GitHub helper implementation; no new framework |
| `assets/agent-hint.md`, `inbox.md`, `memory.md` | Backlog setup templates; hints reference workflows rather than embedding them |

The server-docker runtime is not copied here. Its portable protocol/validator
ownership is a later migration decision; host quota collection, cron/systemd,
webhooks and secrets remain deployment integration. Preserve the existing
proof regressions when the contract changes; do not recreate the runtime.
No full pstack fork or new implementation is included in bootstrap. Any later
reuse must retain source/license attribution and isolate harness bindings.

## Agreement points before drafting content

1. Four skills as above, or a smaller boundary with an explicit reason.
2. Canonical lifecycle owned by backlog; authority/policy owned separately.
3. References and three initial adapters as above; no per-phase skill explosion.
4. One packaged dependency set with tested installed reference resolution.

Miguel reviews these boundaries first. Only then draft the actor-free
lifecycle and its criteria, followed by one reference/adapter at a time.
Do not bulk-author the proposed files. Retain source links and small diffs.

Acceptance checks for the eventual structure: removing Herdr loses no phase
rule; switching to Cursor changes tools, not readiness; blocked work releases
the lane; reserved migration commands cannot run autonomously; missing or
stale evidence never becomes a verified PR. Increased WIP and scheduled
activation stay gated on a supervised pilot, not on a documentation review.
