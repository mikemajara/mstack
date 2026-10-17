# mstack

A proposed harness-neutral delivery stack for small software projects.

**Draft skills, not a release ready for installation.** Develop the stack in
its three skill roots:

- [workflow](skills/workflow/SKILL.md): phase contracts, verification and default memory.
- [orchestrator](skills/orchestrator/SKILL.md): execution and permissions; optional Herdr integration.
- [tracker](skills/tracker/SKILL.md): specifications, workflow facts and evidence; optional GitHub integration.

Start with the [workflow flowchart](skills/workflow/references/lifecycle.md) and
[research gate](skills/workflow/references/phase/research.md). Incomplete contracts are marked
in place. [Structure](docs/STRUCTURE.md) maps file ownership;
[handoff](docs/HANDOFF.md) records the current review point.
[MEMORY.md](MEMORY.md) retains project knowledge not already in `AGENTS.md`.

`legacy/backlog/` is an unchanged historical snapshot of the backlog skill
through skills PR #10. It preserves the source material, including known
mixed responsibilities; it is not the agreed mstack architecture. Do not
install it as mstack or treat its instructions as guidance for this repository.

[Provenance](docs/PROVENANCE.md) records the extraction and verification.
The original skills repository, open PRs, installed skills, server runtime
and timers have not been migrated or activated by this bootstrap.

The repository starts private while the structure is being agreed. There is
no new license selection or public release in this milestone.
