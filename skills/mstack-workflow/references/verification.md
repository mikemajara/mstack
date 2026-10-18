# Verification

## Required traceability

Connect every acceptance criterion to one or more verification scenarios. Each
scenario identifies its prerequisites, inputs or actions, and expected
observable result. For each execution, record the observed result, retained
evidence and tested commit; identify the tested build or instance when it
affects the result. Retain evidence after cleanup. Projects may choose their
own record layout; the required facts must remain clear and traceable per
scenario.

Before accepting an execution record, check that every acceptance criterion
has scenario coverage, each scenario has enough detail to execute and judge,
and each execution identifies its result, evidence and tested commit. Evidence
must show the expected behavior, not only that a command completed.

Record QA execution results, evidence links and the verdict on the PR as defined
by [QA](phases/qa.md). Link them to the story's acceptance criteria and
verification scenarios, and identify the tested commit. The story links to the
verdict without duplicating the report.

## Judgment

Evidence must demonstrate the required behavior. A successful command or a completion
field alone is insufficient. Do not substitute an easier check for an unreachable
required scenario. Missing required evidence blocks a pass; established code defects
return to [implementation][phase:implementation] and specification holes return to
[refinement][phase:refinement]. Apply the same standards regardless of actor or harness.

A verdict applies to the tested commit. If that commit changes before merge,
rerun every required acceptance scenario on the new commit and record the new
results and evidence. Old or replayed evidence is not automatically current
proof. A return to refinement invalidates earlier QA verdicts under
[specification changes](lifecycle.md#specification-changes). Only refinement
changes the specification; a separate specification version is not required.
