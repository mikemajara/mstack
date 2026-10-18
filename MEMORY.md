# Project memory

Current continuity for developing mstack. Repository rules live in
[AGENTS.md](AGENTS.md), file ownership in [STRUCTURE.md](docs/STRUCTURE.md), and
product behavior in the [draft skills](skills/). Keep only context that helps
the next development decision.

## Current review — 2026-10-18

- The user is reviewing [workflow phase references](skills/workflow/references/phases/).
  Continue with the requested reference or boundary; there is no requirement to
  complete every scaffold in one pass.
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

## Open design questions

- Exact phase gate questions and pass conditions, starting with research and
  refinement.
- Detailed evidence format and rules for revalidating changed versions.
- Closure rules for non-delivery dispositions such as duplicate stories; the
  legacy `not planned` closure was not adopted as a delivery-completion rule.
- Orchestrator integration availability and cross-skill reference resolution
  after installation.
- Cross-cutting data-migration requirements.
- Completing the draft memory contract and reviewing its target-project
  convention; this repository's memory location is defined in `AGENTS.md`.

Herdr remains a placeholder. GitHub's procedures are drafted; runtime integration,
packaging and a supervised real-project trial remain future work. Local helper
validation does not establish that the integration has been exercised against a
live repository.
