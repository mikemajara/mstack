# Project memory

Continuity log. Lasting repository guidance is in `AGENTS.md`; the maintained
package specification is `docs/STRUCTURE.md`. This log does not replace current
tracker state or verification evidence.

## 2026-10-17 — Architecture alignment and handoff

- **Agreed:** exactly three skill roots: `workflow/`, `orchestrator/`,
  `tracker/`. The earlier four-skill proposal is superseded. Responsibilities
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
