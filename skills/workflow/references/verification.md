# Verification

Draft: detailed record format and validation questions are not specified.

## Required traceability

Connect each acceptance criterion to its verification scenarios. A scenario
identifies prerequisites, inputs/actions and expected observable results.
An execution record adds observed results, retained evidence and the tested
change/specification versions. Identify the tested build or instance when it
affects the result. Retain evidence after cleanup.

## Judgment

Evidence must demonstrate the required behavior. A successful command or a completion
field alone is insufficient. Do not substitute an easier check for an unreachable
required scenario. Missing required evidence blocks a pass; established code defects
return to [implementation][phase:implementation] and specification holes return to
[refinement][phase:refinement]. Apply the same standards regardless of actor or harness.

A verdict applies to the identified versions. Changed versions require
revalidation; old or replayed evidence is not automatically current proof.
