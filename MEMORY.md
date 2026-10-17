# Project memory

Continuity log. Lasting repository guidance is in `AGENTS.md`; the maintained
package specification is `docs/STRUCTURE.md`. This log does not replace current
tracker state or verification evidence.

## 2026-10-17 — Architecture alignment and handoff

- **Agreed:** exactly three skill roots: `skills/workflow/`, `skills/orchestrator/`,
  `skills/tracker/`. The earlier four-skill proposal is superseded. Responsibilities
  are maintained in [STRUCTURE.md](docs/STRUCTURE.md), not restated here.
- **Agreed:** memory belongs to workflow by default, not a fourth skill or an
  optional end-of-session task. `MEMORY.md` is the working project convention;
  retain relevant information not already in lasting project guidance.
- **Clarified:** implementation complete, QA complete, integrated and deployed
  are distinct facts. Tracker status represents those facts, not proof of them.
  Do not reopen the earlier done-versus-closed-issue ambiguity.
- **Authoring gotcha:** design discussion must not leak into distributed
  instructions. Concrete adapter references describe their operations, not
  speculative replacements or personal setup history.
- **Current state:** no new operational skill or phase reference exists. This
  session saves structure, memory and handoff documents only.
- **Preserved:** the unchanged backlog archive and all three import tags;
  see [PROVENANCE.md](docs/PROVENANCE.md). Do not install the archive as mstack.
- **Continue:** [HANDOFF.md](docs/HANDOFF.md) contains the session context,
  prior work pointers, limits and next review unit. Detailed phase contracts,
  adapter implementation, installed-reference resolution and runtime cutover
  remain unfinished, not implicitly approved or deployed.

## 2026-10-17 — Phase entry and optional integrations

- **Agreed:** no intake phase. Research is the earliest phase; work can enter
  research, refinement or implementation when the destination's prerequisites
  are already satisfied. Entering later does not weaken readiness criteria.
- **Clarified:** use “optional integration” for areas such as Herdr under
  orchestrator and GitHub under tracker. A skill can have or use an integration,
  or operate without one where its capabilities allow. The integration is an
  instructional boundary; the term does not imply a deterministic program.
- **Clarified:** integration instructions supply concrete tool operations and
  constraints while workflow supplies the phase requirements. Autonomous
  execution through Herdr depends on its availability and the user's selected
  execution mode. Missing-integration handling is still being discussed.
- **Discussion mode:** align one area at a time through conversation before
  authoring full skill instructions; the earlier five-file proposal is pending.
- **Agreed direction:** use question-driven phase validation with explicit
  distinction between mandatory gates and investigation guidance. Avoid vague
  readiness claims and invented answers used to obtain a transition.
- **Superseded review surface:** the temporary `docs/WORKFLOW.md` draft moved
  into workflow references when the user requested actual skill scaffolds.

## 2026-10-17 — Skill scaffolding

- **Authorized:** create the three skill roots and fill their files incrementally.
  Maintain behavior in skill files and keep planning docs short to reduce
  duplicated context. This does not authorize installation or automation.
- **Current review:** [research.md](skills/workflow/references/phase/research.md) contains two
  draft required gate questions; exact gate wording is not agreed. Continue the
  feature example one phase at a time. The question-driven gate/fallback model
  is in [lifecycle.md](skills/workflow/references/lifecycle.md).
- **Incomplete:** later phase contracts and integration procedures are scaffolds.
  Deployment's phase applicability and missing-integration handling remain open.
  Cross-skill resolution in an installed package still needs defining.

- **Layout:** moved all three skill roots beneath `skills/` at the user's
  request for skills.sh discovery. Discovery and installation were not exercised.
