# Project configuration

Use one `.mstack/config.json` at the target project root for runtime, harness,
models, role overrides, run controls and issue-tracker selection. Tracker owns
the [issue-selection semantics](../../tracker/SKILL.md#issue-tracker-selection)
of the optional `tracker.issues` key (`github` or `filesystem`). If no issue
tracker is selected, filesystem issues are the default; a configured tracker's
access failure does not select that fallback. PR hosting remains independent.

Draft: this reference defines configuration semantics, not an executable loader.
The runtime/harness mappings must supply concrete launch and control operations.
The durable run-state file format and concurrent controller locking are not
specified. `.mstack/` does not relocate project memory by itself; follow
[workflow memory](../../workflow/references/memory.md) and project instructions.

## Resolution and defaults

Resolve explicit current-run instructions over project configuration, then use
the defaults below for omitted settings. Merge objects by key; a supplied
model-candidate array replaces that array rather than appending inherited
candidates. Empty arrays disable that selection. Malformed configuration and
unknown selections need correction; they are not missing values eligible for
implicit defaults. Retain the effective settings used for the run.

| Setting | Skill default |
| --- | --- |
| Orchestrator, research and refinement tier | `high` |
| Implementation and QA tier | `low` |
| Runtime, harness and concrete model IDs | No universal default; resolve from project settings or the current request. |
| Model fallback order | Only the configured candidate order; a single model has no fallback. |
| Deadline behavior | `drain`: no new dispatches; admitted workers may finish. |
| Issue tracker | `filesystem` when no issue tracker is selected; see tracker. |

`medium` is available for role overrides or explicitly configured fallback
candidates. High/medium/low are configurable selection tiers; the same model
need not occupy the same tier in different projects or harnesses. Runtime
references do not redefine the role defaults. An orchestrator tier applies
when launching the controller too; an existing session cannot be assumed to
support changing its own model.

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
| `run.stories` | Optional ordered series of story references. Dispatch only within that series. If omitted, use the backlog scope authorized for this run; absence does not authorize an unrestricted backlog. |
| `run.maxDurationMinutes` | Optional positive elapsed-time admission limit, measured from the retained run start. Resuming the same run does not restart its clock. |
| `run.onDeadline` | `drain` by default; `halt` requests cancellation of active workers at the deadline and requires runtime support. |
| Explicit user halt | Immediately closes admission and requests cancellation. The runtime maps the stop signal to its worker control mechanism. |

Configured stop conditions combine: reaching any one closes admission. An empty
story series has no work. For a series, stop when no further eligible action
remains within it; retain unfinished stories and their reasons. Workflow
determines completed, returned and blocked outcomes.

An omitted duration means no elapsed-time limit, not permission to exceed other
run controls. Completion of the authorized work, an explicit halt or any
configured limit still ends dispatch. A bounded series and a two-hour limit
can be used together. With `drain`, the last admitted worker can finish after
two hours; `halt` is the choice for requesting cancellation at that point.

Recheck stop state, elapsed time and story scope before every new action,
including configured fallback attempts.
