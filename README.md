# mstack

A harness-neutral delivery stack for small software projects, currently being
developed as three draft skills:

- [workflow](skills/workflow/SKILL.md): phase contracts, verification and project memory.
- [orchestrator](skills/orchestrator/SKILL.md): execution, project configuration and permissions; optional runtime integrations.
- [tracker](skills/tracker/SKILL.md): specifications, workflow facts and evidence; filesystem issues by default when no tracker is selected, with optional GitHub integration.

The [filesystem tracker](skills/tracker/references/filesystem.md) stores issues
in `.mstack/issues/` with YAML frontmatter and a Markdown body. It applies only
when no issue tracker is selected (or filesystem is explicitly selected), not
when a configured tracker is unavailable.

To work on this repository, follow [AGENTS.md](AGENTS.md). The skill files are
the product under development, not automatically this repository's operating
instructions. [STRUCTURE.md](docs/STRUCTURE.md) maps ownership and files;
[MEMORY.md](MEMORY.md) holds the current review context and open questions.

Start reviewing the workflow with its [lifecycle](skills/workflow/references/lifecycle.md)
and [phase references](skills/workflow/references/phases/). Incomplete contracts
are marked in place. These drafts are not a release ready for installation.

`legacy/backlog/` is frozen historical source material from
[mikemajara/skills](https://github.com/mikemajara/skills/tree/2ba433139fc2e102ad2f231de2c2d79f6ea12a06/skills/backlog),
retained for comparison while developing the new skills. It is not a maintained
implementation of mstack. Import history is preserved in Git, including
`backlog-import-pr10`, `feature-workflow-import` and
`feature-workflow-claude-import`; extraction details remain in this repository's
file history.
