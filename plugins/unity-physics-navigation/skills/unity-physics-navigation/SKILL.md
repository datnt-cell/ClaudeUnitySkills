---
name: unity-physics-navigation
description: Automate Unity physics (raycasts, overlaps, gravity), NavMesh baking/queries, Terrain creation/painting, and ProBuilder mesh editing from chat. Use when the user wants physical simulation setup, pathfinding/navigation, terrain sculpting, or in-editor mesh authoring done through Editor automation. Read unity-core first for the REST protocol before calling any skill named here.
---

# Unity Physics & Navigation

Part of the Unity Skills plugin.

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
| physics | Mixed | Raycast/overlap/gravity | [modules/physics/SKILL.md](modules/physics/SKILL.md) |
| navmesh | Mixed* | NavMesh bake/query | [modules/navmesh/SKILL.md](modules/navmesh/SKILL.md) |
| terrain | FA | Terrain create/paint | [modules/terrain/SKILL.md](modules/terrain/SKILL.md) |
| probuilder | FA* | ProBuilder mesh edits | [modules/probuilder/SKILL.md](modules/probuilder/SKILL.md) (+ [MODELING_REFERENCE.md](modules/probuilder/MODELING_REFERENCE.md)) |

Mode legend: SA = runs without grant in all modes, FA = needs grant under Approval, Mixed = check per-skill, `*` = contains auto-forbidden skills.

## Routing

Skills are named `<module>_<verb>` (e.g. `physics_raycast`, `navmesh_bake`, `terrain_paint_texture`, `probuilder_create_cube`). NavMesh baking can be slow on large terrains — check for a long-running flag in the schema before calling.

Open the matching module doc before calling any skill whose exact parameters you don't already hold — dryRun first.
