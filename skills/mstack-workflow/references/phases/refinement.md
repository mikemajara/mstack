---
entry: true
output: specification
forward: implementation
returns: [research]
---

# Refinement

Draft: gate wording remains under review. An unspecified gate is not a pass.

## Entry prerequisites

Stories in [refinement][phase:refinement] request a change in the project that is to be specified.

## Required outputs

Stories in [refinement][phase:refinement] contain a specification with the goal, what is in scope, what is out
of scope, testable acceptance criteria, and verification scenarios.

Only this phase changes the specification. A return here invalidates earlier
QA verdicts under [specification changes](../lifecycle.md#specification-changes).

## Exit validation

| Required question | Pass condition |
| --- | --- |
| What change is being specified? | Stories in [refinement][phase:refinement] state the goal, what is in scope, and what is out of scope, with testable acceptance criteria. The specification can be judged without the original conversation. |
| Can the result be implemented and judged from these stories? | Stories in [refinement][phase:refinement] resolve the scope and product decisions needed to implement and judge the result. Each acceptance criterion has a verification scenario with prerequisites and an expected observable result. Choices that do not block implementation are identified. |

## Valid destinations

Stories in [refinement][phase:refinement] leave that phase when both pass conditions are true. The
next phase is [implementation][phase:implementation]. They return to
[research][phase:research] when a missing fact prevents the specification. Otherwise
they continue in [refinement][phase:refinement] or are held [blocked][status:blocked]
under [lifecycle](../lifecycle.md).
