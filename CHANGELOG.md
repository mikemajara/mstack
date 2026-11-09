# mstack changelog for adopting projects

Read this when you upgrade a project that uses mstack. Each release says what
is new, what you must change in your project, and what happens to stories
already in flight. The per-skill `CHANGELOG.md` files under `skills/` hold the
detailed cross-skill contract changes.

## Unreleased

### What's new

- Optional, gitignored `.mstack/config.local.json` for personal orchestration
  settings. Explicit run instructions take precedence over local overrides,
  shared configuration and skill defaults, in that order.
- Execution-only local overrides for runtime, harness, role execution choices,
  models and run controls, with object merging, array replacement, runtime-option
  resets and validation. Workflow requirements and tracker selection stay shared.
- Workers inherit resolved assignments and limits. Editing the local file does
  not silently reconfigure an active controller session.

### What to change in your project

- No configuration migration is required. Existing shared settings continue to
  apply when there is no local override. No tracker schema, labels or fields change.
- To use personal settings, add `/.mstack/config.local.json` to the root
  `.gitignore` before writing the local file. Keep shared configuration, project
  memory and filesystem issues versioned; do not ignore all of `.mstack/`.
- If a local override is already tracked, arrange an explicit migration before
  adding personal settings; an ignore rule alone does not untrack it.
- Local files cannot contain `tracker` or `workflow` overrides. Fix invalid or
  unreadable local configuration before orchestration dispatch.

### Stories already in flight

- Story records and QA verdicts need no migration or revalidation solely for this
  addition. Existing workflow gates, controller admission and filesystem locking
  requirements are unchanged.
- Finish an active controller session with its retained settings and checkpoint.
  A later fresh session resolves the local configuration anew, with its own
  elapsed-time limit; an existing halt or uncertain worker still requires
  explicit reconciliation under the session contract.

These are skill configuration semantics, not an executable configuration loader.
Runtime support is unchanged; local overrides do not enable deferred runtimes,
subagents or concurrent controllers. The shared skill version remains `0.2.0`
pending a release decision.

## 0.2.0

### What's new

- **Fix reproduction.** A story of type fix now proves the fix on its PR. It
  records the scenario, the command or steps, and the result on the base commit
  (showing the reported defect) and on the head commit (showing it resolved).
  If reproducing on base is impractical, the PR says why and what substitute
  evidence was used. QA reruns the reproduction on both commits, or judges the
  substitute.
- **Scope and regression check in QA.** Every change in the diff must trace to
  the specification. QA names what the change could affect outside the
  acceptance criteria, in proportion to risk, and runs at least the cheapest
  check that would catch each risk. Only a demonstrated regression fails the
  story; unproven concerns are recorded as notes, and failures that already
  exist on base are recorded separately.
- **Behavior to preserve.** Refinement names existing behavior the change must
  not break, so QA has something concrete to check.

### What to change in your project

1. Update the three installed skills (`mstack-workflow`, `mstack-tracker`,
   `mstack-orchestrator`) to `0.2.0` together. Mixed versions are unsupported.
2. Make sure a fix can be run on an older commit. QA needs a way to check out
   the base commit and run the reproduction there (a test command, a script, or
   browser steps). If your project has no such path, expect fix stories to
   record substitute evidence instead.
3. If your `AGENTS.md`, PR template or QA prompts list what a PR or QA verdict
   must contain, add the fix reproduction (fix stories only) and the scope and
   regression check.
4. No change is needed to `.mstack/config.json`, tracker labels, GitHub or
   Notion fields, or the orchestrator runtime.

### Stories already in flight

- Stories in refinement: add behavior to preserve where a change could affect
  it.
- Stories in implementation of type fix: record the fix reproduction before
  handing off to QA.
- Stories already in QA or waiting to merge under `0.1.0` need a new QA verdict
  that includes the scope and regression check (and, for fixes, the
  reproduction) before they pass.

## 0.1.0

First versioned release of the three-skill set, replacing the deprecated
`backlog` skill. There is nothing to migrate from inside mstack. To adopt it,
follow the steps in the [README](README.md#adopt-in-a-project).
