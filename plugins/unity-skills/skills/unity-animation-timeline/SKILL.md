---
name: unity-animation-timeline
description: Automate Unity Animator controllers, Timeline tracks/clips, Cinemachine virtual cameras, and Scene View camera control from chat. Use when the user wants animation state machines, cutscenes/sequences, or camera work set up through Editor automation. Read unity-core first for the REST protocol before calling any skill named here.
---

# Unity Animation & Timeline

Part of the Unity Skills plugin. **Read the [unity-core](../unity-core/SKILL.md) skill first** for the First Contact Checklist (health check, operating mode, dryRun gate).

## Modules in this skill

| Module | Mode | Description | Doc |
|---|:---:|---|---|
| animator | FA | Animator controllers | [modules/animator/SKILL.md](modules/animator/SKILL.md) |
| timeline | FA* | Timeline tracks/clips | [modules/timeline/SKILL.md](modules/timeline/SKILL.md) |
| cinemachine | FA* | VCam operations | [modules/cinemachine/SKILL.md](modules/cinemachine/SKILL.md) |
| camera | FA | Scene View camera | [modules/camera/SKILL.md](modules/camera/SKILL.md) |

Mode legend defined in unity-core's [MODULE_INDEX.md](../unity-core/MODULE_INDEX.md).

## Routing

Skills are named `<module>_<verb>` (e.g. `animator_create_controller`, `timeline_add_clip`, `cinemachine_create_vcam`). For tweened/scripted motion instead of Animator/Timeline state machines, see the unity-tweening skill (DOTween/PrimeTween) instead.

Open the matching module doc before calling any skill whose exact parameters you don't already hold — dryRun first per the unity-core protocol.
