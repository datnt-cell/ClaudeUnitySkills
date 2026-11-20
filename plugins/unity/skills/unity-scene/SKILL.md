---
name: unity-scene
description: "Create and edit GameObjects, Components, Prefabs and Scenes in an open Unity Editor through the UnitySkills REST server, plus Editor control (play, select, undo/redo). Use for hierarchy authoring, adding or configuring components, prefab variants and overrides, scene load/save/query from chat. Works on Unity 2022.3+ and 6000.x."
license: MIT
---
# Unity scene authoring

> **Layout:** do it in an open Editor → [`actions/`](actions/) (REST, section below).

Hierarchy, components, prefabs and scenes in the open Editor. Domain knowledge for a specific system (UI, lighting, animation, ...) lives in that domain skill; this one is the generic object/scene layer every domain uses.

## Doing it in the open Editor (REST)

Needs the UnitySkills package in the project (REST server on `localhost:8090`-`8100`). Read [`../unity-core/rest/PROTOCOL.md`](../unity-core/rest/PROTOCOL.md) before the first call of the session. Hard rules for every module below:

- **Never guess parameters.** `POST /skill/<name>?mode=dryRun` until `valid:true`, then run the same call for real. Module docs hold guardrails and traps; exact signatures come from `GET /skills/recommend?includeSchema=true` or `/skills/schema`.
- **2+ objects → `POST /skills/batch`** (≤50 steps, also takes `?mode=dryRun`).
- `MODE_RESTRICTED` → ask the user for a grant. `SURFACE_EXCLUDED` is a configuration boundary: don't retry or route around it — switch to guide mode ([`../unity-core/rest/GUIDE.md`](../unity-core/rest/GUIDE.md)).
- After any write, check compilation and new errors with `unity-core` → `actions/console`, `actions/debug` before reporting success.

| Module | Mode | What it does | Doc |
|---|:---:|---|---|
| gameobject | FA* | Object create/move/parent | [actions/gameobject/SKILL.md](actions/gameobject/SKILL.md) |
| component | Mixed* | Component add/remove/configure | [actions/component/SKILL.md](actions/component/SKILL.md) |
| prefab | FA | Prefab create/apply/spawn | [actions/prefab/SKILL.md](actions/prefab/SKILL.md) |
| scene | SA* | Scene load/save/query | [actions/scene/SKILL.md](actions/scene/SKILL.md) |
| editor | SA* | Play/select/undo/redo/change journal | [actions/editor/SKILL.md](actions/editor/SKILL.md) |

Mode: SA = runs without grant in every mode, FA = needs a grant under Approval, Mixed = per skill, `*` = has auto-forbidden skills, — = documentation only.

Skills are named `<module>_<verb>` (e.g. `gameobject_create`, `component_add`, `prefab_apply`, `scene_load`).

### Guidance mode

If `surfaceProfile` is `guide` or `noSceneAuthoring`, GameObject/Component/Scene writes may be hidden. In that case, give the user manual step-by-step guidance (menu paths, Inspector fields) instead of calling REST skills; the guidance-boundary table and `manual-*` step-by-step docs are in [`unity-core/rest/GUIDE.md`](../unity-core/rest/GUIDE.md).

### Batch-first rule

When touching 2+ objects, prefer `*_batch` skills over repeated single-item calls (`POST /skills/batch`, owned by the unity-core skill's `batch` module).
