---
entry: false
output: verdict
returns: [implementation, refinement]
---

# QA

## Entry prerequisites

Stories in [QA][phase:qa] have code within the specified scope that implements the features the specification describes, and executable verification scenarios for the acceptance criteria.

The change is available in an open PR meeting the
[implementation outputs](implementation.md#required-outputs), linked to the story.

QA is assigned to an actor who did not implement any part of the current
change. Use a different human or a separate agent session/task under the
[actor definition](../../SKILL.md#definitions). A role change or continuation
of the implementation session does not satisfy independence.

## Required outputs

Record the QA verdict, scenario results and retained evidence on the PR. The
verdict connects every acceptance criterion in the story to its scenario results
and evidence, and identifies the tested commit, QA actor and implementing
actors so independence can be checked.
Evidence stored elsewhere must remain accessible through links from the PR.
The story links to the verdict; it does not need a duplicate QA report.

## Exit validation

| Required question | Pass condition |
| --- | --- |
| Was QA performed independently? | The verdict identifies the QA actor and implementing actors, and the QA actor did not implement any part of the current change. |
| Do the required scenarios pass for the tested commit? | The PR records a verdict connecting every acceptance criterion to scenario results and retained evidence, and identifies the tested commit. The required scenarios pass on that evidence. Missing required evidence means this condition is false. |

## Valid destinations

When both pass conditions are true, QA has passed. For delivery, the PR must be
merged before the story closes under [story completion](../lifecycle.md#story-completion).
The story remains in [QA][phase:qa] until merge is confirmed or a non-delivery
closure is recorded under that contract. Changes to the tested commit require
[revalidation](../verification.md#judgment) before merge.
A return to refinement invalidates earlier verdicts under
[specification changes](../lifecycle.md#specification-changes).

Stories return to [implementation][phase:implementation] for a code defect and
to [refinement][phase:refinement] for a specification hole. Otherwise they
continue in [QA][phase:qa] or are held [blocked][status:blocked] under
[lifecycle](../lifecycle.md).
