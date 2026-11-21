# OpenBot integration

Use OpenBot as the execution runtime, with one project channel, a PM agent as
its lead/controller, and separate agents as workers. Follow the parent
[orchestrator contract](../SKILL.md) and [configuration](configuration.md).
Workflow owns phase gates; tracker owns canonical stories, PRs and evidence.
The channel coordinates work; it does not replace the selected tracker.

This is an unreleased instruction mapping, not an installed integration or a
claim of validated autonomous operation. Discover the current OpenBot tool
schemas before setup or dispatch. Capabilities and permissions vary by
installation. This reference supplies no account, agent, channel, model or
workspace defaults tied to a particular user's environment.

## Set up the project

1. Identify the repository, authoritative checkout, selected story tracker,
   requested scope and allowed actions. Inspect existing project channels with
   `openbot.list_channels`; reuse the matching channel rather than create a
   second project home. Resolve ambiguity before changing channel membership.
2. Inspect available agents using `openbot.list_agents` and their setup using
   `openbot.read_agent`. Select by role, capability, access and availability,
   not by name. Recommend a PM lead, phase workers and an independent QA actor.
   The maintainer advising on adoption recommends assignments, models,
   reasoning settings and permitted fallbacks to the consulting user or agent;
   do not require a particular maintainer identity or invent missing choices.
3. Inspect available providers/models with `openbot.list_models`. Check each
   selected agent's configured provider, account, model, reasoning effort and
   fallback settings against the resolved role policy. A message assigning a
   model does not change an agent's configuration. Do not mutate an existing
   reusable agent's settings just for this run: configuration changes are
   separate, authorized setup work, and can affect its other projects.
4. With setup authority, use `openbot.create_channel` or
   `openbot.update_channel` to establish the project channel, its members and
   PM lead. Missing roles need an approved provisioning decision; do not
   silently create agents. Keep installation-specific bindings in project
   execution settings or explicit setup instructions, not in distributed skills.
5. Put the repository, tracker reference, role responsibilities, workspace
   policy, run limits and authority boundaries in channel instructions. Link
   to the same-version mstack skills and project instructions. The PM assigns
   and reconciles work; workers return evidence and proposed record updates.
   Independent QA must be a different actor from the implementer. Shared
   access to a checkout or browser does not imply safe concurrent use.
6. Read back the channel and agent setup. Check access to the canonical records
   and assigned workspaces, then establish exclusive controller admission
   before dispatch. Channel lead selection routes requests; it does not prove
   there is only one active controller for the project.

The setup result identifies the project channel, PM lead, role-to-agent and
model choices, checkout/isolation policy, canonical tracker, capability limits
and remaining prerequisites. Keep personal execution bindings in the existing
local-override layer when appropriate. Credentials never belong in those files.

## Configuration binding

Select `execution.runtime: "openbot"`, `execution.harness: "openbot-agent"` and
`execution.delegation: "agent"`. This harness identity means an existing OpenBot
agent, not a terminal executable. Use these runtime options:

| Option | Meaning |
| --- | --- |
| `channelId` | Exact project channel returned by discovery. Required for dispatch. |
| `agents` | Object mapping `orchestrator`, `research`, `refinement`, `implementation` and `qa` to discovered agent IDs for the roles the run uses. |

Reject unknown options or roles and resolve every dispatched role before use.
The resolved orchestrator binding must match the channel lead and the actor
performing controller work. Role overrides must still resolve to this same
project channel and admitted PM. An unresolved role blocks that dispatch;
workers need not all be different actors, except implementation and QA.
Bind workers to actual channel members. Values are installation-specific; the
shared configuration policy governs whether they belong in shared settings,
local overrides or explicit run instructions. Switching runtime replaces the
previous runtime's options under the existing configuration rule.

Use `models["openbot-agent"][tier]` for configured model candidates. Candidate
model IDs and supported reasoning options must match the selected agent's
verified setup; the current mapping selects an existing agent, not a
per-message model override. Do not switch an agent's account or alter its
fallbacks during dispatch. If the binding cannot satisfy a candidate, consider
only configured compatible candidates. Reassignment requires an explicit role
binding, current membership and reconciliation of previous ownership.
Configured fallback models do not by themselves authorize another agent.
Record the actual model when observable; agent configuration is evidence of
intended settings, not proof of the model used by a completed turn.

## Execution operations

Use channel operations only while handling that project's channel task. A
private conversation is not channel context: discovering a channel does not
let the caller use `channel_assign` there. Finish setup and direct the user to
start the scoped request in the project channel. Do not substitute private
agent messages for the shared project run.

