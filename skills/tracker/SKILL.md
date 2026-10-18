---
name: tracker
description: Read and record work specifications, workflow facts, evidence and relationships through a selected work-record integration.
---

# Tracker

Draft: general operation contracts and concrete adapters are not specified.

Record specifications, validated workflow facts, evidence and relationships.
Follow workflow contracts for required content and readiness. A field or label
records a fact; changing it does not prove the fact. Preserve the identities of
the specification, change and evidence used for a validated transition.

A story may live in any selected work-record system. Maintain links between
the story and its PR, including a link from the story to the QA verdict on the
PR. Record story closure only after workflow's
[story completion](../workflow/references/lifecycle.md#story-completion)
requirements are satisfied.

Use the selected work-record integration's operations and constraints. For GitHub,
consult [GitHub](references/github.md). Integration availability, failure handling
and operation contracts are not yet specified.
