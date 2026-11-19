---
name: unity-animation-timeline
description: Automate Unity Animator controllers, Timeline tracks/clips, Cinemachine virtual cameras, and Scene View camera control from chat. Use when the user wants animation state machines, cutscenes/sequences, or camera work set up through Editor automation. Read unity-core first for the REST protocol before calling any skill named here.
---

# Unity Animation & Timeline

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
| animator | FA | Animator controllers | [modules/animator/SKILL.md](modules/animator/SKILL.md) |
| timeline | FA* | Timeline tracks/clips | [modules/timeline/SKILL.md](modules/timeline/SKILL.md) |
| cinemachine | FA* | VCam operations | [modules/cinemachine/SKILL.md](modules/cinemachine/SKILL.md) |
| camera | FA | Scene View camera | [modules/camera/SKILL.md](modules/camera/SKILL.md) |

Mode legend: SA = runs without grant in all modes, FA = needs grant under Approval, Mixed = check per-skill, `*` = contains auto-forbidden skills.

## Routing

Skills are named `<module>_<verb>` (e.g. `animator_create_controller`, `timeline_add_clip`, `cinemachine_create_vcam`). For tweened/scripted motion instead of Animator/Timeline state machines, see the unity-tweening skill (DOTween/PrimeTween) instead.

Open the matching module doc before calling any skill whose exact parameters you don't already hold — dryRun first.
