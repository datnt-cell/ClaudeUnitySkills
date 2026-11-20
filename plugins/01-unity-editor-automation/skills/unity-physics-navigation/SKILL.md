---
name: unity-physics-navigation
description: Automate Unity physics (raycasts, overlaps, gravity), NavMesh baking/queries, Terrain creation/painting, and ProBuilder mesh editing from chat. Use when the user wants physical simulation setup, pathfinding/navigation, terrain sculpting, or in-editor mesh authoring done through Editor automation. Not for designing AI, character controllers or projectile physics (scenario-unity-gameplay) or procedural terrain and level design (scenario-unity-world-building), both plugin 02. Read unity-core first for the REST protocol before calling any skill named here.
---

# Unity Physics & Navigation

## Protocol

Read [`unity-core`](../unity-core/SKILL.md) before the first REST call of the session (health check + `projectName` match, operating mode, schema layers, error codes). Rules that hold for every module below:

- **Never guess parameters.** `POST /skill/<name>?mode=dryRun` until `valid:true`, then run the same call for real. Module docs hold guardrails and traps; exact signatures come from `GET /skills/recommend?includeSchema=true` or `/skills/schema`.
- **2+ objects → `POST /skills/batch`** (≤50 steps, also takes `?mode=dryRun`).
- `MODE_RESTRICTED` → ask the user for a grant. `SURFACE_EXCLUDED` is a configuration boundary: don't retry or route around it — switch to guide mode ([`unity-core/SKILL_GUIDE.md`](../unity-core/SKILL_GUIDE.md)).

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
