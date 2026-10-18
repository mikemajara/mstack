---
name: tracker
description: Read and record work specifications, workflow facts, evidence and relationships through a selected work-record integration.
---

# Tracker

Draft: installed cross-skill reference resolution is not specified.

Record specifications, validated workflow facts, evidence and relationships.
Follow workflow contracts for required content and readiness. A field or label
records a fact; changing it does not prove the fact. Preserve the identities of
the specification, change and evidence used for a validated transition.

A story may live in any selected work-record system. Maintain links between
the story and its PR, including a link from the story to the QA verdict on the
PR. Record story closure only after workflow's
[story completion](../workflow/references/lifecycle.md#story-completion)
requirements are satisfied.

## Integration contract

Select integrations from project configuration or the current request. Story
storage and PR hosting may use different systems. Each integration describes
the capabilities it supplies, its prerequisites, concrete record mappings and
operations. Load only the references needed for the selected systems.

For the operations it supplies, an integration must support these outcomes:

| Operation | Required result |
| --- | --- |
| Find and read | Identify records by system, project and stable ID or URL. Return the requested content, attributes and relationships; disclose filters, limits and missing data. Search matches are candidates, not established duplicates or links. |
| Create and update | Preserve the specification and unrelated content. Map workflow attributes without inventing missing values. Return the record identity and confirm the stored result. |
| Link records and evidence | Maintain story/PR links and a story link to the QA verdict on the PR, even across systems. Preserve the identities of the specification, tested change and retained evidence. |
| Record a transition or closure | Record an already validated workflow fact. Confirm the resulting state under the workflow contract, including merge before story closure. |

Follow workflow for the content required in each phase and the meaning of its
[attributes](../workflow/references/attributes.json). Selection, dispatch and
execution authority belong to orchestrator and project policy. An integration
does not establish readiness from labels, body headings or tool success.

## Availability and failures

Check access to the actual project and the capability needed for the operation.
Read access does not establish write access. Report an unsupported capability
or missing access and the input needed to resume; stop the affected operation.
Continue independent work where possible. Apply workflow's
[blocked-work rules](../workflow/references/lifecycle.md#missing-inputs-blocked-work-and-resume)
when required input is unavailable. If the tracker itself cannot be updated,
report the hold to the caller rather than claiming it was recorded.

After a write, read the affected record to confirm the result. If a write fails
or its outcome is uncertain, report confirmed changes and remaining work, then
read current state before retrying to avoid duplicate records or evidence.
Do not report a failed or partial read as an empty result. Mark cached or
exported records with their source, time and coverage; they do not establish
current remote state. Do not silently switch the story's system of record.

## Integrations

| System | Capabilities | Reference |
| --- | --- | --- |
| GitHub | Stories in Issues; PRs, QA records and merge confirmation, independently of story storage | [GitHub](references/github.md) |

Integration helpers ship under `scripts/<integration>/` within this skill.
The integration reference explains how to locate and invoke them against the
target project. Use direct system operations for ordinary actions; add helpers
for useful aggregation or reliable repeated updates.
