# Working on mstack

## Repository guidance and skill content

This repository authors a delivery workflow. Its draft skills are the product
being developed; they do not automatically govern development of this repository.
Follow this `AGENTS.md` and the user's current instructions when working here.
Read `README.md`, `MEMORY.md` and `docs/STRUCTURE.md` before making changes.

The three skill roots are `skills/mstack-workflow/`, `skills/mstack-orchestrator/` and
`skills/mstack-tracker/`. `docs/STRUCTURE.md` maps ownership and files; behavior belongs
in the owning skill. Root `MEMORY.md` holds this project's continuity. The
memory convention described in `skills/mstack-workflow/references/memory.md` is draft
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
convention in `skills/mstack-workflow/SKILL.md`. Keep lifecycle validity separate from
role scope, harness tools and concrete tracker mappings. For each forward
handoff, the source's exit must supply the destination's entry prerequisites
and preserve required outputs. Check return paths separately; direct entry
may have different prerequisites from a handoff. Keep overlapping entry and
exit requirements consistent so each phase reference stands alone.

## Versioning and release notes

Release mStack as one versioned set. The `version` field in each of the three
skill frontmatters must match; the starting version is `0.1.0`. Apply one
version bump to all three skills for every release, including skills whose
behavior does not change.

The release set contains all three skill directories under `skills/`.
Preserve their names and sibling layout in any distribution or installation so
relative cross-skill references resolve to the same version. Do not publish
independently versioned skill artifacts.

Use these project bump rules: patch for corrections or clarifications with no
intended behavior change; minor for compatible additions; major for changes
that require existing users or agents to adapt. Record the reason for the bump
in the release entries.

Each skill has its own `CHANGELOG.md`. For every release, add an entry to all
three, including an explicit no-change entry when that skill's behavior is
unchanged. Every entry must summarize changes across all three skills and state
how each affects the skill whose changelog it appears in, including whether
that skill must adapt. State explicitly when no adaptation is needed; readers
must not infer compatibility from an omitted change. Keep the three entries at
the same version and consistent about cross-skill effects.

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
