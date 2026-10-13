# Backlog history extraction

## Source

- Repository: https://github.com/mikemajara/skills
- Published branch: `fix/backlog-proof-contract`
- Source commit: `2ba433139fc2e102ad2f231de2c2d79f6ea12a06`
- Source directory: `skills/backlog/`
- Source review: https://github.com/mikemajara/skills/pull/10 (open at extraction)
- Extracted history tip: `5d2b5db5fb6b49693d0bbb9449b986d9af8a23bc`
- Extracted commits: 48
- Imported directory tree: `5e3c058f29a3bf38b5afb6f8bf461b16bbb71abf`

## Earlier names retained as historical tags

Backlog was previously named `feature-workflow`. Directory splitting does not
automatically follow those renames, so their histories are retained explicitly:

| Tag | Source directory / tip | Extracted tip | Commits |
| --- | --- | --- | --- |
| `feature-workflow-claude-import` | `.claude/skills/feature-workflow` at `51a1a62beb398eb71a75280a859703d4b5afa154` | `409adfbf0b50922c08072294b3136d17f9454457` | 1 |
| `feature-workflow-import` | `skills/feature-workflow` at `669ce6e5ab25ec39d86aae7a1029bece42cc75ac` | `57a66e53674b02c2ec5b0c9d60c8435e60af08ad` | 7 |
| `backlog-import-pr10` | `skills/backlog` at the source tip above | `5d2b5db5fb6b49693d0bbb9449b986d9af8a23bc` | 48 |

These are historical tags, not three maintained implementations or releases.
All three path histories are accessible with `git log --all`; the pre-rename
segments were not rewritten into main's ancestry after bootstrap publication.
For an older file, inspect its original path in the matching tag, for example
`git log feature-workflow-import -- SKILL.md`.

## Method and boundaries

An isolated clone of the source was checked out at the source commit. No refs,
files or history in the original skills checkout were rewritten.

```sh
git subtree split --prefix=skills/backlog --branch backlog-history
```

A separate checkout was cloned from that branch, then the unchanged files
were mechanically moved to `legacy/backlog/`. Bootstrap documentation is a
new commit after the imported history. The directory tree must equal the
source tree above before publication.

Subtree split produces synthetic commit IDs. Relevant authors, messages,
dates and changes remain in Git history; original IDs and PR discussions
remain in the source repository. This is directory-history preservation,
not a transfer of GitHub issues, PR reviews or the entire skills repository.

The source directory has 31 files. Use rename-following history for a moved
file, for example:

```sh
git log --follow -- legacy/backlog/SKILL.md
```

The root skills changelog and server-docker runtime are outside that source
directory and were not imported. No installed or deployed version changed.
The archive is frozen; removal/cutover of the original skill is a later,
explicitly reviewed operation, not a second maintained implementation.
