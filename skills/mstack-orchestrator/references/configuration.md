# Project configuration

Use `.mstack/config.json` at the target project root for shared project settings
and optional orchestration defaults. Use an optional, gitignored
`.mstack/config.local.json` in the same root for personal orchestration overrides.
Workflow and tracker settings remain shared; the local file changes only how
work is executed. Tracker owns
the [issue-selection semantics](../../mstack-tracker/SKILL.md#issue-tracker-selection)
of the optional `tracker.issues` key (`github` or `filesystem`). If no issue
tracker is selected, filesystem issues are the default; a configured tracker's
access failure does not select that fallback. PR hosting remains independent.

Draft: this reference defines configuration semantics, not an executable loader.
The runtime/harness mappings must supply concrete launch and control operations.
The durable run-state file format and concurrent controller locking are not
specified. Project memory lives in `.mstack/memory.md` under
[workflow memory](../../mstack-workflow/references/memory.md) and project instructions.

## Resolution and defaults

For orchestration settings, resolve these layers from highest to lowest priority:

1. Explicit current-run instructions.
2. `.mstack/config.local.json`, when present.
3. Shared `.mstack/config.json`, when present.
4. Skill defaults below.

Merge configuration objects by key before resolving role overrides over
`execution`. A local `execution` value replaces the shared value at the same
path, but does not remove explicit shared `roles` settings; override the relevant
role locally when needed. A supplied array replaces the inherited array rather
than appending to it, including model candidates and `run.stories`. Empty arrays
disable that selection. Apply the runtime-option replacement rule below whenever
an overlay changes runtime; do not carry another runtime's options forward.

Both files are optional. A missing local file or an empty local object changes
nothing; a local file can supply execution settings without a shared file. An
unreadable file, malformed JSON, a non-object root (including `null`), invalid
values or unknown selections need correction before dispatch; do not treat them
as absence or silently fall back. Validate each supplied layer even when a
higher-priority layer replaces its values. Structural errors and unknown
configuration keys are fatal configuration problems; a well-formed model
candidate that is unavailable through the selected runtime/harness follows the
configured fallback procedure, not this validation failure rule. Retain the
effective settings and their sources for the run; do not copy local values back
into shared configuration.

| Setting | Skill default |
| --- | --- |
| Orchestrator, research and refinement tier | `high` |
| Implementation and QA tier | `low` |
| Runtime, harness and concrete model IDs | No universal default; resolve from project settings or the current request. |
| Current-session work | No separate runtime is required when an actor performs phases directly. Launch settings are required when starting separate workers. |
| Model fallback order | Only the configured candidate order; a single model has no fallback. |
| Deadline behavior | `drain`: no new dispatches; admitted workers may finish. |
| Issue tracker | `filesystem` when no issue tracker is selected; see tracker. |

`medium` is available for role overrides or explicitly configured fallback
candidates. High/medium/low are configurable selection tiers; the same model
need not occupy the same tier in different projects or harnesses. Runtime
references do not redefine the role defaults. An orchestrator tier applies
when launching the controller too; an existing session cannot be assumed to
support changing its own model.

## Local override boundary

Accept only these top-level sections in `.mstack/config.local.json`:

| Section | Permitted local settings |
| --- | --- |
| `execution` | `runtime`, `harness`, `delegation`, `runtimeOptions` |
| `roles` | The defined workflow roles and `orchestrator`, each with `runtime`, `harness`, `delegation`, `runtimeOptions`, `tier` |
| `models` | Harness-specific tier mappings and model candidates under the model-selection contract below |
| `run` | `stories`, `maxDurationMinutes`, `onDeadline`, within the requested scope |

Reject other sections or unsupported keys with their configuration path and
reason. In particular, `tracker` and `workflow` are not local overrides, even
when they repeat shared values. Runtime options and model options must be
understood by the selected integration/harness, not interpreted as arbitrary
commands. Local settings cannot change the system of record, workflow gates,
verification requirements, role authority or project instructions. A run setting
cannot grant permission to create stories, merge or close them, or expand the
current request's scope. Tracker's explicit-request selection rules are unchanged.

Keep the local file out of version control. In the target project's root
`.gitignore`, add the exact rule (preserving existing rules):

```gitignore
/.mstack/config.local.json
```

Do not ignore the whole `.mstack/` directory: shared configuration, memory and
filesystem issues still belong to the project. Verify the local path is ignored
and not tracked before writing personal settings; an ignore rule does not
untrack an existing file. If it is already tracked, report that condition and
obtain an explicit migration decision rather than silently removing it from Git.
Do not write credentials into either configuration file.

Each teammate or checkout may use different execution overrides against the same
workflow and tracker. This does not add concurrent controller admission or tracker
locking: the existing single-controller/single-writer limits still apply. The
local file is per checkout, not per harness session. Two sessions in that checkout
read the same file; use explicit run instructions for session-specific differences.
Workers inherit their controller's resolved assignment and limits, rather than
re-resolving a different local file in their worktree. Retain effective settings
for resume; editing the file does not silently reconfigure an active run or
clear a halt. Changes on resume require explicit current-run instructions and
revalidation before further dispatch; they do not reset the retained start time,
release an active worker or silently clear a halt.

## Configuration shape

```json
{
  "execution": {
    "runtime": "herdr",
    "harness": "codex"
  },
  "roles": {
    "orchestrator": { "tier": "high" },
    "research": { "tier": "high" },
    "refinement": { "tier": "high" },
    "implementation": { "harness": "agent-cursor", "tier": "low" },
    "qa": { "harness": "agent-cursor", "tier": "low" }
  },
  "models": {
    "codex": {
      "high": ["PREMIUM_MODEL_ID", "CONFIGURED_FALLBACK_ID"],
      "medium": ["MEDIUM_MODEL_ID"],
      "low": ["LUNA_MODEL_ID"]
    },
    "agent-cursor": {
      "low": ["COMPOSER_MODEL_ID", "GROK_MODEL_ID"]
    }
  },
  "run": {
    "stories": ["STORY_REFERENCE_1", "STORY_REFERENCE_2"],
    "maxDurationMinutes": 120,
    "onDeadline": "drain"
  }
}
```

The model IDs and story references above are placeholders. Use identifiers
available through the selected harness/account and the selected story
integration. A filesystem story reference identifies the project and its
`.mstack/issues/<number>.md` path in the authoritative checkout, not a GitHub
issue number. The example omits `tracker.issues`, so it uses the default unless
the current request selects a tracker. To select GitHub Issues, add
`"tracker": { "issues": "github" }` at the top level; no tracker configuration
file or key is required to use filesystem issues.

`execution` selects default runtime and harness. Each role can override
`runtime`, `harness`, `delegation`, `runtimeOptions` and `tier`. Resolve the role
over `execution`, then look up
`models[resolvedHarness][resolvedTier]`. Harness names such as `agent-cursor`,
`codex` and `fx` are configuration identities; the integration must map them to
actual executable/API operations. Naming a harness does not install it or
provide an operation mapping. Account access follows the harness's configured
authentication; a model ID does not transfer subscription access between clients.

`execution.delegation` defaults to `agent`. Set it to `subagent` only for a
Cursor session with native delegation support; use the
[subagent mapping](cursor-subagents.md) within that session's actual runtime.
It does not make a native task a separate hosted agent or change its account.

`execution.runtimeOptions` holds target settings owned by the runtime reference:
[Herdr](herdr.md) may use `session` or `machine`, [Cursor Cloud](cursor-cloud.md)
uses its `repos` or `env` target, and [cmux](cmux.md) uses `interface` plus a
`socket` or TUI `session`. Resolve these before dispatch; do not pass arbitrary
keys as CLI flags or mix one runtime's target settings into another's operation.
When an override changes runtime, replace its `runtimeOptions` with settings
for that runtime, or an empty object if none are supplied. Inherit and merge
runtime options by key only while the runtime remains the same.

### Local override example

With the shared example above, this `.mstack/config.local.json` keeps its runtime,
role harness choices, tracker selection and other tiers, but replaces the Codex
high-tier candidates and the run's duration:

```json
{
  "models": {
    "codex": {
      "high": ["LOCAL_PREMIUM_MODEL_ID"]
    }
  },
  "run": {
    "maxDurationMinutes": 45
  }
}
```

The effective high-tier list contains only `LOCAL_PREMIUM_MODEL_ID`, not the
shared candidates. An explicit run instruction setting the duration to 15 minutes
wins over the local 45 and shared 120. The shared story series is retained unless
explicitly replaced within the requested scope. The model ID is a placeholder;
validate it through the selected harness before launch. To change runtime or
harness locally, use the same `execution`/`roles` shape as shared configuration
and supply compatible options and model mappings.

## Model selection and fallbacks

For the resolved runtime and harness, consider candidates in configured order.
A candidate is a model ID string or an object with an explicit `model`, and
optional `runtime`, `harness`, `delegation`, `runtimeOptions` and `options`
overrides. `options` holds model settings understood by that harness, such as
reasoning effort. Validate support
before dispatch; report unsupported candidates and continue only through the
remaining configured alternatives.

Objects allow an intentional change of model, runtime or harness. For example:

```json
{
  "high": [
    "PREFERRED_MODEL_ID",
    {
      "harness": "agent-cursor",
      "model": "EXPLICIT_ALTERNATIVE_ID"
    }
  ]
}
```

The example is one harness's tier mapping. An alternative inherits the
resolved runtime/harness unless overridden, and selects its `model` directly;
it does not recursively follow another tier's fallbacks. Moving to a cheaper
or more capable model is allowed only when that exact alternative is configured
or directed for the run. List each candidate once per attempt sequence.

Distinguish execution failures from worker outcomes. A workflow
return or missing product input is handled under workflow; it does not by
itself call for another model. If execution fails or stalls, reconcile the
current worker and retained outputs before admitting a configured alternative.
Do not duplicate a task while an earlier launch may still be active. When the
candidate list is exhausted, report the execution hold and resume condition.

## Run controls

| Setting or signal | Meaning |
| --- | --- |
| `run.stories` | Optional ordered series of story references. Dispatch only within that series. If omitted, use the backlog scope requested for this run; omission does not expand that scope. |
| `run.maxDurationMinutes` | Optional positive elapsed-time admission limit, measured from the retained run start. Resuming the same run does not restart its clock. |
| `run.onDeadline` | `drain` by default; `halt` requests cancellation of active workers at the deadline and requires runtime support. |
| Explicit user halt | Immediately closes admission and requests cancellation. The runtime maps the stop signal to its worker control mechanism. |

Configured stop conditions combine: reaching any one closes admission. An empty
story series has no work. For a series, stop when no further eligible action
remains within it; retain unfinished stories and their reasons. Workflow
determines completed, returned and blocked outcomes.

An omitted duration means no elapsed-time limit. Other run controls still
apply. Completion of the requested work, an explicit halt or any
configured limit still ends dispatch. A bounded series and a two-hour limit
can be used together. With `drain`, the last admitted worker can finish after
two hours; `halt` is the choice for requesting cancellation at that point.

Recheck stop state, elapsed time and story scope before every new action,
including configured fallback attempts.
