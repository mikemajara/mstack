# cmux integration

Use runtime `cmux` with a configured CLI harness. Follow the parent
[execution contract](../SKILL.md#execution-integration-contract) and
[configuration](configuration.md). Workflow and tracker remain the sources for
phase requirements and work records.

The macOS app and cmux TUI are separate command surfaces; select one and do not
mix their CLI grammars. Draft: no live run has validated a selected installed
interface/harness pairing. Before unattended dispatch, confirm launch readiness
and a way to observe task completion. Default to `drain`; configure hard halt
only when the selected harness can stop the process and confirm its termination.
Closing a pane or terminal alone does not prove process-tree cancellation.

## Availability and target

Select `execution.runtimeOptions.interface` as `macos` or `tui`. Resolve the
control socket/session and assigned checkout, and inspect the installed CLI's
help before using that interface. Connecting to a control endpoint is separate
from installing or starting cmux. Do not broaden socket access to make a
dispatch succeed.

cmux owns terminals; the harness supplies model flags, prompt submission and
task completion semantics. Verify those for `agent-cursor`, `codex`, `fx` or
another configured harness before launch. A terminal container does not itself
provide filesystem isolation or a confirmed model selection.

## macOS app mapping

Use the [macOS CLI reference](https://cmux.com/docs/api) and the project's
[maintained CLI contract](https://github.com/manaflow-ai/cmux/blob/main/docs/cli-contract.md).
`runtimeOptions.socket` selects the control endpoint. Scope commands to actual
workspace and surface IDs, using JSON responses where available.

| Operation | macOS CLI mapping |
| --- | --- |
| Inspect | `list-workspaces`, `list-panels`, and `tree` |
| Launch | `new-split` with the prepared harness command, or explicit input to a confirmed available shell surface. |
| Observe | `read-screen` and verified harness receipts/session state. |
| Continue | `send` plus `send-key` to the confirmed same agent surface. |
| Halt | A verified harness interrupt/exit operation through targeted terminal input; confirm the resulting process state. |

Example operation forms, after resolving the target IDs and prepared command:

```bash
cmux --socket "$socket_path" list-workspaces --json
cmux --socket "$socket_path" list-panels --workspace "$workspace_id" --json
cmux --socket "$socket_path" new-split right --workspace "$workspace_id" --surface "$source_surface" --command "$launch_command" --json
cmux --socket "$socket_path" read-screen --workspace "$workspace_id" --surface "$worker_surface"
cmux --socket "$socket_path" send --workspace "$workspace_id" --surface "$worker_surface" "$mandate"
cmux --socket "$socket_path" send-key --workspace "$workspace_id" --surface "$worker_surface" enter
```

The initial command is interpreted by the terminal's shell. Construct it from
trusted executable/argument values with shell quoting; keep the mandate as
data. Retain the created surface identity before sending further input. Sending
input does not establish readiness or task completion. A shell can remain open
after its initial command exits.

Before submitting a mandate, verify that the intended harness owns the target
terminal and is ready for input. Without that confirmation, input could reach
the underlying shell. Use the verified harness readiness mechanism for both
interfaces; terminal creation alone is not a startup acknowledgment.

## TUI mapping

Use the [public TUI CLI contract](https://github.com/manaflow-ai/cmux/blob/main/cmux-tui/spec/cli.md).
`runtimeOptions.session` or `runtimeOptions.socket` selects the existing owner.
Use opaque returned IDs; target names and `current` are context-dependent.

| Operation | TUI CLI mapping |
| --- | --- |
| Inspect | `workspace list`, `terminal list`, `agent list` |
| Launch | `workspace <id> run -- <harness argv...>` |
| Observe/wait | `terminal <id> screen read`, `terminal <id> process show` or supported bounded process waits. |
| Continue | `terminal <id> write` and `terminal <id> keys` |
| Halt | Targeted interrupt keys plus a verified harness stop/exit confirmation. |

```bash
cmux --session "$session_name" workspace list --json
cmux --session "$session_name" workspace "$workspace_id" run -- "${harness_argv[@]}"
cmux --session "$session_name" terminal "$terminal_id" screen read
cmux --session "$session_name" terminal "$terminal_id" process show --json
cmux --session "$session_name" terminal "$terminal_id" write --text "$mandate"
cmux --session "$session_name" terminal "$terminal_id" keys enter
cmux --session "$session_name" terminal "$terminal_id" keys ctrl+c
```

Resolve the returned terminal and associated process identity from launch. A
process receipt describes that process, not every subprocess or the workflow
outcome. For an uncertain creation, use the installed interface's supported
creation-reconciliation operation before trying again.

## Waiting, halt and cleanup

Use harness completion events or bounded observations that leave the shared
run controls usable. Notifications and terminal text alone do not establish a
validated phase result. Apply the parent reconciliation procedure before
continuation, fallback or workspace reuse.

On halt, close work dispatch and interrupt only workers owned by this run.
Retain controller admission through reconciliation and checkpointing. Confirm
the harness task and relevant processes stopped; input acknowledgment is not
that confirmation. If confirmation is unavailable, report and retain uncertain
ownership. Deadline cancellation requires that capability; draining follows
the shared time-limit policy.

Closing a pane, detaching a client or stopping the whole cmux session is not a
portable per-worker cancellation procedure. Reconcile outputs through tracker
before cleaning up run-owned terminal resources under project policy.
