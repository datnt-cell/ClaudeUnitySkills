---
name: unity-physics-navigation
description: Automate Unity physics (raycasts, overlaps, gravity), NavMesh baking/queries, Terrain creation/painting, and ProBuilder mesh editing from chat. Use when the user wants physical simulation setup, pathfinding/navigation, terrain sculpting, or in-editor mesh authoring done through Editor automation. Read unity-core first for the REST protocol before calling any skill named here.
---

# Unity Physics & Navigation

Part of the Unity Skills plugin. **Read the [unity-core](../unity-core/SKILL.md) skill first** for the First Contact Checklist (health check, operating mode, dryRun gate).

## Modules in this skill

| Module | Mode | Description | Doc |
|---|:---:|---|---|
| physics | Mixed | Raycast/overlap/gravity | [modules/physics/SKILL.md](modules/physics/SKILL.md) |
| navmesh | Mixed* | NavMesh bake/query | [modules/navmesh/SKILL.md](modules/navmesh/SKILL.md) |
| terrain | FA | Terrain create/paint | [modules/terrain/SKILL.md](modules/terrain/SKILL.md) |
| probuilder | FA* | ProBuilder mesh edits | [modules/probuilder/SKILL.md](modules/probuilder/SKILL.md) (+ [MODELING_REFERENCE.md](modules/probuilder/MODELING_REFERENCE.md)) |

Mode legend defined in unity-core's [MODULE_INDEX.md](../unity-core/MODULE_INDEX.md).

## Routing

Skills are named `<module>_<verb>` (e.g. `physics_raycast`, `navmesh_bake`, `terrain_paint_texture`, `probuilder_create_cube`). NavMesh baking can be slow on large terrains — check for a long-running flag in the schema before calling.

Open the matching module doc before calling any skill whose exact parameters you don't already hold — dryRun first per the unity-core protocol.
