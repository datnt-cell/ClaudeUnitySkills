---
name: unity-scripting
description: Create, read and update C# scripts and ScriptableObject assets in the open Unity Editor through the UnitySkills REST server. Use when the user wants script files or SO assets actually written or changed in their project from chat. Not for architecture advice, pattern choice, asmdef planning or code review — that is scenario-unity-architecture (plugin 02). Read unity-core first for the REST protocol before calling any skill named here.
---

# Unity Scripting

## Protocol

Read [`unity-core`](../unity-core/SKILL.md) before the first REST call of the session (health check + `projectName` match, operating mode, schema layers, error codes). Rules that hold for every module below:

- **Never guess parameters.** `POST /skill/<name>?mode=dryRun` until `valid:true`, then run the same call for real. Module docs hold guardrails and traps; exact signatures come from `GET /skills/recommend?includeSchema=true` or `/skills/schema`.
- **2+ objects → `POST /skills/batch`** (≤50 steps, also takes `?mode=dryRun`).
- `MODE_RESTRICTED` → ask the user for a grant. `SURFACE_EXCLUDED` is a configuration boundary: don't retry or route around it — switch to guide mode ([`unity-core/SKILL_GUIDE.md`](../unity-core/SKILL_GUIDE.md)).

## Modules in this skill

| Module | Mode | Description | Doc |
|---|:---:|---|---|
| script | SA* | Script create/read/update | [modules/script/SKILL.md](modules/script/SKILL.md) |
| scriptableobject | Mixed* | ScriptableObject assets | [modules/scriptableobject/SKILL.md](modules/scriptableobject/SKILL.md) |

Mode legend: SA = runs without grant in all modes, FA = needs grant under Approval, Mixed = check per-skill, `*` = contains auto-forbidden skills.

## Routing

Skills are named `<module>_<verb>` (e.g. `script_create`, `script_update`, `scriptableobject_create`). Design guidance (script roles, patterns, async model, asmdef, testability, script review) is not here: read `scenario-unity-architecture` → `references/design/README.md` (plugin 02) before writing a batch of gameplay scripts.

After creating or editing a script, verify compilation via the unity-dev-workflow skill's `debug`/`console` modules — don't assume success from the write response alone.

Open the matching module doc before calling any skill whose exact parameters you don't already hold — dryRun first.
