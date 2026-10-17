---
name: orchestrator
description: Coordinate software work execution, including autonomous runs, dispatch, runtime limits and permissions. Use when asked to orchestrate work through workflow phases.
---

# Orchestrator

Draft: execution procedure and runtime contracts are not specified.

Follow workflow phase contracts for readiness, validation and transitions.
Own selection, admission, dispatch, limits, permissions, waiting, cancellation,
reconciliation and reporting. Execution mode does not weaken workflow gates.
Receive recommendations for decisions within the authority granted by project
policy and the current request.

Use tracker for work-record operations. Select an available execution integration
when the requested mode needs one. Having integration instructions does not
establish that its tools are installed or that an operation is authorized.

For Herdr execution, consult [Herdr](references/herdr.md). Its concrete procedure,
capability checks and missing-integration handling are not yet specified.
