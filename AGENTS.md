# Agent Instructions

These instructions apply repository-wide.

## Repository purpose

This repository is an educational/research AirSim project for simulated vehicle control,
path planning, distance-sensor integration, and YOLO-based image detection.

It is **not** a production autonomous-driving stack, a safety-certified controller, or
evidence of real-world vehicle readiness. Simulator/demo success proves only the exact
simulated scenario that was exercised.

A nested `AGENTS.md` hierarchy is not needed at the current repository size. Add one only
if a subtree later develops materially different authority, release, or runtime-safety
rules.

## Read before changing behavior

Inspect the affected implementation and configuration together with:

- `README.md`
- `configs/README.md`
- `core/`
- `detection/`
- `utils/`
- focused tests or smoke paths when present.

Do not infer current AirSim, Ultralytics, OpenCV, Python, or model APIs from memory when an
exact external contract matters; verify the supported version/documentation first.

## Simulation and safety truth

- Do not describe this project as safe for physical vehicles or public roads.
- Do not convert simulated obstacle avoidance, path completion, object detection, or
  connection success into a real-world safety/performance claim.
- Missing camera frames, unavailable sensors, stale telemetry, failed model inference, or
  lost AirSim connectivity are unknown/error states, not evidence that a path is safe.
- Motion limits, steering bounds, sensor thresholds, retry counts, sleeps, and recovery
  loops should remain explicit and bounded.
- A new movement path must not silently bypass the existing control/sensor decision path.
- Preserve a deterministic way to stop/brake and release AirSim API control during normal
  completion, interruption, and error cleanup.

## AirSim boundary

AirSim is an external simulator.

- Keep connection/retry behavior bounded.
- Treat simulator RPC failures and malformed/missing responses as explicit failures.
- Preserve vehicle names, sensor names, coordinate conventions, units, and camera IDs when
  changing configuration or code.
- Configuration examples must not be presented as universal AirSim settings.
- Tests that do not launch AirSim are offline/unit evidence only.

## Vision boundary

YOLO/OpenCV output is perception evidence, not a safety authority.

- Treat image retrieval and model output as fallible.
- Keep image/channel/shape assumptions explicit and validated before indexing.
- Do not fabricate detection confidence, model accuracy, FPS, or robustness claims.
- Model weights and external assets should be versioned/documented when reproducibility
  depends on them.
- Avoid unbounded frame buffers or background inference loops.

## Path planning and control

- Keep A* graph/coordinate semantics deterministic for equivalent inputs.
- Preserve steering saturation and document units when changing pure-pursuit behavior.
- Recovery/reverse behavior must remain bounded and observable.
- Avoid adding hidden global state that changes mission behavior across runs.
- Behavioral fixes to navigation, obstacle handling, or control loops should include
  focused regression coverage when practical.

## Dependencies and secrets

- Do not commit API keys, credentials, private endpoints, local simulator secrets, large
  generated datasets, or machine-specific paths.
- Keep dependency changes narrow and review upstream compatibility before major upgrades.
- Do not weaken dependency-review or workflow protections to land unrelated work.
- Third-party GitHub Actions should remain pinned to immutable reviewed commit SHAs.

## Validation

Use the narrowest executable checks available for the changed surface. At minimum:

- import/compile the touched Python modules;
- run any focused tests or smoke scripts that exist;
- for live control/sensor changes, exercise AirSim when the environment is actually
  available and report clearly when it was not.

The GitHub dependency-review workflow is authoritative for dependency changes. A green
dependency review does not prove simulator correctness.

## Change discipline

- Keep diffs focused.
- Do not combine unrelated refactors with navigation/sensor/control changes.
- Preserve platform-independent core logic where possible.
- Update README/config docs when user-visible setup or behavior changes.
- Do not weaken validation or safety-related bounds merely to make an experiment run.

## Definition of done

A change is ready when the implementation, affected configuration/docs, focused regression
evidence, bounded failure behavior, and exact-head GitHub checks agree. State explicitly
whether live AirSim, GPU/model inference, and physical hardware were not exercised.
