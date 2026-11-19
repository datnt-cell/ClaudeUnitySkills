---
name: unity-addressables-yooasset
description: Automate Unity Addressables and YooAsset hot-update/asset-bundle pipelines, plus HybridCLR hot-update codegen, from chat — group/Collector CRUD, asset entry assignment, profile switching, content builds, BuildReport analysis, and DLL compile/copy. Use when the user wants asset-bundle or hot-update workflows wired up through Editor automation. Read unity-core first for the REST protocol before calling any skill named here.
---

# Unity Addressables & YooAsset

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
| addressables | Mixed* | Group CRUD, asset entry assignment, profile switching, content build (com.unity.addressables, reflection-based) | [modules/addressables/SKILL.md](modules/addressables/SKILL.md) |
| addressables-design | — | Dual-version (1.22.3 Unity 2022 / 2.9.1 Unity 6) source-anchored rules + migration table (advisory, no REST skills) | [modules/addressables-design/SKILL.md](modules/addressables-design/SKILL.md) |
| yooasset | Mixed* | Build bundles, Collector CRUD, BuildReport analysis, PlayMode validation, Reporter/Debugger/AssetArtScanner | [modules/yooasset/SKILL.md](modules/yooasset/SKILL.md) |
| yooasset-design | — | YooAsset v2.3.18 source-anchored rules (advisory, no REST skills) | [modules/yooasset-design/SKILL.md](modules/yooasset-design/SKILL.md) |
| hybridclr | Mixed* | HybridCLR hot-update settings, codegen, DLL compile/copy pipeline (com.code-philosophy.hybridclr, reflection-based) | [modules/hybridclr/SKILL.md](modules/hybridclr/SKILL.md) |

Mode legend: SA = runs without grant in all modes, FA = needs grant under Approval, Mixed = check per-skill, `*` = contains auto-forbidden skills.

## Routing

Skills are named `<module>_<verb>` (e.g. `addressables_create_group`, `yooasset_build_bundles`, `hybridclr_compile_dll`). Addressables and YooAsset are alternative/competing asset-bundle systems — check which one the target project actually has installed (`package_query` in unity-assets-pipeline, or the reflection-based check each module does internally) before picking a module; don't assume both are present.

**Version-sensitive**: `addressables-design` and `yooasset-design` are source-anchored to specific package versions. Read the matching design doc before authoring group/bundle configuration — API surfaces shift between the versions it documents.

Open the matching module doc before calling any skill whose exact parameters you don't already hold — dryRun first.
