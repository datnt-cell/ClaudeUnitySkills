---
name: unity-scene-authoring
description: Create and edit GameObjects, Components, Prefabs, and Scenes in the Unity Editor, plus Editor-level control (play/select/undo/redo). Use for hierarchy authoring, prefab variants, scene load/save/query, and any scene-authoring automation from chat. Read unity-core first for the REST protocol (health check, operating mode, dryRun gate) before calling any skill named here.
---

# Unity Scene Authoring

Part of the Unity Skills plugin. **Read the [unity-core](../unity-core/SKILL.md) skill first** — it has the First Contact Checklist (`GET /health`, operating mode, schema layers) that every REST call in this plugin depends on. This skill only covers module-specific guardrails and routing.

## Modules in this skill

| Module | Mode | Description | Doc |
|---|:---:|---|---|
| gameobject | FA* | Object create/move/parent | [modules/gameobject/SKILL.md](modules/gameobject/SKILL.md) |
| component | Mixed* | Component add/remove/configure | [modules/component/SKILL.md](modules/component/SKILL.md) |
| prefab | FA | Prefab create/apply/spawn | [modules/prefab/SKILL.md](modules/prefab/SKILL.md) |
| scene | SA* | Scene load/save/query | [modules/scene/SKILL.md](modules/scene/SKILL.md) |
| editor | SA* | Play/select/undo/redo/change journal | [modules/editor/SKILL.md](modules/editor/SKILL.md) |

Mode legend (SA = runs without grant in all modes, FA = needs grant under Approval, Mixed = check per-skill, `*` = contains auto-forbidden skills) is defined in unity-core's [MODULE_INDEX.md](../unity-core/MODULE_INDEX.md).

## Routing

Skills are named `<module>_<verb>` (e.g. `gameobject_create`, `component_add`, `prefab_apply`, `scene_load`). Open the matching module doc above before calling any skill whose exact parameters you don't already hold — dryRun first per the unity-core protocol.

## Guidance mode

If `surfaceProfile` is `guide` or `noSceneAuthoring` (see unity-core's [SKILL_GUIDE.md](../unity-core/SKILL_GUIDE.md)), GameObject/Component/Scene writes may be hidden — fall back to the manual advisories in unity-core: [manual-gameobject](../unity-core/manual/manual-gameobject/SKILL.md), [manual-component](../unity-core/manual/manual-component/SKILL.md), [manual-scene](../unity-core/manual/manual-scene/SKILL.md).

## Batch-first rule

When touching 2+ objects, prefer `*_batch` skills over repeated single-item calls (see unity-core's batch protocol, owned by the unity-dev-workflow skill's `batch` module).
