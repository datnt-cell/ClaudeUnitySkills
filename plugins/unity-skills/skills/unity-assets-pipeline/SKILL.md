---
name: unity-assets-pipeline
description: Manage Unity's asset pipeline from chat — asset refresh/find/info, texture/audio/model importers, unused/duplicate asset cleanup, asset optimization, UPM package install/query, and broken-reference validation. Use when the user wants asset-level operations across the project rather than editing one specific asset's content. Read unity-core first for the REST protocol before calling any skill named here.
---

# Unity Assets Pipeline

Part of the Unity Skills plugin. **Read the [unity-core](../unity-core/SKILL.md) skill first** for the First Contact Checklist (health check, operating mode, dryRun gate).

## Modules in this skill

| Module | Mode | Description | Doc |
|---|:---:|---|---|
| asset | SA* | Asset refresh/find/info | [modules/asset/SKILL.md](modules/asset/SKILL.md) |
| importer | Mixed | Texture/audio/model import | [modules/importer/SKILL.md](modules/importer/SKILL.md) (+ [IMPORT_REFERENCE.md](modules/importer/IMPORT_REFERENCE.md)) |
| cleaner | SA* | Unused/duplicate assets | [modules/cleaner/SKILL.md](modules/cleaner/SKILL.md) |
| optimization | Mixed | Asset optimization | [modules/optimization/SKILL.md](modules/optimization/SKILL.md) |
| package | Mixed* | UPM install/query | [modules/package/SKILL.md](modules/package/SKILL.md) |
| validation | SA* | Broken reference checks | [modules/validation/SKILL.md](modules/validation/SKILL.md) |

Mode legend defined in unity-core's [MODULE_INDEX.md](../unity-core/MODULE_INDEX.md).

## Routing

Skills are named `<module>_<verb>` (e.g. `asset_find`, `importer_set_texture_settings`, `cleaner_find_unused`). Run `validation` before and after bulk cleanup/optimization passes to catch broken references early. `cleaner` and `optimization` skills can touch many assets at once — prefer `*_batch` where available and dryRun first.

Open the matching module doc before calling any skill whose exact parameters you don't already hold — dryRun first per the unity-core protocol.
