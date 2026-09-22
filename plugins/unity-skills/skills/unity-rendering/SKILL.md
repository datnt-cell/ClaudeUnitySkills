---
name: unity-rendering
description: Configure Unity rendering from chat — materials, lights, shaders, Shader Graph, URP/HDRP settings, post-processing volumes, and Decal Projectors. Use whenever the user wants to change how the scene looks (colors, lighting, shaders, render pipeline assets, post-fx) through Editor automation rather than by hand. Read unity-core first for the REST protocol before calling any skill named here.
---

# Unity Rendering

Part of the Unity Skills plugin. **Read the [unity-core](../unity-core/SKILL.md) skill first** for the First Contact Checklist (health check, operating mode, dryRun gate).

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

Mode legend defined in unity-core's [MODULE_INDEX.md](../unity-core/MODULE_INDEX.md).

## Routing

Skills are named `<module>_<verb>` (e.g. `material_set_property`, `shadergraph_create`, `urp_get_renderer`). For Shader Graph work, read `shadergraph-design` for source-anchored node/version rules before authoring graphs — schema alone won't tell you which nodes exist in the user's SRP version.

Open the matching module doc before calling any skill whose exact parameters you don't already hold — dryRun first per the unity-core protocol.
