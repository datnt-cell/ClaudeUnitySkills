---
name: unity-dev-workflow
description: Cross-cutting Unity Editor developer workflow tools from chat — batch/async job execution, task snapshots and undo orchestration, compile/system diagnostics, console log capture, profiler stats, undo/redo history, Scene View bookmarks, the Unity Test Runner, demo/test scaffolding, query/layout/auto-bind smart tools, scene/project analysis, project info/settings, and UnityEvent wiring. Use for anything spanning multiple modules, or for the batch/dryRun execution machinery itself. Not for performance investigations (CPU/GPU bound, spikes, GC, draw calls) — that is scenario-unity-performance (plugin 02); this skill only reads the open Editor's Profiler and console. Read unity-core first for the REST protocol before calling any skill named here.
---

# Unity Dev Workflow

Part of the Unity Skills plugin. This skill's `batch` module is the mechanism the protocol's batch execution step below uses.

## Protocol

Read [`unity-core`](../unity-core/SKILL.md) before the first REST call of the session (health check + `projectName` match, operating mode, schema layers, error codes). Rules that hold for every module below:

- **Never guess parameters.** `POST /skill/<name>?mode=dryRun` until `valid:true`, then run the same call for real. Module docs hold guardrails and traps; exact signatures come from `GET /skills/recommend?includeSchema=true` or `/skills/schema`.
- **2+ objects → `POST /skills/batch`** (≤50 steps, also takes `?mode=dryRun`).
- `MODE_RESTRICTED` → ask the user for a grant. `SURFACE_EXCLUDED` is a configuration boundary: don't retry or route around it — switch to guide mode ([`unity-core/SKILL_GUIDE.md`](../unity-core/SKILL_GUIDE.md)).

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