| Operation | OpenBot mapping and required observation |
| --- | --- |
| Inspect | Read channels and agent setup, plus `openbot.channel_history` in the active channel. Identify the PM, members, current request/task IDs, assignments, resource use and unresolved workers. An agent's global ready/working status is not a project task outcome. |
| Admit | Require the installation's verified exclusive project admission mechanism. Retain the project/controller identity and release condition. A PM title, channel lead or shared instructions alone are not a lock. If exclusivity cannot be established, refuse unattended dispatch. |
| Launch | PM calls `openbot.channel_assign` with `recipientAgentId`, a complete `task`, `expectedResult`, and actual `sourceMessageIds`. Retain the returned task identity and observed assignment outcome; never invent an ID or blindly retry an uncertain delivery. |
| Observe or wait | Inspect shared history and receive the assigned result through channel continuation. End the current turn to await it; do not poll a terminal/subagent wait or resubmit the assignment. Distinguish a yielded turn from completion of the logical controller session. |
| Continue | Use `openbot.channel_reply` on a real member message for clarification under its current reply limits. A new phase is a new admitted assignment after reconciliation. Do not treat an asynchronous reply as permission to restart a halted run. |
| Halt | For a user-started channel run that the controller owns, `openbot.channel_cancel` targets that request or subtask ID, subject to its current-context restrictions. It is unavailable for some contexts, including the current task itself. Inspect the result and reconcile affected work; cancellation does not prove external jobs or side effects stopped. |
| Reconcile and release | Workers return the assigned result once with `openbot.channel_result`. PM checks evidence against workflow and current tracker/PR state, writes authorized canonical updates and records the checkpoint before releasing its own admission/resources. |

An assignment includes the canonical story and phase, scope/acceptance criteria,
current commit and branch, permitted actions, resolved agent/model settings,
workspace paths, required output/evidence, limits and the condition for
returning control. Pass enough context for a worker to act without assuming it
can read private PM chat. Share only the data the task needs.

Use `resources` on `channel_assign` for the shared browser and each changed
workspace according to the discovered schema; use actual returned dependency
IDs for `dependencies`. Resource serialization is narrower than project
controller admission and does not establish isolation. Separate worktrees or
serialized writes must follow project policy. Refuse conflicting writes if the
available resource mechanism cannot enforce that policy.

## Asynchronous control and recovery

The PM retains logical controller ownership while a worker is pending across
turns. Reconcile the result and dispatch the next eligible phase without a new
user prompt when scope and controls permit. Do not report the run complete
because a turn yielded. A second request or scheduled invocation must inspect
that ownership and the existing task tree before dispatch.

OpenBot task routing and a lead agent do not, by themselves, demonstrate the
parent contract's exclusive admission, recovery or deadline enforcement. Verify
those mechanisms in the installation. After confirmed controller termination,
a fresh controller enters recovery-only mode while workers remain active or
uncertain. Read canonical records, task history and retained artifacts before
retrying, releasing claims or reusing resources. Late results are evidence to
reconcile, not automatic workflow transitions.

Use `drain` by default: close new dispatch at the deadline, reconcile admitted
workers and checkpoint. An idle PM cannot be assumed to wake exactly at a
deadline. Deadline/halt guarantees require a verified supervisor and observable
cancellation for the selected task context and any external jobs. If missing,
report the unsupported control before dispatch; do not promise a hard stop or
invent usage telemetry. Schedule recurring work only through OpenBot routines,
when separately authorized. This integration creates no routine or timer.

## Supervised validation

Before claiming unattended readiness, retain observable evidence for:

- One project channel with a PM lead, configured role/model bindings and
  separate implementation/QA actors, with no user-specific constants in mstack.
- Denial of concurrent controller admission and conflicting workspace writes.
- A real phase assignment, a returned evidence-bearing result and the next
  eligible dispatch after the asynchronous wait, without another user prompt.
- A held story alongside other eligible work, with the eligible work continuing.
- Configured-model incompatibility, unavailable workers and uncertain assignment
  delivery, without invented fallback, duplicate dispatch or lost ownership.
- A controlled stop/deadline, observed worker/external-job outcomes, a durable
  checkpoint and fresh recovery-only admission while an outcome is uncertain.
- Independent QA against the actual tested commit, with canonical tracker/PR
  reconciliation rather than completion inferred from chat or agent status.

Use the selected target project for the trial. Record setup-only inspection,
document checks and live execution separately. Missing admission or control
capabilities remain validation blockers; a successful channel exchange alone
does not establish autonomous reliability.
