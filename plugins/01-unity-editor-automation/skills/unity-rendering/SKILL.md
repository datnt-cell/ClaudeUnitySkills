---
name: unity-rendering
description: Configure Unity rendering from chat — materials, lights, shaders, Shader Graph, URP/HDRP settings, post-processing volumes, and Decal Projectors. Use whenever the user wants to change how the scene looks (colors, lighting, shaders, render pipeline assets, post-fx) through Editor automation rather than by hand. Read unity-core first for the REST protocol before calling any skill named here.
---

# Unity Rendering

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
