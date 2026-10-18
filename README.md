# mstack

A harness-neutral delivery stack for small software projects, currently being
developed as three draft skills:

- [mstack-workflow](skills/mstack-workflow/SKILL.md) holds the rules: phases, gates, transitions and memory.
- [mstack-orchestrator](skills/mstack-orchestrator/SKILL.md) coordinates stories through those phases.
- [mstack-tracker](skills/mstack-tracker/SKILL.md) performs record operations, with filesystem and GitHub integrations.

These skills guide behavior; access is controlled by the environment the actor
runs in. Workflow can guide an actor directly without the other skills. Tracker
and orchestrator follow workflow; orchestrator uses tracker for record
operations. Local issue storage removes the need for a hosted issue tracker;
delivery still requires a PR and independent QA. By default, orchestrator
works on existing stories and stops after passing QA to propose merge.

Target projects use this layout; directories and files are created as needed:

```text
.mstack/
  memory.md    # Project continuity
  config.json  # Optional project settings
  issues/      # Local stories when no issue tracker is selected
```

The [filesystem tracker](skills/mstack-tracker/references/filesystem.md) stores issues
in `.mstack/issues/` with YAML frontmatter and a Markdown body. It applies only
when no issue tracker is selected (or filesystem is explicitly selected), not
when a configured tracker is unavailable.

To work on this repository, follow [AGENTS.md](AGENTS.md). The skill files are
the product under development, not automatically this repository's operating
instructions. [STRUCTURE.md](docs/STRUCTURE.md) maps ownership and files;
[MEMORY.md](MEMORY.md) holds the current review context and open questions.

Start reviewing the workflow with its [lifecycle](skills/mstack-workflow/references/lifecycle.md)
and [phase references](skills/mstack-workflow/references/phases/). Incomplete contracts
are marked in place. The target first release is `0.0.1`. Gate review,
packaging checks and a supervised story walkthrough remain before release;
these drafts are not ready for installation.

`legacy/backlog/` is frozen historical source material from
[mikemajara/skills](https://github.com/mikemajara/skills/tree/2ba433139fc2e102ad2f231de2c2d79f6ea12a06/skills/backlog),
retained for comparison while developing the new skills. It is not a maintained
implementation of mstack. Import history is preserved in Git, including
`backlog-import-pr10`, `feature-workflow-import` and
`feature-workflow-claude-import`; extraction details remain in this repository's
file history.
