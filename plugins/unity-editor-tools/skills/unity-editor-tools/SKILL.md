---
name: unity-editor-tools
description: Low-level and fallback Unity Editor tooling from chat — hand-editing serialized YAML (.unity/.prefab/.asset/.meta/ProjectSettings) when REST can't reach the target, the experimental Unity CLI for headless cold-start test/run/build, and Unity Behavior graph automation (agents, blackboard variables). Use when REST skills from other unity-* skills can't reach what you need, or when the user explicitly wants headless CLI operation or Behavior graph editing. Read unity-core first for the REST protocol before calling any skill named here.
---

# Unity Editor Tools

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
| yaml-editing | — | Safe hand-edit rules for serialized YAML when REST cannot reach — reference/fileID repair, .meta/GUID safety, ProjectSettings patch, merge conflict (advisory, no REST skills) | [modules/yaml-editing/SKILL.md](modules/yaml-editing/SKILL.md) |
| unity-cli | — | Experimental Unity CLI on the bound project (opt-in) — cold start with Editor closed, headless test/run/build, exit codes, JSON/NDJSON contract, hard DO-NOT list (advisory, no REST skills) | [modules/unity-cli/SKILL.md](modules/unity-cli/SKILL.md) |
| behavior | Mixed | Unity Behavior graph assets, agents, blackboard variables (com.unity.behavior, reflection-based) | [modules/behavior/SKILL.md](modules/behavior/SKILL.md) |

Mode legend: SA = runs without grant in all modes, FA = needs grant under Approval, Mixed = check per-skill, `*` = contains auto-forbidden skills.

## Routing

**`yaml-editing` is a last resort** — only hand-edit serialized YAML when the REST skills in every other unity-* skill genuinely can't reach the target (e.g. the Editor is closed, or the asset type has no REST coverage). Getting fileIDs/GUIDs wrong corrupts scenes/prefabs silently; read the module doc's safety rules before touching any `.unity`/`.prefab`/`.asset`/`.meta` file directly.

**`unity-cli`** requires the user to have opted in (`Library/UnitySkills/cli_config.json`) and runs with the Editor closed — read this plugin's own `modules/unity-cli/SKILL.md` for the cold-start contract before using it.

`behavior` skills are named `behavior_<verb>` and follow the normal REST protocol — dryRun first.
