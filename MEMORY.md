# Project memory

Current continuity for developing mstack. Repository rules live in
[AGENTS.md](AGENTS.md), file ownership in [STRUCTURE.md](docs/STRUCTURE.md), and
product behavior in the [draft skills](skills/). Keep only context that helps
the next development decision.

## Current review — 2026-10-17

- The user is reviewing [workflow phase references](skills/workflow/references/phase/).
  Continue with the requested reference or boundary; there is no requirement to
  complete every scaffold in one pass.
- [Research](skills/workflow/references/phase/research.md) now explicitly includes
  feasibility questions and possible changes. Its two required gate questions
  still have draft wording. Later phase contracts and integration procedures
  remain incomplete.
- Attribute definitions live in [attributes.json](skills/workflow/references/attributes.json).
  The short semantic-reference convention is maintained in
  [workflow definitions](skills/workflow/SKILL.md#definitions).

## Open design questions

- Exact phase gate questions and pass conditions, starting with research and
  refinement.
- Integration rework routes and whether deployment is applicable to a given
  work item.
- Detailed evidence format and rules for revalidating changed versions.
- Missing-integration handling and cross-skill reference resolution after
  installation.
- Cross-cutting data-migration requirements.
- Completing the draft memory contract and reviewing its target-project
  convention; this repository's memory location is defined in `AGENTS.md`.

Herdr and GitHub integration procedures are placeholders. Runtime integration,
packaging and a supervised real-project trial remain future work; draft content
does not establish that any of them have been validated.
