# Working on mstack

## Repository guidance and skill content

This repository authors a delivery workflow. Its draft skills are the product
being developed; they do not automatically govern development of this repository.
Follow this `AGENTS.md` and the user's current instructions when working here.
Read `README.md`, `MEMORY.md` and `docs/STRUCTURE.md` before making changes.

The three skill roots are `skills/workflow/`, `skills/orchestrator/` and
`skills/tracker/`. `docs/STRUCTURE.md` maps ownership and files; behavior belongs
in the owning skill. Root `MEMORY.md` holds this project's continuity. The
memory convention described in `skills/workflow/references/memory.md` is draft
product behavior, not an instruction to move this repository's memory.

## Developing references

Work on the reference or boundary requested by the user. Read related contracts
to check consistency, but do not fill adjacent scaffolds merely to make the
package look complete. The user owns product decisions; distinguish agreed
behavior from proposals and unresolved questions.

For each phase reference, make entry prerequisites, required outputs, exit
validation and valid destinations clear. Separate mandatory gate questions and
their pass conditions from optional investigation prompts. Leave unknown rules
explicitly incomplete rather than inventing requirements or treating gaps as a
pass. Use examples to check general rules, not to encode the example as policy.

When a change affects phase names or transitions, check the shared vocabulary,
lifecycle and affected phase contracts together. Use the attribute naming
convention in `skills/workflow/SKILL.md`. Keep lifecycle validity separate from
execution authority, harness tools and concrete tracker mappings.

Distributed skill content should guide its eventual user. Keep authoring
discussion, review progress and open design questions in `MEMORY.md`, rather
than in the skill instructions or concrete integration references. Mark draft
limitations in the affected reference.

## Keeping context useful

Keep a file only when it helps current development or preserves needed source
material. When reviewing it, ask: is it useful now, does it contradict maintained
guidance, and is it outdated? Correct or remove stale claims and consolidate
duplicate context. Keep `README.md` as the entry point and `docs/STRUCTURE.md`
as the ownership map, not additional workflow specifications.

Capture material new decisions, facts, open questions and gotchas in `MEMORY.md`
when they are not already in maintained guidance. Link to that guidance instead
of copying it. Remove superseded entries; promote stable repository guidance
deliberately.

## Boundaries

Keep `legacy/backlog/` unchanged as historical source material. Consult relevant
parts when useful; its instructions are not operating guidance for this repo.
Drafting does not authorize installation or activation. Do not install skills,
run an orchestrator, alter timers, merge existing PRs, remove the original
backlog, or change deployment configuration as part of this authoring work.

Do not publish private workplace material, credentials or environment config.
Preserve imported history; do not rewrite or force-push published history.
