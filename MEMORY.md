# Project memory

Current continuity for developing mstack. Repository rules live in
[AGENTS.md](AGENTS.md), file ownership in [STRUCTURE.md](docs/STRUCTURE.md), and
product behavior in the [draft skills](skills/). Keep only context that helps
the next development decision.

## Current review — 2026-10-18

- [Filesystem tracking](skills/tracker/references/filesystem.md) and a bundled
  [issue template](skills/tracker/assets/issue.md) now define the default when
  no issue tracker is selected. Workflow states the fallback; tracker owns
  selection and operations, with the optional selector cross-referenced from
  project configuration. This is a contract/template pass, with no local CLI,
  installation or activation. Single-writer operation in an authoritative
  checkout is the v1 limit; concurrent worktree writes remain unsupported.
- Legacy plans, setup/status, dedupe, queue and snapshot behavior were reviewed
  for that pass. Specification sections and local inspection were adapted;
  draft promotion states, snapshots, readiness heuristics and unrelated
  scaffolding were not imported. Local files are canonical issues, not an
  outage cache. QA/merge gates and unresolved non-delivery closure rules remain
  owned by workflow. Local checks exercised the populated YAML template and
  documented search commands, including closed files under a hidden, ignored
  directory. No live target-project trial has been performed.
- [Research](skills/workflow/references/phases/research.md) now explicitly includes
  feasibility questions and possible changes. Its two required gate questions
  still have draft wording.
- Version-control and PR conventions now live in
  [implementation](skills/workflow/references/phases/implementation.md), and
  evidence placement in [QA](skills/workflow/references/phases/qa.md).
  [Story completion](skills/workflow/references/lifecycle.md#story-completion)
  defines the agreed QA, merge and closure sequence independently of the
  story's tracker. Exceptions to the default story/branch/PR relationship have
  not been designed.
- Attribute definitions live in [attributes.json](skills/workflow/references/attributes.json).
  The short semantic-reference convention is maintained in
  [workflow definitions](skills/workflow/SKILL.md#definitions).
- [Tracker](skills/tracker/SKILL.md) now defines the small common integration
  contract, including capability selection and partial-operation handling.
  [GitHub](skills/tracker/references/github.md) supplies Issues and PR operations
  independently, using direct `gh` commands and a bundled label helper under
  `skills/tracker/scripts/github/`. The legacy scripts were reviewed as source;
  their scaffolding, readiness heuristics and orchestration behavior were not
  imported. Label migration and cache/export remain outside this pass.
- GitHub's reference was tightened to link to workflow's phase, evidence and
  completion requirements rather than repeat them. The first draft's extra
  snapshot/digest and verdict-history requirements were removed; the detailed
  evidence format remains an open workflow question. GitHub-specific storage,
  identity constraints and operation confirmation remain in the adapter.
- The label helper reads its bundled manifest without depending on an installed
  sibling workflow path. Its names were checked against workflow's vocabulary.
  Local checks used a temporary fake `gh` for preview/apply, repeat runs,
  preservation of unrelated labels, and read/write/verification failures.
  Recheck the manifest when changing workflow attributes.
- [Orchestrator](skills/orchestrator/SKILL.md) now drafts the common execution
  contract and run procedure. [Configuration](skills/orchestrator/references/configuration.md)
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
- [Herdr](skills/orchestrator/references/herdr.md) has an initial operation
  mapping checked against official documentation. The legacy four-window
  policy, environment-only launch prerequisite and fixed model choices were
  not imported. Its incomplete launch-argument and confirmed-cancellation
  mappings are marked in place. No runtime was launched or configured.
- The user deferred quota observation. Current run controls cover story scope,
  elapsed time and explicit halt; subscription monitoring and quota-based stop
  settings were removed from the orchestrator draft.
- [Cursor Cloud](skills/orchestrator/references/cursor-cloud.md),
  [native subagents](skills/orchestrator/references/cursor-subagents.md) and
  [cmux](skills/orchestrator/references/cmux.md) now have documentation mappings.
  cmux has separate macOS and TUI sections because their CLI grammars differ;
  its installed interface is not known in this workspace. Cursor Cloud's
  follow-up request has no documented model override, so the mapping uses a
  new agent on the intended branch/PR when changing models. Runtime-specific
  target settings and native delegation selection are described in the shared
  configuration reference. No live runtime validation has been performed.

## Open design questions

- Exact phase gate questions and pass conditions, starting with research and
  refinement.
- Detailed evidence format and rules for revalidating changed versions.
- Closure rules for non-delivery dispositions such as duplicate stories; the
  legacy `not planned` closure was not adopted as a delivery-completion rule.
- Durable orchestrator run-state format, concurrent controller admission/locking,
  control-signal transport and installed cross-skill reference resolution.
- Verified harness launch/readiness arguments, native subagent cancellation and
  confirmed process cancellation/descendant handling for terminal runtimes.
- Cross-cutting data-migration requirements.
- Completing the draft memory contract and reviewing its target-project
  convention; this repository's memory location is defined in `AGENTS.md`.

Tracker and orchestrator integration procedures are drafted with marked limitations; runtime
integration, packaging and a supervised real-project trial remain future work.
Local helper validation does not establish that the integrations have been
exercised against live repositories or runtimes.
