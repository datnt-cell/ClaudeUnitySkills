---
name: unity-tween-async
description: "Write DOTween Pro and PrimeTween tweens and sequences and UniTask async code in Unity, with version-pinned API rules (DOTween 1.3.015, PrimeTween, UniTask 2.5.10) that catch hallucinated or removed APIs and lifetime/cancellation pitfalls. Use for scripted motion, easing, sequences, UI punch, or async/await on the player loop. Tween authoring also runs directly in an open Editor via the UnitySkills REST server."
license: MIT
---
# Unity tweens and async (DOTween, PrimeTween, UniTask)

> **Layout:** understand → [`references/`](references/) · do it in an open Editor → [`actions/`](actions/) (REST, section below).

Tweening and async code on the Unity player loop. Read the matching `references/*-design` doc before generating code: each is pinned to an exact package version and catalogs lifetime and cancellation traps. Game-feel decisions (how much punch, how long) belong to `unity-game-feel`.

## Doing it in the open Editor (REST)

Needs the UnitySkills package in the project (REST server on `localhost:8090`-`8100`). Read [`../unity-core/rest/PROTOCOL.md`](../unity-core/rest/PROTOCOL.md) before the first call of the session. Hard rules for every module below:

- **Never guess parameters.** `POST /skill/<name>?mode=dryRun` until `valid:true`, then run the same call for real. Module docs hold guardrails and traps; exact signatures come from `GET /skills/recommend?includeSchema=true` or `/skills/schema`.
- **2+ objects → `POST /skills/batch`** (≤50 steps, also takes `?mode=dryRun`).
- `MODE_RESTRICTED` → ask the user for a grant. `SURFACE_EXCLUDED` is a configuration boundary: don't retry or route around it — switch to guide mode ([`../unity-core/rest/GUIDE.md`](../unity-core/rest/GUIDE.md)).
- After any write, check compilation and new errors with `unity-core` → `actions/console`, `actions/debug` before reporting success.

| Module | Mode | What it does | Doc |
|---|:---:|---|---|
| dotween | Mixed* | DOTween Pro DOTweenAnimation editor-time configuration (add/batch/stagger/tune) | [actions/dotween/SKILL.md](actions/dotween/SKILL.md) |
| dotween-design | — | DOTween 1.3.015 source-anchored rules (advisory, no REST skills) | [references/dotween-design/SKILL.md](references/dotween-design/SKILL.md) |
| primetween | Mixed* | PrimeTween Free inspection, factory discovery, runtime tween/sequence script generation | [actions/primetween/SKILL.md](actions/primetween/SKILL.md) |
| primetween-design | — | PrimeTween 1.4.6 source-anchored rules (advisory, no REST skills) | [references/primetween-design/SKILL.md](references/primetween-design/SKILL.md) |
| unitask-design | — | UniTask 2.5.10 source-anchored rules: basics/playerloop/cancellation/composition/conversion/asyncenumerable/triggers/pitfalls (advisory, no REST skills) | [references/unitask-design/SKILL.md](references/unitask-design/SKILL.md) |

Mode: SA = runs without grant in every mode, FA = needs a grant under Approval, Mixed = per skill, `*` = has auto-forbidden skills, — = documentation only.

Skills are named `<module>_<verb>` (e.g. `dotween_add_animation`, `primetween_generate_sequence`). DOTween and PrimeTween are alternative tweening libraries — check which one the project has installed before picking a module. **Always read the matching `*-design` doc before generating tween code**: these are source-anchored to exact package versions and catalog lifetime/cancellation pitfalls (e.g. tweens outliving destroyed GameObjects) that the REST schema won't warn about. For pure async/await without tweening, read `unitask-design`.
