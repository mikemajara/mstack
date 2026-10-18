# Project memory

Current continuity for developing mstack. Repository rules live in
[AGENTS.md](AGENTS.md), file ownership in [STRUCTURE.md](docs/STRUCTURE.md), and
product behavior in the [draft skills](skills/). Keep only context that helps
the next development decision.

## Current review — 2026-10-18

- Versioning decisions are recorded in [AGENTS.md](AGENTS.md#versioning-and-release-notes):
  one shared mStack version across the three skills, starting at `0.1.0`, with
  a version field in each skill frontmatter and an individual changelog per
  skill. Every release entry summarizes all skill changes and states their
  impact/adaptation requirement for that skill, including explicit no-change
  entries. The authoring session has added the initial version fields and
  `0.1.0` baseline changelogs; no published release process or installer has
  been validated.
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
  remain open. The temporary
  [draft review queue](docs/DRAFT-REVIEW.md) records the settled behavior and
  remaining runtime and packaging validation items.
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
  installation or activation. For coordinated runs, the controller is the
  sole writer and workers return proposed updates. Standalone writes require
  one writer and serialization; no CLI or file lock is supplied. Concurrent
  worktree writes remain unsupported and this path has not had a live trial.
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
- The user's runtime examples are Herdr, Cursor Cloud and cmux; agent harness
  examples are `agent-cursor`, `codex` and `fx`. Cursor also needs native
  subagent operation mappings. They use Cursor for Grok/Composer and Codex
  subscription access for OpenAI models; do not assume interchangeable account
  routes. Model IDs are deliberately project choices. Additional harness
  mappings remain pending.
- [Herdr](skills/mstack-orchestrator/references/herdr.md) has an initial operation
  mapping checked against official documentation. Codex and Cursor model flags
  are documented; other harnesses remain unsupported until verified. The
  integration requires external project exclusivity and does not promise hard
  halt without process-tree confirmation. No runtime was launched or configured.
- The suggested 90% of a five-hour window is not a universal policy. The
  working v1 rule permits optional runtime-specific guards only when the adapter
  defines signal, units, window, polling cadence, threshold and stop effect.
  Unsupported requested guards block dispatch; live runtime validation remains
  future work.
- [Cursor Cloud](skills/mstack-orchestrator/references/cursor-cloud.md),
  [native subagents](skills/mstack-orchestrator/references/cursor-subagents.md) and
  [cmux](skills/mstack-orchestrator/references/cmux.md) now have documentation mappings.
  cmux keeps separate macOS and TUI sections because their CLI grammars differ;
  select one interface and validate the installed harness pairing before use.
  Default to drain; hard halt needs verified process stop and confirmation.
  Cursor Cloud's
  follow-up request has no documented model override, so the mapping uses a
  new agent on the intended branch/PR when changing models. Cloud agent creation
  supports duplicate detection with a client-supplied agent ID; follow-up runs
  have no client-supplied run ID, so ambiguous requests must be reconciled
  through API records before retry. A later controller does not continue the
  old agent's conversation: it reconciles the outcome and starts fresh work from
  current project records. Cloud Agents API v1 is public beta, with no documented
  cancellation-latency guarantee. The mapping is conditionally usable unattended:
  default to drain, and use hard halt only when cancellation and terminal-state
  observation can be confirmed before checkpointing. No live runtime validation
  has been performed. Runtime-specific target settings and native delegation
  selection are described in the shared configuration reference. Native Cursor
  delegation follows the same drain default; deadline halt depends on
  cancellation in the selected task surface and cannot be inferred from the
  separate SDK.
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
  guards; and control-signal transport. No shared lock API is required.
  `maxDurationMinutes` is per session and does not accumulate across scheduled
  sessions.
- Validate external project admission and recovery-only takeover with Herdr,
  Cursor Cloud and cmux; readiness/cancellation limitations and native subagent
  cancellation also need runtime checks.
- Cross-cutting data-migration requirements.
- No installer or package has yet validated the release layout. Future
  packaging must preserve the three same-version sibling directories and their
  relative references as described in `README.md`.

Tracker and orchestrator integration procedures are drafted with marked limitations; runtime
integration, packaging and a supervised real-project trial remain future work.
Local helper validation does not establish that the integrations have been
exercised against live repositories or runtimes.
