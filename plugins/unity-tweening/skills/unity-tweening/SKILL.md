---
name: unity-tweening
description: Automate DOTween Pro and PrimeTween tween/sequence authoring in Unity, plus UniTask async patterns, from chat. Use when the user wants scripted motion, easing, tween sequences, or async/await code generated for the Unity player loop through Editor automation. Read unity-core first for the REST protocol before calling any skill named here.
---

# Unity Tweening

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
| dotween | Mixed* | DOTween Pro DOTweenAnimation editor-time configuration (add/batch/stagger/tune) | [modules/dotween/SKILL.md](modules/dotween/SKILL.md) |
| dotween-design | — | DOTween 1.3.015 source-anchored rules (advisory, no REST skills) | [modules/dotween-design/SKILL.md](modules/dotween-design/SKILL.md) |
| primetween | Mixed* | PrimeTween Free inspection, factory discovery, runtime tween/sequence script generation | [modules/primetween/SKILL.md](modules/primetween/SKILL.md) |
| primetween-design | — | PrimeTween 1.4.6 source-anchored rules (advisory, no REST skills) | [modules/primetween-design/SKILL.md](modules/primetween-design/SKILL.md) |
| unitask-design | — | UniTask 2.5.10 source-anchored rules: basics/playerloop/cancellation/composition/conversion/asyncenumerable/triggers/pitfalls (advisory, no REST skills) | [modules/unitask-design/SKILL.md](modules/unitask-design/SKILL.md) |

Mode legend: SA = runs without grant in all modes, FA = needs grant under Approval, Mixed = check per-skill, `*` = contains auto-forbidden skills.

## Routing

Skills are named `<module>_<verb>` (e.g. `dotween_add_animation`, `primetween_generate_sequence`). DOTween and PrimeTween are alternative tweening libraries — check which one the project has installed before picking a module. **Always read the matching `*-design` doc before generating tween code**: these are source-anchored to exact package versions and catalog lifetime/cancellation pitfalls (e.g. tweens outliving destroyed GameObjects) that the REST schema won't warn about. For pure async/await without tweening, read `unitask-design`.

Open the matching module doc before calling any skill whose exact parameters you don't already hold — dryRun first.
