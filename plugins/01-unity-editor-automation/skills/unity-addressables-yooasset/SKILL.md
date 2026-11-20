---
name: unity-addressables-yooasset
description: Automate Unity Addressables and YooAsset hot-update/asset-bundle pipelines, plus HybridCLR hot-update codegen, from chat — group/Collector CRUD, asset entry assignment, profile switching, content builds, BuildReport analysis, and DLL compile/copy. Use when the user wants asset-bundle or hot-update workflows wired up through Editor automation. For Addressables inside CI or command-line builds, or on WebGL, use scenario-unity-pipeline-automation or scenario-unity-web (plugin 02). Read unity-core first for the REST protocol before calling any skill named here.
---

# Unity Addressables & YooAsset

## Protocol

Read [`unity-core`](../unity-core/SKILL.md) before the first REST call of the session (health check + `projectName` match, operating mode, schema layers, error codes). Rules that hold for every module below:

- **Never guess parameters.** `POST /skill/<name>?mode=dryRun` until `valid:true`, then run the same call for real. Module docs hold guardrails and traps; exact signatures come from `GET /skills/recommend?includeSchema=true` or `/skills/schema`.
- **2+ objects → `POST /skills/batch`** (≤50 steps, also takes `?mode=dryRun`).
- `MODE_RESTRICTED` → ask the user for a grant. `SURFACE_EXCLUDED` is a configuration boundary: don't retry or route around it — switch to guide mode ([`unity-core/SKILL_GUIDE.md`](../unity-core/SKILL_GUIDE.md)).

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
