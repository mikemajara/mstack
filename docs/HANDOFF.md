# Session handoff — 2026-10-17

Authoring context for continuing here. Not distributed skill content or an
instruction to install or activate anything.

## Start here

Work in `/home/miguel/github/mikemajara/mstack`, not the sibling skills repo.
Read `AGENTS.md`, [MEMORY.md](../MEMORY.md) and [STRUCTURE.md](STRUCTURE.md).
The structure now reflects the final three skills, replacing the older
four-skill proposal that was still present during the conversation.

## Why this session happened

Backlog grew to mix phase definitions, verification, scheduling, tracker
commands and Herdr mechanics. The goal is a small, strict stack usable manually
or through automation, preserving valuable backlog behavior and selected
pstack verification practices without shipping its whole Cursor-oriented suite
or maintaining two conflicting active workflows.

The practical goal is bounded background work that advances eligible items,
leaves verified changes for review and reports results. One blocked item must
not stop the queue. One active lane does not mean one issue for a whole run.
Time, quota and model settings are runtime policy, not validation rules.

## Final clarifications

- Three skill roots: workflow, orchestrator, tracker; no extra router, backlog
  or delivery skill. Phase procedures are references under workflow.
- Every phase specifies prerequisites, exit validation and valid destinations.
  Work items supply concrete scope, AC and scenarios. The process is actor-
  and tool-independent; a human can use it without automation.
- Implementation done means ready for QA. QA done means required evidence
  supports the identified change against the identified specification. Merge
  and deployment are separate facts. In the discussed GitHub mapping, close
  after QA passes and merge is complete, not at implementation completion.
- A work item must provide a usable QA brief and traceable evidence so a
  human or agent can understand the judgment without the original chat.
  There is no weaker human, agent or autonomous verification standard.
- Missing evidence blocks; defects return to implementation and spec holes
  to refinement. Green checks or metadata are insufficient if they do not
  demonstrate the required behavior.
- Default project memory belongs to workflow. This repo now uses `MEMORY.md`;
  the reusable read/capture/promotion procedure still needs authoring.
- Runtime permissions are separate from QA procedure. Safe disposable test
  checks are allowed; production and reserved migration commands are protected.
- Public skill content must be operational, not our design dialogue. A concrete
  GitHub adapter does not narrate future replacement with another tracker.

## What exists and what does not

The private repository has preserved backlog history, an unchanged archive,
authoring guidance, project memory and the structure specification.
[PROVENANCE.md](PROVENANCE.md) records source commits and archival tags,
including earlier feature-workflow names. No new `workflow/SKILL.md`,
`orchestrator/SKILL.md` or `tracker/SKILL.md` exists yet.

Prior hardening was published in
[skills PR #10](https://github.com/mikemajara/skills/pull/10) and
[server-docker PR #2](https://github.com/mikemajara/server-docker/pull/2).
At that point 73 isolated tests passed and independent reviews confirmed
fixes for dirty-tree evidence, stale/replayed QA, cancellation/process cleanup,
state reconciliation and issue-local delivery holds. This did not establish
successful live autonomous delivery. Current PR, installation and runtime
status was not re-audited for this handoff: inspect it before cutover rather
than assuming historical branches are merged or active.

## Next review unit

Begin with workflow: its actor-free phase flow, entry/exit criteria,
verification contract and default memory behavior. Keep the first draft
focused and review it before authoring all remaining files. Inherited material
is source evidence, not a requirement to retain its labels, mixed ownership
or Herdr-only assumptions.

Then draft orchestrator and tracker against those contracts. Packaging,
runtime ownership/cutover, legacy removal and a supervised pilot are later
explicit steps. The runtime config schema and exact generic evidence schema
are not finalized. Three versus four skills is settled, not an open question.

This capture is for continuity/planning only. Do not launch workers, install
the archive, merge old PRs, access production data or change timers while
wrapping up this session.
