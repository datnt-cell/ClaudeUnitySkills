---
name: unity-tweening
description: Automate DOTween Pro and PrimeTween tween/sequence authoring in Unity, plus UniTask async patterns, from chat. Use when the user wants scripted motion, easing, tween sequences, or async/await code generated for the Unity player loop through Editor automation. Read unity-core first for the REST protocol before calling any skill named here.
---

# Unity Tweening

Part of the Unity Skills plugin. **Read the [unity-core](../unity-core/SKILL.md) skill first** for the First Contact Checklist (health check, operating mode, dryRun gate).

## Modules in this skill

| Module | Mode | Description | Doc |
|---|:---:|---|---|
| dotween | Mixed* | DOTween Pro DOTweenAnimation editor-time configuration (add/batch/stagger/tune) | [modules/dotween/SKILL.md](modules/dotween/SKILL.md) |
| dotween-design | — | DOTween 1.3.015 source-anchored rules (advisory, no REST skills) | [modules/dotween-design/SKILL.md](modules/dotween-design/SKILL.md) |
| primetween | Mixed* | PrimeTween Free inspection, factory discovery, runtime tween/sequence script generation | [modules/primetween/SKILL.md](modules/primetween/SKILL.md) |
| primetween-design | — | PrimeTween 1.4.6 source-anchored rules (advisory, no REST skills) | [modules/primetween-design/SKILL.md](modules/primetween-design/SKILL.md) |
| unitask-design | — | UniTask 2.5.10 source-anchored rules: basics/playerloop/cancellation/composition/conversion/asyncenumerable/triggers/pitfalls (advisory, no REST skills) | [modules/unitask-design/SKILL.md](modules/unitask-design/SKILL.md) |

Mode legend defined in unity-core's [MODULE_INDEX.md](../unity-core/MODULE_INDEX.md).

## Routing

Skills are named `<module>_<verb>` (e.g. `dotween_add_animation`, `primetween_generate_sequence`). DOTween and PrimeTween are alternative tweening libraries — check which one the project has installed before picking a module. **Always read the matching `*-design` doc before generating tween code**: these are source-anchored to exact package versions and catalog lifetime/cancellation pitfalls (e.g. tweens outliving destroyed GameObjects) that the REST schema won't warn about. For pure async/await without tweening, read `unitask-design`.

Open the matching module doc before calling any skill whose exact parameters you don't already hold — dryRun first per the unity-core protocol.
