# Project memory

Current continuity for developing mstack. Repository rules live in
[AGENTS.md](AGENTS.md), file ownership in [STRUCTURE.md](docs/STRUCTURE.md), and
product behavior in the [draft skills](skills/). Keep only context that helps
the next development decision.

## Current review — 2026-10-19

- Versioning decisions are recorded in [AGENTS.md](AGENTS.md#versioning-and-release-notes):
  one shared mstack version across the three skills, starting at `0.1.0`, with
  a version field in each skill frontmatter and an individual changelog per
  skill. Every release entry summarizes all skill changes and states their
  impact/adaptation requirement for that skill, including explicit no-change
  entries. The authoring session has added the initial version fields and
  `0.1.0` baseline changelogs; no public release has been made. Users can
  install the skill set from GitHub with the third-party Skills CLI:
  `npx skills add mikemajara/mstack --skill '*'`. mstack does not bundle an
  installer, and does not install Herdr, fx or other external runtime products.
- The user settled the autonomous-session boundary: each controller session
  should close with a durable checkpoint, and a later session should start fresh
  from GitHub or local project records. Do not rely on open runtime panes or
  assume workers retain current context across autonomous runs. Checkpoint
  contents are defined as reconciliation of canonical records, not a separate
  state store. If worker termination or outcome is uncertain, retain ownership
  and record the reconciliation condition; do not dispatch replacement work
  before reconciliation. Once the prior controller is confirmed ended, a fresh
  controller may take exclusive controller admission in recovery-only mode to
  inspect and reconcile, without dispatching or changing claims. Runtime-specific
  detection and confirmation, cleanup details and runtime limit monitoring
  remain open. The temporary [review queue](docs/DRAFT-REVIEW.md) records
  settled behavior and remaining runtime/filesystem validation items.
- Autonomous jobs in the old backlog have been observed stopping after reporting
  a next step despite its continuation instructions. The user requires explicit
  continuation in mstack and expects iterations with runtime trials; stronger
  wording alone is not evidence that the behavior is fixed. The orchestrator's
  [continuation contract](skills/mstack-orchestrator/SKILL.md#autonomous-sessions)
  now requires repeating dispatch, checking other eligible stories after a hold,
  and accounting for continuation before ending an invocation. Future trials
  should exercise phase handoffs, worker
  completion/waits and a blocked story alongside other eligible work, checking
  actual dispatch rather than accepting a reported next step as continuation.
  Trials should also trigger a configured, controlled runtime stop or
  pre-limit guard, verify worker reconciliation and checkpointing before the
  controller ends, then confirm a later invocation makes a fresh decision from
  those records. This does not assume cleanup after an abrupt hard limit. No
  live trial has been run.
- The reviewed cleanup targets the shared `0.1.0` baseline. The three skill
  roots and names are `mstack-workflow`, `mstack-orchestrator` and
  `mstack-tracker`; [STRUCTURE.md](docs/STRUCTURE.md) maps their files. Workflow
  can guide an actor directly; tracker supplies storage operations and
  orchestrator coordinates runs. Neither supplies environment access.
- The cleanup incorporates the review's handoff, independent-QA, role-scope,
  claim, drift-check and untrusted-record corrections in their owning contracts. See
  [specification changes](skills/mstack-workflow/references/lifecycle.md#specification-changes)
  for invalidation; formal specification versions were deliberately removed.
  [Non-delivery closure](skills/mstack-workflow/references/lifecycle.md#story-completion)
  and [project memory](skills/mstack-workflow/references/memory.md) now replace
  their former open design questions. This repository's root memory convention
  remains in [AGENTS.md](AGENTS.md).
- [Filesystem tracking](skills/mstack-tracker/references/filesystem.md) and a bundled
  [issue template](skills/mstack-tracker/assets/issue.md) now define the default when
  no issue tracker is selected. Workflow states the fallback; tracker owns
  selection and operations, with the optional selector cross-referenced from
  project configuration. This is a contract/template pass, with no local CLI,
  installation or activation. Filesystem mutations use an environment-provided
  short-lived lock scoped to the issue file being changed; workers return
  proposed updates for reconciliation by the controller. The lock releases
  after the confirmed write and must not remain stuck after a crash. There is
  no project/session-wide tracker lock; live concurrent-writer and crash
  recovery validation remains open, and any stricter policy should follow
  evidence from actual conflicts.
- Legacy plans, setup/status, dedupe, queue and snapshot behavior were reviewed
  for that pass. Specification sections and local inspection were adapted;
  draft promotion states, snapshots, readiness heuristics and unrelated
  scaffolding were not imported. Local files are canonical issues, not an
  outage cache. QA/merge gates and non-delivery closure rules are owned by
  workflow. Local checks exercised the populated YAML template and
  documented search commands, including closed files under a hidden, ignored
  directory. No live target-project trial has been performed.
- [Research](skills/mstack-workflow/references/phases/research.md) explicitly includes
  feasibility questions and possible changes. Research, refinement,
  implementation and QA gates have been reviewed and are recorded in their
  phase contracts. Lifecycle defines their transitions and blocked-work rules.
- Version-control and PR conventions now live in
  [implementation](skills/mstack-workflow/references/phases/implementation.md), and
  evidence placement in [QA](skills/mstack-workflow/references/phases/qa.md).
  [Story completion](skills/mstack-workflow/references/lifecycle.md#story-completion)
  defines the agreed QA, merge and closure sequence independently of the
  story's tracker. Exceptions to the default story/branch/PR relationship have
  not been designed.
- Attribute definitions live in [attributes.json](skills/mstack-workflow/references/attributes.json).
  The short semantic-reference convention is maintained in
  [workflow definitions](skills/mstack-workflow/SKILL.md#definitions).
- [Tracker](skills/mstack-tracker/SKILL.md) now defines the small common integration
  contract, including capability selection and partial-operation handling.
  [GitHub](skills/mstack-tracker/references/github.md) supplies Issues and PR operations
  independently, using direct `gh` commands and a bundled label helper under
  `skills/mstack-tracker/scripts/github/`. The legacy scripts were reviewed as source;
  their scaffolding, readiness heuristics and orchestration behavior were not
  imported. Automatic legacy-label migration, offline cache/export tooling and
  GitHub Projects fields remain deliberate exclusions. Existing-record
  migration requires an explicit decision; exports never replace a current
  remote read.
- GitHub's reference was tightened to link to workflow's phase, evidence and
  completion requirements rather than repeat them. The first draft's extra
  snapshot/digest and verdict-history requirements were removed. The evidence
  record layout remains project-owned under the Verification contract.
  GitHub-specific storage, story identity and operation confirmation remain in
  the adapter.
- The label helper reads its bundled manifest without depending on an installed
  sibling workflow path. Its names were checked against workflow's vocabulary.
  Local checks used a temporary fake `gh` for preview/apply, repeat runs,
  preservation of unrelated labels, and read/write/verification failures.
  After the rename, isolated checks confirmed manifest discovery from another
  working directory, a passing current mapping, and detection of stale QA
  metadata. Skill validation, local links/anchors, phase YAML and vocabulary
  parity passed. No remote labels were read or changed in the cleanup.
- [Orchestrator](skills/mstack-orchestrator/SKILL.md) now drafts the common execution
  contract and run procedure. [Configuration](skills/mstack-orchestrator/references/configuration.md)
  defines project runtime/harness/model choices, role defaults, configured
  fallbacks and run controls in one `.mstack/config.json`; the earlier separate
  model-file proposal was rejected as unnecessary. These are instructions and
  configuration semantics, not an executable controller or configuration loader.
- mstack 0.1.0 supports Herdr as the runtime and documents Codex, Cursor CLI
  and fx CLI harness mappings. Cursor Cloud and native Cursor subagents are
  deferred and unsupported; cmux is removed from the release scope. Do not
  assume interchangeable account routes. Model IDs are project choices.
- [Herdr](skills/mstack-orchestrator/references/herdr.md) has an initial operation
  mapping checked against official documentation. The current official docs
  support the documented CLI launch, prompt and wait operations. Herdr does not
  provide project-level controller admission, and `ctrl+c` does not prove
  process-tree termination. The integration remains conditional on external
  exclusivity and defaults to `drain`; no live runtime trial was performed.
- The [fx CLI mapping](skills/mstack-orchestrator/references/fx.md) now covers
  `fx ask` as a harness only, requiring an execution runtime that can launch and
  observe it. Its command flags and structured result/exit handling were
  checked against the official fx documentation. Native fx subagents are
  deferred. No live runtime or cancellation trial has been performed.
- The suggested 90% of a five-hour window is not a universal policy. The
  working v1 rule permits optional runtime-specific guards only when the adapter
  defines signal, units, window, polling cadence, threshold and stop effect.
  Unsupported requested guards block dispatch; live runtime validation remains
  future work.
- Cursor Cloud and native Cursor subagent references are retained for possible
  later work, but neither is selectable in mstack 0.1.0. cmux support was
  removed because there is no near-term plan to implement it. A low-priority,
  not-ready backlog story can be created if cmux is prioritized later.
- Legacy remains frozen historical source material; removal is a separate
  change, so its repository guidance and source pointers remain. The cleanup
  deliberately does not port scaling by story type, legacy hardening prompts,
  additional PR hygiene rules,
  catch-up briefs or legacy label migration. These are deferred rather than
  silently required; the current branch/PR/link contracts still apply. Review
  needed source material before any eventual removal.

## Open design questions

- Runtime-specific enforcement of the shared controller-admission protocol
  (exclusive admission, recovery-only takeover after the prior controller ends,
  and release after checkpointing); live validation of runtime-specific usage
  guards; and control-signal transport. Filesystem tracker uses a separate
  short-lived per-file lock for writes; it does not replace controller
  admission.
  `maxDurationMinutes` is per session and does not accumulate across scheduled
  sessions.
- Cross-cutting data-migration requirements.
- **Reliability evaluation story:** Create a follow-up story for supervised
  Herdr runs that evaluates autonomous continuation, worker reconciliation,
  session checkpoints and fresh-session decisions. Retain logs/evidence and
  decide the reliability pass criteria in that story; no live trial has run.
- **Project setup and feedback intake:** The Skills CLI install path is
  confirmed, but target-project setup remains undecided: whether first use
  should check for `.mstack/` and ask before creating it, use a separate setup
  skill/repo, or another entry point. Separately consider a thin feedback skill
  that can file bug reports and feature requests at
  https://github.com/mikemajara/mstack. These are options, not decided behavior.
- Distribution must preserve the three same-version sibling skill directories
  and relative paths. The third-party Skills CLI supports installing all skills
  from the repository; mstack itself does not bundle an installer.

Tracker and orchestrator procedures include documented limits. Autonomous
session reliability, filesystem lock behavior, and project setup/feedback intake
remain follow-up work; local document checks do not establish live runtime
behavior.
