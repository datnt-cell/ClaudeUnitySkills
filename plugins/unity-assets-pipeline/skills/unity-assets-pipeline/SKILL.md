---
name: unity-assets-pipeline
description: Manage Unity's asset pipeline from chat — asset refresh/find/info, texture/audio/model importers, unused/duplicate asset cleanup, asset optimization, UPM package install/query, and broken-reference validation. Use when the user wants asset-level operations across the project rather than editing one specific asset's content. Read unity-core first for the REST protocol before calling any skill named here.
---

# Unity Assets Pipeline

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
| asset | SA* | Asset refresh/find/info | [modules/asset/SKILL.md](modules/asset/SKILL.md) |
| importer | Mixed | Texture/audio/model import | [modules/importer/SKILL.md](modules/importer/SKILL.md) (+ [IMPORT_REFERENCE.md](modules/importer/IMPORT_REFERENCE.md)) |
| cleaner | SA* | Unused/duplicate assets | [modules/cleaner/SKILL.md](modules/cleaner/SKILL.md) |
| optimization | Mixed | Asset optimization | [modules/optimization/SKILL.md](modules/optimization/SKILL.md) |
| package | Mixed* | UPM install/query | [modules/package/SKILL.md](modules/package/SKILL.md) |
| validation | SA* | Broken reference checks | [modules/validation/SKILL.md](modules/validation/SKILL.md) |

Mode legend: SA = runs without grant in all modes, FA = needs grant under Approval, Mixed = check per-skill, `*` = contains auto-forbidden skills.

## Routing

Skills are named `<module>_<verb>` (e.g. `asset_find`, `importer_set_texture_settings`, `cleaner_find_unused`). Run `validation` before and after bulk cleanup/optimization passes to catch broken references early. `cleaner` and `optimization` skills can touch many assets at once — prefer `*_batch` where available and dryRun first.

Open the matching module doc before calling any skill whose exact parameters you don't already hold — dryRun first.
