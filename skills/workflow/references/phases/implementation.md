---
entry: true
output: reviewable-change
forward: qa
returns: refinement
---

# Implementation

## Entry prerequisites

Stories in [implementation][phase:implementation] state the goal, what is in scope, what is out of scope, and testable acceptance criteria. Each acceptance criterion has a verification scenario with prerequisites and an expected observable result. The specification must be assesable without the original context.

## Required outputs

Stories in [implementation][phase:implementation] contain the code for the features the specification describes, and that code stays within the specified scope.

## Exit validation

| Required question | Pass condition |
| --- | --- |
| Does the change stay within the specified scope? | Stories in [implementation][phase:implementation] change only what the specification covers. |
| Are all the specified features implemented? | Stories in [implementation][phase:implementation] implement every feature the specification describes. The change is that code or configuration |

## Valid destinations

Stories in [implementation][phase:implementation] leave that phase when both pass conditions are true. The next phase is [QA][phase:qa]. They return to [refinement][phase:refinement] when the specification does not cover the entry prerequisits. Otherwise they continue in [implementation][phase:implementation] or are held [blocked][status:blocked] under [lifecycle](../lifecycle.md).
