---
name: unity-assets-pipeline
description: Manage Unity's asset pipeline from chat — asset refresh/find/info, texture/audio/model importers, unused/duplicate asset cleanup, asset optimization, UPM package install/query, and broken-reference validation. Use when the user wants asset-level operations across the project rather than editing one specific asset's content. Not for writing AssetPostprocessor rules, prefab generators, content-validation tests or CI builds — that is scenario-unity-pipeline-automation (plugin 02). Read unity-core first for the REST protocol before calling any skill named here.
---

# Unity Assets Pipeline

## Protocol

Read [`unity-core`](../unity-core/SKILL.md) before the first REST call of the session (health check + `projectName` match, operating mode, schema layers, error codes). Rules that hold for every module below:

- **Never guess parameters.** `POST /skill/<name>?mode=dryRun` until `valid:true`, then run the same call for real. Module docs hold guardrails and traps; exact signatures come from `GET /skills/recommend?includeSchema=true` or `/skills/schema`.
- **2+ objects → `POST /skills/batch`** (≤50 steps, also takes `?mode=dryRun`).
- `MODE_RESTRICTED` → ask the user for a grant. `SURFACE_EXCLUDED` is a configuration boundary: don't retry or route around it — switch to guide mode ([`unity-core/SKILL_GUIDE.md`](../unity-core/SKILL_GUIDE.md)).

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
