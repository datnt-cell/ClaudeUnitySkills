---
name: unity-rendering
description: Configure Unity rendering from chat — materials, lights, shaders, Shader Graph, URP/HDRP settings, post-processing volumes, and Decal Projectors. Use whenever the user wants to change how the scene looks (colors, lighting, shaders, render pipeline assets, post-fx) through Editor automation rather than by hand. Not for deciding a lighting setup, diagnosing washed-out, pink, blurry or leaking results, or writing HLSL and renderer features — that is scenario-unity-rendering-lighting or scenario-unity-shaders (plugin 02). Read unity-core first for the REST protocol before calling any skill named here.
---

# Unity Rendering

## Protocol

Read [`unity-core`](../unity-core/SKILL.md) before the first REST call of the session (health check + `projectName` match, operating mode, schema layers, error codes). Rules that hold for every module below:

- **Never guess parameters.** `POST /skill/<name>?mode=dryRun` until `valid:true`, then run the same call for real. Module docs hold guardrails and traps; exact signatures come from `GET /skills/recommend?includeSchema=true` or `/skills/schema`.
- **2+ objects → `POST /skills/batch`** (≤50 steps, also takes `?mode=dryRun`).
- `MODE_RESTRICTED` → ask the user for a grant. `SURFACE_EXCLUDED` is a configuration boundary: don't retry or route around it — switch to guide mode ([`unity-core/SKILL_GUIDE.md`](../unity-core/SKILL_GUIDE.md)).

## Modules in this skill

| Module | Mode | Description | Doc |
|---|:---:|---|---|
| material | FA | Material property edits | [modules/material/SKILL.md](modules/material/SKILL.md) |
| light | FA | Light create/configure | [modules/light/SKILL.md](modules/light/SKILL.md) |
| shader | Mixed* | Shader create/list | [modules/shader/SKILL.md](modules/shader/SKILL.md) |
| shadergraph | Mixed* | Shader Graph create/inspect/blackboard edit/node editing | [modules/shadergraph/SKILL.md](modules/shadergraph/SKILL.md) |
| shadergraph-design | — | ShaderGraph dual-version source-anchored rules (advisory, no REST skills) | [modules/shadergraph-design/SKILL.md](modules/shadergraph-design/SKILL.md) |
| graphics | Mixed | GraphicsSettings / QualitySettings / SRP assets | [modules/graphics/SKILL.md](modules/graphics/SKILL.md) |
| volume | Mixed* | Volume / VolumeProfile / VolumeComponent | [modules/volume/SKILL.md](modules/volume/SKILL.md) |
| postprocess | FA* | Modern URP/HDRP post-processing | [modules/postprocess/SKILL.md](modules/postprocess/SKILL.md) |
| urp | Mixed* | URP asset / renderer / renderer features | [modules/urp/SKILL.md](modules/urp/SKILL.md) |
| decal | Mixed* | URP Decal Projector workflow | [modules/decal/SKILL.md](modules/decal/SKILL.md) |

Mode legend: SA = runs without grant in all modes, FA = needs grant under Approval, Mixed = check per-skill, `*` = contains auto-forbidden skills.

## Routing

Skills are named `<module>_<verb>` (e.g. `material_set_property`, `shadergraph_create`, `urp_get_renderer`). For Shader Graph work, read `shadergraph-design` for source-anchored node/version rules before authoring graphs — schema alone won't tell you which nodes exist in the user's SRP version.

Open the matching module doc before calling any skill whose exact parameters you don't already hold — dryRun first.
