---
entry: true
output: research-brief
forward: refinement
---

# Research

Draft contract; exit gate wording is under review.

## Entry prerequisites

A work request identifies a problem, a requested capability, or a possible change
whose relevant facts need investigation. A feasibility question is in scope,
including whether a capability can be added and what effect it would have.

## Required outputs

A [research][phase:research] brief with the request, relevant current-state evidence,
remaining factual unknowns and decisions needed to shape a specification. Distinguish
observed facts from recommendations.

## Exit validation

| Required question | Draft pass condition |
| --- | --- |
| What change or problem is being considered, and what is true today? | State the request and affected behavior/system. Cite inspected sources or observations establishing the relevant current behavior, including absence of the requested capability where applicable. |
| Can a testable specification be shaped from these facts? | List remaining choices and factual unknowns, or explicitly state none remain. No unresolved factual unknown prevents defining feasible scope, acceptance criteria and verification scenarios. Product choices may remain for [refinement][phase:refinement]. |

Investigation prompts: affected users, dependencies, compatibility, project
conventions and behavior to preserve. Use the prompts relevant to the change;
retain material findings and uncertainty. These are guidance, not extra gates.

## Valid destinations

Research is done when the exit gate passes. The next phase is
[refinement][phase:refinement]. Otherwise continue [research][phase:research] or hold it
[blocked][status:blocked] under [lifecycle](../lifecycle.md).
