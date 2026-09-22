---
name: unity-dev-workflow
description: Cross-cutting Unity Editor developer workflow tools from chat — batch/async job execution, task snapshots and undo orchestration, compile/system diagnostics, console log capture, profiler stats, undo/redo history, Scene View bookmarks, the Unity Test Runner, demo/test scaffolding, query/layout/auto-bind smart tools, scene/project analysis, project info/settings, and UnityEvent wiring. Use for anything spanning multiple modules, or for the batch/dryRun execution machinery itself. Read unity-core first for the REST protocol before calling any skill named here.
---

# Unity Dev Workflow

Part of the Unity Skills plugin. This skill's `batch` module is the mechanism the protocol's batch execution step below uses.

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

| Module | Mode | Description | Doc |
|---|:---:|---|---|
| batch | SA | Batch and async jobs | [modules/batch/SKILL.md](modules/batch/SKILL.md) |
| workflow | SA* | Task snapshots/undo, batch orchestration | [modules/workflow/SKILL.md](modules/workflow/SKILL.md) |
| debug | SA* | Compile/system diagnostics | [modules/debug/SKILL.md](modules/debug/SKILL.md) |
| console | SA | Log capture/filter | [modules/console/SKILL.md](modules/console/SKILL.md) |
| profiler | SA | Perf statistics | [modules/profiler/SKILL.md](modules/profiler/SKILL.md) |
| history | SA | Undo/redo history | [modules/history/SKILL.md](modules/history/SKILL.md) |
| bookmark | SA | Scene View bookmarks | [modules/bookmark/SKILL.md](modules/bookmark/SKILL.md) |
| test | Mixed* | Unity Test Runner | [modules/test/SKILL.md](modules/test/SKILL.md) |
| sample | Mixed* | Demo/test skills | [modules/sample/SKILL.md](modules/sample/SKILL.md) |
| smart | FA* | Query/layout/auto-bind | [modules/smart/SKILL.md](modules/smart/SKILL.md) |
| perception | SA | Scene/project analysis | [modules/perception/SKILL.md](modules/perception/SKILL.md) |
| project | SA* | Project info/settings | [modules/project/SKILL.md](modules/project/SKILL.md) |
| event | Mixed* | UnityEvent wiring | [modules/event/SKILL.md](modules/event/SKILL.md) |

Mode legend: SA = runs without grant in all modes, FA = needs grant under Approval, Mixed = check per-skill, `*` = contains auto-forbidden skills.

## Routing

Skills are named `<module>_<verb>`, except `scene_analyze`, `hierarchy_describe`, `project_stack_detect` → `perception`, and `job_*` → `batch`. Use `debug`/`console` to close the loop after any write from another unity-* skill — verify compilation and check for new errors before reporting success. Use `workflow` for multi-step task snapshots you may need to roll back as a unit, and `batch` (`POST /skills/batch`) for up to 50 steps per call when touching 2+ objects across any module in this plugin.

Open the matching module doc before calling any skill whose exact parameters you don't already hold — dryRun first.
