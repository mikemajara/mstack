---
entry: false
output: verdict
forward: integration
returns: [implementation, refinement]
---

# QA

Draft: detailed gate questions and procedure are not specified.

## Entry prerequisites

A reviewable change satisfying [implementation][phase:implementation] [exit
requirements](implementation.md#exit-validation), an identified specification and
executable verification scenarios.

## Required outputs

A verdict connecting every acceptance criterion to scenario results and retained
evidence, identifying tested change/specification versions. Follow
[verification](../verification.md).

## Exit validation

Required criteria and scenarios must pass with sufficient, current evidence.
Missing required evidence blocks a pass. Detailed verdict format and validation
questions remain unspecified.

## Valid destinations

[Integration][phase:integration] after a validated pass and its entry prerequisites.
[Implementation][phase:implementation] for code defects; [refinement][phase:refinement]
for specification holes. Otherwise continue [QA][phase:qa] or hold it
[blocked][status:blocked] under [lifecycle](../lifecycle.md).
