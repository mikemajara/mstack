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

## Required outputs

Record the QA verdict, scenario results and retained evidence on the PR. The
verdict connects every acceptance criterion in the story to its scenario results
and evidence, and identifies the tested commit and the specification version.
Evidence stored elsewhere must remain accessible through links from the PR.
The story links to the verdict; it does not need a duplicate QA report.

## Exit validation

| Required question | Pass condition |
| --- | --- |
| Do the required scenarios pass for the identified versions? | The PR records a verdict connecting every acceptance criterion to scenario results and retained evidence, and identifies the tested commit and the specification version. The required scenarios pass on that evidence. Missing required evidence means this condition is false. |

## Valid destinations

When the pass condition is true, QA has passed and the PR must be merged before
the story closes, as defined by [story completion](../lifecycle.md#story-completion).
The story remains in [QA][phase:qa] until the merge is confirmed. Changes to the
tested versions require [revalidation](../verification.md#judgment) before merge.

Stories return to [implementation][phase:implementation] for a code defect and
to [refinement][phase:refinement] for a specification hole. Otherwise they
continue in [QA][phase:qa] or are held [blocked][status:blocked] under
[lifecycle](../lifecycle.md).
