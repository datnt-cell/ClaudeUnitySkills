---
name: unity-editor-tools
description: Low-level and fallback Unity Editor tooling from chat — hand-editing serialized YAML (.unity/.prefab/.asset/.meta/ProjectSettings) when REST can't reach the target, the experimental Unity CLI for headless cold-start test/run/build, and Unity Behavior graph automation (agents, blackboard variables). Use when REST skills from other unity-* skills can't reach what you need, or when the user explicitly wants headless CLI operation or Behavior graph editing. Read unity-core first for the REST protocol before calling any skill named here.
---

# Unity Editor Tools

Part of the Unity Skills plugin. **Read the [unity-core](../unity-core/SKILL.md) skill first** for the First Contact Checklist (health check, operating mode, dryRun gate).

## Modules in this skill

| Module | Mode | Description | Doc |
|---|:---:|---|---|
| yaml-editing | — | Safe hand-edit rules for serialized YAML when REST cannot reach — reference/fileID repair, .meta/GUID safety, ProjectSettings patch, merge conflict (advisory, no REST skills) | [modules/yaml-editing/SKILL.md](modules/yaml-editing/SKILL.md) |
| unity-cli | — | Experimental Unity CLI on the bound project (opt-in) — cold start with Editor closed, headless test/run/build, exit codes, JSON/NDJSON contract, hard DO-NOT list (advisory, no REST skills) | [modules/unity-cli/SKILL.md](modules/unity-cli/SKILL.md) |
| behavior | Mixed | Unity Behavior graph assets, agents, blackboard variables (com.unity.behavior, reflection-based) | [modules/behavior/SKILL.md](modules/behavior/SKILL.md) |

Mode legend defined in unity-core's [MODULE_INDEX.md](../unity-core/MODULE_INDEX.md).

## Routing

**`yaml-editing` is a last resort** — only hand-edit serialized YAML when the REST skills in every other unity-* skill genuinely can't reach the target (e.g. the Editor is closed, or the asset type has no REST coverage). Getting fileIDs/GUIDs wrong corrupts scenes/prefabs silently; read the module doc's safety rules before touching any `.unity`/`.prefab`/`.asset`/`.meta` file directly.

**`unity-cli`** requires the user to have opted in (`Library/UnitySkills/cli_config.json`) and runs with the Editor closed — read unity-core's [protocol-unity-cli.md](../unity-core/references/protocol-unity-cli.md) for the cold-start contract before using it.

`behavior` skills are named `behavior_<verb>` and follow the normal REST protocol — dryRun first per unity-core.
