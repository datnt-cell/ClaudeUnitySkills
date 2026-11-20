---
name: unity-animation-timeline
description: Automate Unity Animator controllers, Timeline tracks/clips, Cinemachine virtual cameras, and Scene View camera control from chat. Use when the user wants animation state machines, cutscenes/sequences, or camera work set up through Editor automation. Not for diagnosing retargeting, root motion, foot sliding, IK or camera jitter — that is scenario-unity-animation (plugin 02). Read unity-core first for the REST protocol before calling any skill named here.
---

# Unity Animation & Timeline

## Protocol

Read [`unity-core`](../unity-core/SKILL.md) before the first REST call of the session (health check + `projectName` match, operating mode, schema layers, error codes). Rules that hold for every module below:

- **Never guess parameters.** `POST /skill/<name>?mode=dryRun` until `valid:true`, then run the same call for real. Module docs hold guardrails and traps; exact signatures come from `GET /skills/recommend?includeSchema=true` or `/skills/schema`.
- **2+ objects → `POST /skills/batch`** (≤50 steps, also takes `?mode=dryRun`).
- `MODE_RESTRICTED` → ask the user for a grant. `SURFACE_EXCLUDED` is a configuration boundary: don't retry or route around it — switch to guide mode ([`unity-core/SKILL_GUIDE.md`](../unity-core/SKILL_GUIDE.md)).

## Modules in this skill

| Module | Mode | Description | Doc |
|---|:---:|---|---|
| animator | FA | Animator controllers | [modules/animator/SKILL.md](modules/animator/SKILL.md) |
| timeline | FA* | Timeline tracks/clips | [modules/timeline/SKILL.md](modules/timeline/SKILL.md) |
| cinemachine | FA* | VCam operations | [modules/cinemachine/SKILL.md](modules/cinemachine/SKILL.md) |
| camera | FA | Scene View camera | [modules/camera/SKILL.md](modules/camera/SKILL.md) |

Mode legend: SA = runs without grant in all modes, FA = needs grant under Approval, Mixed = check per-skill, `*` = contains auto-forbidden skills.

## Routing

Skills are named `<module>_<verb>` (e.g. `animator_create_controller`, `timeline_add_clip`, `cinemachine_create_vcam`). For tweened/scripted motion instead of Animator/Timeline state machines, see the unity-tweening skill (DOTween/PrimeTween) instead.

Open the matching module doc before calling any skill whose exact parameters you don't already hold — dryRun first.
