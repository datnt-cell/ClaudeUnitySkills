---
name: unity-architecture-advisory
description: Design-time advisory guidance for Unity projects — inspecting an existing project, planning system/asmdef boundaries, recording architecture decision records (ADRs), reviewing performance hot paths, sketching small-game blueprints, defining scene wiring contracts, and designing authoring UX. Use when the user wants planning, review, or documentation of a Unity project's architecture rather than direct Editor automation — these modules define no REST skills. Read unity-core first if the task also needs Editor automation.
---

# Unity Architecture Advisory

Part of the Unity Skills plugin. These modules are documentation-only — they define no REST skills and don't require the Unity Skills protocol below.

If the task also needs Editor automation (creating files, querying the scene), use this quick reference:

## Unity Skills protocol (quick reference)

This plugin talks to the Unity Editor through a local REST server. Before the first skill call in a session:

1. **`GET /health`** — discover the server (it binds a port in `8090`–`8100`). Read `currentMode` (`"approval"` / `"auto"` / `"bypass"`) and `surfaceProfile` (`full` / `guide` / `noSceneAuthoring`) — confirm `projectName` matches the project you're editing.
2. Under `approval` mode, the first write to a `FullAuto` skill returns `MODE_RESTRICTED` and needs a user grant; under `auto`/`bypass` writes execute directly.

**dryRun gate**: before executing any skill whose exact parameters you don't already hold, call `POST /skill/<name>?mode=dryRun` and iterate until the response says `valid:true` — never guess parameters from a skill name alone. Then execute the same call without `?mode=dryRun`.

**Batch execution**: when touching 2+ objects, prefer `POST /skills/batch` (`{"steps":[{"skill","args"}],"continueOnError":false}`, up to 50 steps per call, also supports `?mode=dryRun`) over repeated single-item calls.

**Error codes**:
| Code | Meaning |
|---|---|
| `MODE_RESTRICTED` / `MODE_FORBIDDEN` | Needs a user grant, or needs Bypass/Allowlist. |
| `SURFACE_EXCLUDED` | Hidden by the current `surfaceProfile` — a configuration boundary, not a failure; don't retry or route around it. |
| `MISSING_PARAM` / `TARGET_NOT_FOUND` | Bad or unresolvable arguments — dryRun for the schema, locate the target first. |

For the full protocol detail, error code catalog, and guide-mode `manual-*` docs, optionally install the `unity-core` plugin — it's not required, this summary covers what you need for this skill.

## Modules in this skill

| Module | Description | Doc |
|---|---|---|
| project-scout | Inspect existing project | [modules/project-scout/SKILL.md](modules/project-scout/SKILL.md) |
| architecture | Plan system boundaries | [modules/architecture/SKILL.md](modules/architecture/SKILL.md) |
| adr | Record tradeoffs | [modules/adr/SKILL.md](modules/adr/SKILL.md) |
| performance | Review hot paths | [modules/performance/SKILL.md](modules/performance/SKILL.md) |
| blueprints | Small-game blueprints | [modules/blueprints/SKILL.md](modules/blueprints/SKILL.md) |
| scene-contracts | Define scene wiring | [modules/scene-contracts/SKILL.md](modules/scene-contracts/SKILL.md) |
| inspector | Design authoring UX | [modules/inspector/SKILL.md](modules/inspector/SKILL.md) |

## Routing

Start with `project-scout` when picking up an unfamiliar project — it establishes the baseline the other advisories build on. Use `architecture` and `adr` together when a design decision needs both a plan and a recorded rationale. `performance` and `scene-contracts` are useful before large automation passes from other unity-* skills, to avoid baking in a design that needs rework.

None of these modules call REST endpoints — they're read/write-to-docs guidance you produce directly, not automation you dispatch through the Unity Editor server.
