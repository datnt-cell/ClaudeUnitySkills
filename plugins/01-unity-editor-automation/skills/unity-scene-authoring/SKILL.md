---
name: unity-scene-authoring
description: Create and edit GameObjects, Components, Prefabs, and Scenes in the Unity Editor, plus Editor-level control (play/select/undo/redo). Use for hierarchy authoring, prefab variants, scene load/save/query, and any scene-authoring automation from chat. Read unity-core first for the REST protocol (health check, operating mode, dryRun gate) before calling any skill named here.
---

# Unity Scene Authoring

Part of the Unity Skills plugin. This skill covers module-specific guardrails and routing; the shared REST protocol every call depends on is summarized below.

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
| gameobject | FA* | Object create/move/parent | [modules/gameobject/SKILL.md](modules/gameobject/SKILL.md) |
| component | Mixed* | Component add/remove/configure | [modules/component/SKILL.md](modules/component/SKILL.md) |
| prefab | FA | Prefab create/apply/spawn | [modules/prefab/SKILL.md](modules/prefab/SKILL.md) |
| scene | SA* | Scene load/save/query | [modules/scene/SKILL.md](modules/scene/SKILL.md) |
| editor | SA* | Play/select/undo/redo/change journal | [modules/editor/SKILL.md](modules/editor/SKILL.md) |

Mode legend: SA = runs without grant in all modes, FA = needs grant under Approval, Mixed = check per-skill, `*` = contains auto-forbidden skills.

## Routing

Skills are named `<module>_<verb>` (e.g. `gameobject_create`, `component_add`, `prefab_apply`, `scene_load`). Open the matching module doc above before calling any skill whose exact parameters you don't already hold — dryRun first.

## Guidance mode

If `surfaceProfile` is `guide` or `noSceneAuthoring`, GameObject/Component/Scene writes may be hidden. In that case, give the user manual step-by-step guidance (menu paths, Inspector fields) instead of calling REST skills; for the full guidance-boundary table and manual-* step-by-step docs, the optional unity-core plugin covers this in more depth.

## Batch-first rule

When touching 2+ objects, prefer `*_batch` skills over repeated single-item calls (`POST /skills/batch`, owned by the unity-dev-workflow skill's `batch` module).
