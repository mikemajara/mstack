---
entry: false
output: verdict
returns: [implementation, refinement]
---

# QA

## Entry prerequisites

Stories in [QA][phase:qa] have code within the specified scope that implements the features the specification describes, and executable verification scenarios for the acceptance criteria.

## Required outputs

Stories in [QA][phase:qa] contain a verdict that connects every acceptance criterion to scenario results and retained evidence, and that identifies the tested change version and the specification version.

## Exit validation

| Required question | Pass condition |
| --- | --- |
| Do the required scenarios pass for the identified versions? | Stories in [QA][phase:qa] connect every acceptance criterion to scenario results and retained evidence, and identify the tested change version and the specification version. The required scenarios pass on that evidence. Missing required evidence means this condition is false. |

## Valid destinations

Stories in [QA][phase:qa] are done with that phase when the pass condition is true. They return to [implementation][phase:implementation] for a code defect. They return to [refinement][phase:refinement] for a specification hole. Otherwise they continue in [QA][phase:qa] or are held [blocked][status:blocked] under [lifecycle](../lifecycle.md).
