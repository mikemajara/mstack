# Session handoff — 2026-10-17

## Current state

The three draft skill roots now exist. [STRUCTURE.md](STRUCTURE.md) maps their
files and ownership. Develop behavior directly in those files; avoid a parallel
workflow specification in docs. The temporary `docs/WORKFLOW.md` was folded into
workflow references. This is a scaffold, not a release ready for installation.

## Continue the conversation

Use short exchanges about one workflow boundary at a time. The current example
is a request to add dark mode, used to discover general requirements, not to
encode feature-specific rules. Workflow is the priority before runtime/adapters.

Start with [research.md](../skills/workflow/references/phase/research.md): its two proposed
mandatory gate questions remain under review. The user wants concrete,
question-driven gates with a small required set, clearly distinguished from
investigation guidance. Missing answers must stay explicit, without inventing
facts to advance. [lifecycle.md](../skills/workflow/references/lifecycle.md) contains the
flowchart and proposed fallback model.

Then work through refinement and its implementation-readiness requirements,
implementation/QA, integration/deployment, verification detail and default memory.
The phase files expose entry, outputs, exit validation and destinations so each
can be reviewed in place. Detailed procedures remain incomplete.

## Open details

- Exact research gate wording and later detailed phase gates.
- Integration rework routes and deployment's phase applicability.
- Detailed evidence format and version revalidation rules.
- Missing-integration handling and installed cross-skill reference resolution.

Herdr and GitHub references are placeholders. Runtime contracts, adapters,
packaging, cutover and a supervised real-repository trial remain later work.
Authoring does not authorize installation, workers, timers, merging or deployment.

## Historical context

[PROVENANCE.md](PROVENANCE.md) records preserved backlog history and import tags.
Prior hardening was published in skills PR #10 and server-docker PR #2. The
previous handoff reported 73 isolated tests and independent reviews of evidence,
cancellation and reconciliation fixes; it did not establish successful live
execution. Current PR, installation and runtime state has not been re-audited.
Runtime/proof ownership remains in server-docker pending explicit cutover.
