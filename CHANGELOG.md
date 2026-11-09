# Changelog

## Unreleased

### Added

- Optional, gitignored `.mstack/config.local.json` for personal orchestration
  settings. Explicit run instructions take precedence over local overrides,
  shared configuration and skill defaults, in that order.
- Execution-only local overrides for runtime, harness, role execution choices,
  models and run controls. Workflow requirements and tracker selection remain
  shared and cannot be overridden locally.
- Configuration guidance for object merging, array replacement, runtime-option
  resets, invalid local files, worker inheritance and resumed runs.
- An exact `/.mstack/config.local.json` ignore rule, leaving shared configuration,
  project memory and filesystem issues available to version control.

These changes define draft skill behavior, not an executable configuration loader.
Existing single-controller and filesystem single-writer limits remain unchanged.
