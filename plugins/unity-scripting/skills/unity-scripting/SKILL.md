---
name: unity-scripting
description: Create and edit C# scripts and ScriptableObjects in Unity, plus scripting-design advisories — assembly definitions (asmdef), script roles, testability, patterns, and async-model choice. Use when the user wants scripts written/read/updated through Editor automation, or wants review/guidance on script architecture. Read unity-core first for the REST protocol before calling any skill named here.
---

# Unity Scripting

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
| script | SA* | Script create/read/update | [modules/script/SKILL.md](modules/script/SKILL.md) |
| scriptableobject | Mixed* | ScriptableObject assets | [modules/scriptableobject/SKILL.md](modules/scriptableobject/SKILL.md) |
| script-roles | — | Assign class roles (advisory, no REST skills) | [modules/script-roles/SKILL.md](modules/script-roles/SKILL.md) |
| scriptdesign | — | Review script structure (advisory, no REST skills) | [modules/scriptdesign/SKILL.md](modules/scriptdesign/SKILL.md) |
| asmdef | — | Plan asmdef deps (advisory, no REST skills) | [modules/asmdef/SKILL.md](modules/asmdef/SKILL.md) |
| testability | — | Extract testable logic (advisory, no REST skills) | [modules/testability/SKILL.md](modules/testability/SKILL.md) |
| patterns | — | Choose patterns (advisory, no REST skills) | [modules/patterns/SKILL.md](modules/patterns/SKILL.md) |
| async | — | Choose async model (advisory, no REST skills) | [modules/async/SKILL.md](modules/async/SKILL.md) |

Mode legend: SA = runs without grant in all modes, FA = needs grant under Approval, Mixed = check per-skill, `*` = contains auto-forbidden skills.

## Routing

Skills are named `<module>_<verb>` (e.g. `script_create`, `script_update`, `scriptableobject_create`). The advisory modules (script-roles, scriptdesign, asmdef, testability, patterns, async) define no REST skills — read them for guidance/review before or after writing scripts, not for API calls.

After creating or editing a script, verify compilation via the unity-dev-workflow skill's `debug`/`console` modules — don't assume success from the write response alone.

Open the matching module doc before calling any skill whose exact parameters you don't already hold — dryRun first.
