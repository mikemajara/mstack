# Cursor native subagent integration

Use when `execution.delegation` is `subagent` and the selected
`agent-cursor` session exposes native delegation. The parent session still
belongs to its actual runtime. Follow the shared
[execution contract](../SKILL.md#execution-integration-contract) and
[configuration](configuration.md); phase requirements remain in workflow.

Draft: tool signatures vary with the current Cursor surface. This mapping
requires inspecting the exposed tool schema; a portable task cancellation
operation and confirmed deadline enforcement have not been established.

## Availability and model selection

Inspect the current session's delegation tool and existing task identities.
Cursor documents [subagents](https://cursor.com/docs/subagents) as separate
contexts, with foreground/background execution and returned agent IDs for
resume. Native tasks use the parent's Cursor access; other harnesses require
their own launch integration.

Resolve the role's model through shared configuration. Pass it through the
native launch tool when supported, or use an existing custom subagent
definition with the selected model. An implicit `inherit` does not implement
an explicitly different model selection. Record the actual model when the
surface exposes it and report uncertainty under the parent contract.

Do not generate or install custom subagent definitions as an incidental part
of dispatch. Their provisioning is separate project setup. Include the task's
required context in the mandate; a separate context is not a separate checkout.

## Operation mapping

| Operation | Current-session mapping |
| --- | --- |
| Launch | Invoke the exposed Task/delegation tool with the admitted mandate and resolved model, using its actual schema. Retain its agent/task ID. |
| Observe/wait | Foreground return, background completion event, or the task's exposed status/output mechanism. |
| Continue | Resume the recorded agent ID through the exposed delegation operation. |
| Halt | Use a cancellation operation for that task if the current surface supplies one; otherwise report unsupported cancellation and any active task. |
| Reconcile/release | Use returned output and retained tracker artifacts through the parent contract before retry or context reuse. |

Background local tasks may expose output under `~/.cursor/subagents/`; use the
path and identity actually returned for the task. Do not assume a local path
works for a cloud session. Default shared-checkout behavior requires attention
to the parent's isolation/admission contract before parallel edits.

A task tool returning a final message is an execution observation. Record and
validate the assigned phase result using workflow and tracker. Resuming a task
retains its context; use a new task for a newly selected model unless the
surface explicitly supports changing it.

## Run controls

Use background delegation or a verified interruptible foreground operation
when the controller must continue receiving stop/deadline signals. Apply the
shared story and elapsed-time controls before each launch or resume.

A user halt closes admission even if task cancellation is unavailable. Report
that limitation and retain active/uncertain task ownership for reconciliation.
Do not promise a hard stop at a deadline without a supported cancellation and
confirmation path. Model fallbacks use the parent's configured-candidate rule.
