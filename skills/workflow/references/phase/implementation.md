---
entry: true
output: reviewable-change
forward: qa
returns: refinement
---

# Implementation

Draft: detailed gate questions and procedure are not specified.

## Entry prerequisites

A specification meeting the [refinement][phase:refinement] [exit
requirements](refinement.md#exit-validation): clear goal/scope, testable acceptance
criteria and verification scenarios.

## Required outputs

A reviewable change within the specified scope and retained self-check evidence
identifying the change and specification versions.

## Exit validation

[Implementation][phase:implementation] complete means ready for [QA][phase:qa]. An
existing change alone does not establish readiness. Detailed required self-checks and
gate questions remain unspecified.

## Valid destinations

[QA][phase:qa] when ready for judgment. [Refinement][phase:refinement] for specification
holes. Otherwise continue [implementation][phase:implementation] or hold it
[blocked][status:blocked] under [lifecycle](../lifecycle.md).
