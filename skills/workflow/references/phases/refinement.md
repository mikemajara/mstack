---
entry: true
output: specification
forward: implementation
returns: research
---

# Refinement

Draft: detailed gate questions and procedure are not specified.

## Entry prerequisites

The [research][phase:research] [exit gate](research.md#exit-validation) is satisfied by
existing evidence or completed [research][phase:research].

## Required outputs

A work specification containing goal, in/out scope, testable acceptance criteria
and verification scenarios with prerequisites and expected observable results.
It must support judgment without the original conversation. Bring recommendations
to whoever handles the decision; identify unresolved choices explicitly.

## Exit validation

Before [implementation][phase:implementation], scope and product decisions needed to
implement and judge the result must be resolved. Required criteria and scenarios must
meet the [verification contract](../verification.md). Detailed validation questions
remain unspecified.

## Valid destinations

[Implementation][phase:implementation] when its entry prerequisites are met.
[Research][phase:research] when missing facts prevent specification. Otherwise continue
[refinement][phase:refinement] or hold it [blocked][status:blocked] under
[lifecycle](../lifecycle.md).
