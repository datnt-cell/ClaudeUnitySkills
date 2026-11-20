---
name: unity-scene-authoring
description: Create and edit GameObjects, Components, Prefabs, and Scenes in the Unity Editor, plus Editor-level control (play/select/undo/redo). Use for hierarchy authoring, prefab variants, scene load/save/query, and any scene-authoring automation from chat. Read unity-core first for the REST protocol (health check, operating mode, dryRun gate) before calling any skill named here.
---

# Unity Scene Authoring

Part of the Unity Skills plugin. This skill covers module-specific guardrails and routing; the shared REST protocol every call depends on is summarized below.

## Protocol

Read [`unity-core`](../unity-core/SKILL.md) before the first REST call of the session (health check + `projectName` match, operating mode, schema layers, error codes). Rules that hold for every module below:

- **Never guess parameters.** `POST /skill/<name>?mode=dryRun` until `valid:true`, then run the same call for real. Module docs hold guardrails and traps; exact signatures come from `GET /skills/recommend?includeSchema=true` or `/skills/schema`.
- **2+ objects → `POST /skills/batch`** (≤50 steps, also takes `?mode=dryRun`).
- `MODE_RESTRICTED` → ask the user for a grant. `SURFACE_EXCLUDED` is a configuration boundary: don't retry or route around it — switch to guide mode ([`unity-core/SKILL_GUIDE.md`](../unity-core/SKILL_GUIDE.md)).

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

If `surfaceProfile` is `guide` or `noSceneAuthoring`, GameObject/Component/Scene writes may be hidden. In that case, give the user manual step-by-step guidance (menu paths, Inspector fields) instead of calling REST skills; the guidance-boundary table and `manual-*` step-by-step docs are in [`unity-core/SKILL_GUIDE.md`](../unity-core/SKILL_GUIDE.md).

## Batch-first rule

When touching 2+ objects, prefer `*_batch` skills over repeated single-item calls (`POST /skills/batch`, owned by the unity-dev-workflow skill's `batch` module).
