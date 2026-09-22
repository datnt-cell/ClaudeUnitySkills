---
name: unity-scripting
description: Create and edit C# scripts and ScriptableObjects in Unity, plus scripting-design advisories — assembly definitions (asmdef), script roles, testability, patterns, and async-model choice. Use when the user wants scripts written/read/updated through Editor automation, or wants review/guidance on script architecture. Read unity-core first for the REST protocol before calling any skill named here.
---

# Unity Scripting

Part of the Unity Skills plugin. **Read the [unity-core](../unity-core/SKILL.md) skill first** for the First Contact Checklist (health check, operating mode, dryRun gate).

## Modules in this skill

| Module | Mode | Description | Doc |
|---|:---:|---|---|
| script | SA* | Script create/read/update | [modules/script/SKILL.md](modules/script/SKILL.md) |
| scriptableobject | Mixed* | ScriptableObject assets | [modules/scriptableobject/SKILL.md](modules/scriptableobject/SKILL.md) |
| script-roles | — | Assign class roles (advisory, no REST skills) | [modules/script-roles/SKILL.md](modules/script-roles/SKILL.md) |
| scriptdesign | — | Review script structure (advisory, no REST skills) | [modules/scriptdesign/SKILL.md](modules/scriptdesign/SKILL.md) |
| asmdef | — | Plan asmdef deps (advisory, no REST skills) | [modules/asmdef/SKILL.md](modules/asmdef/SKILL.md) |
| testability | — | Extract testable logic (advisory, no REST skills) | [modules/testability/SKILL.md](modules/testability/SKILL.md) |
| patterns | — | Choose patterns (advisory, no REST skills) | [modules/patterns/SKILL.md](modules/patterns/SKILL.md) |
| async | — | Choose async model (advisory, no REST skills) | [modules/async/SKILL.md](modules/async/SKILL.md) |

Mode legend defined in unity-core's [MODULE_INDEX.md](../unity-core/MODULE_INDEX.md).

## Routing

Skills are named `<module>_<verb>` (e.g. `script_create`, `script_update`, `scriptableobject_create`). The advisory modules (script-roles, scriptdesign, asmdef, testability, patterns, async) define no REST skills — read them for guidance/review before or after writing scripts, not for API calls.

After creating or editing a script, verify compilation via unity-core's [observability doc](../unity-core/references/protocol-observability.md) or the unity-dev-workflow skill's `debug`/`console` modules — don't assume success from the write response alone.

Open the matching module doc before calling any skill whose exact parameters you don't already hold — dryRun first per the unity-core protocol.
