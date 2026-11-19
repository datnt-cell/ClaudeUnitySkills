---
name: unity-game-feel
description: "Use when a Unity game must feel better rather than merely work: 'the jump feels floaty', 'combat has no impact', 'it feels unresponsive', 'add juice', 'the camera loses the player', 'the drop feels bad', 'shake is too much / makes me sick'. Covers input latency and buffering, coyote time and jump buffer, variable jump height, fall clamp, apex hang, corner correction, hazard hitbox forgiveness, camera follow bias and asymmetric fall damping, camera kick, hit stop, screen shake, layered impact feedback with VFX, audio and UI, and per-feature accessibility opt-outs. Also use for tuning passes on an existing controller (cảm giác chơi, độ nhạy, game feel, juice, phản hồi va chạm) before or after a playtest."
license: MIT
---

# Unity game feel (feel and juice designer)

Feel is not a polish pass at the end; it is a set of separately tunable
decisions, each with a number, each behind a toggle, each proven by a
measurement and then judged by a human. The agent's job is to make the
decision legible: state the profile (precision vs momentum vs arcade), state
the number, ship the on/off switch, and never claim "it feels good" from a test.

Two rules kill most bad feel work:

1. **Every forgiveness trick is a separate code path with its own toggle.**
   Coyote time, jump buffer, variable height, fall clamp, apex hang, corner
   correction, hit-stop and shake each get an on/off flag so a human can A/B
   them. Tarodev ships them toggled in the demo; Nijman plays with and without
   every effect (see `plugins/02-unity-expert-6-3/skills/scenario-unity-2d/references/expert-notes.md` §8–9).
2. **Feel is a human verdict.** Tests prove timing and invariants, not
   pleasure. Deliver numbers, toggles and captures; ask for the verdict.

## The five layers

| Layer | What the player blames | Owned by |
|---|---|---|
| Input and latency | "unresponsive", "dropped my jump" | latch in `Update`, apply in `FixedUpdate`, buffer windows |
| Movement rule | "floaty", "sticky", "slides off ledges" | accel/decel, air friction, variable height, fall clamp, apex band |
| Camera | "I lost the player", "nauseous" | damping, bias, room swaps, kick, shake |
| Impact feedback | "combat has no weight" | hit stop, VFX, audio, knockback, flash, shake |
| Presentation | "looks cheap" | squash and stretch, anticipation, particles, UI punch, easing |

Fix in that order. A perfect shake cannot rescue dropped input.

## Numbers that ship

Every value below is traceable to the sources in
`plugins/02-unity-expert-6-3/skills/scenario-unity-2d/references/`; `[added]` marks
values the skills themselves mark as authored rather than sourced.

| Value | Meaning | Source |
|---|---|---|
| 0.02–0.07 s | hit stop duration per impact (≈20 ms per hit; Celeste freezes ~4 frames ≈67 ms) | Nijman `AJdEqssNZ-U` [00:18:22]; GMTK `yorTG9at90g` [00:08:40] |
| extend, never stack | overlapping hit-stop requests share one end time | `HitStop.cs` [added] |
| 0.15 s | coyote time window | Tarodev stats |
| 0.2 s | jump buffer window | Tarodev stats |
| release ×3 gravity / cut velocity 50% / sustain while held | three variable-jump methods with different feel | Tarodev x3; Dawnosaur; Celeste VarJumpTime 0.2 s |
| gravity ×0.5 below 5 u/s near the apex (+0.08 s) | apex hang | Dawnosaur [00:02:06] [added] |
| 0.25 units | corner correction nudge | Tarodev [00:01:52] [added] |
| inset 2 px sides, 3 px top on a 16×8 spike (0.75 × 0.625) | hazard hitbox smaller than art | GMTK [00:09:28]; `Hazard2D.FitHitbox` [added] |
| 0.6 s loose / 0.1 s tight, switch below −15 u/s, back at 0, 0.2 s blend | asymmetric vertical camera damping | Sasquatch `9dzBrLUIF8g` [00:05:37]; `CinemachineFallDamping` [added] |
| AirDecel 30 → 3.13 units drift; 240 → 0.27 | air friction decides precision vs flow | measured replay |
| ~6 frames to top speed, 3 to stop, jump ≈3 body heights | Celeste reference profile | GMTK [00:02:01] [00:03:42] |
| Multiplier = 0 disables shake and hit stop | accessibility, not a debug switch | Nijman [00:32:14] |
| shake offsets rounded to 1/PPU, amplitude ≥ 1 art pixel | sub-pixel shake vanishes under Pixel Perfect Camera | `CameraShake2D.cs` [added] |

## Workflow

1. **Declare the profile.** Ask once: precision platformer (own the velocity,
   high air friction, static room cameras), momentum/flow (low air friction,
   follow camera), physics-rich (dynamic body, explicit knockback), or top-down.
   The profile decides air friction, jump method and camera model — never
   default them.
2. **Instrument before tuning.** PlayMode tests with simulated input
   (`ut_run.run_tests(P, "PlayMode", graphics=True)`), recording apex height,
   rise time, air drift in body widths, and one-press-one-jump at 30/60/144 fps.
3. **Tune one layer at a time.** Change a number, capture before/after, keep the
   toggle. State the measured delta, not an adjective.
4. **Layer impact feedback deliberately.** Order: hit stop → directional camera
   kick → shake → VFX/muzzle/impact → audio → UI punch → knockback. Nijman's
   ordering rule is that each additional jolt costs player attention and
   desensitises them over time, so add cheap-and-clear before expensive-and-loud.
5. **Ship settings, not opinions.** Expose shake intensity, hit-stop scale,
   screen flash, camera motion and shake-off in the options menu; store the
   values; verify they survive a build relaunch.
6. **Ask for the human verdict.** Report Verified (measured) and Assumed
   (GUI steps, taste) separately.

## Reusable implementations

The 2D specialist ships working, tested components — copy, do not re-invent:

| Component | Use for |
|---|---|
| `plugins/02-unity-expert-6-3/skills/scenario-unity-2d/scripts/Runtime/TwoD/HitStop.cs` | global freeze on unscaled time, restores the previous time scale, static `Multiplier` |
| `plugins/02-unity-expert-6-3/skills/scenario-unity-2d/scripts/Runtime/TwoD/CameraShake2D.cs` | Perlin shake + recoil kick, pixel-grid rounding, shake-off multiplier |
| `plugins/02-unity-expert-6-3/skills/scenario-unity-2d/scripts/Runtime/TwoD/PlatformerController2D.cs` | input latching, coyote/buffer, one-way platforms, corner correction |
| `plugins/02-unity-expert-6-3/skills/scenario-unity-2d/scripts/Runtime/TwoD/PlatformerStats.cs` | every forgiveness trick behind a toggle |
| `plugins/02-unity-expert-6-3/skills/scenario-unity-2d/scripts/Runtime/TwoDCinemachine/CinemachineFallDamping.cs` | asymmetric damping with hysteresis |
| `plugins/02-unity-expert-6-3/skills/scenario-unity-2d/scripts/Runtime/TwoDCinemachine/CinemachineRoomSwap.cs` | room cameras swapped on trigger exit by direction |

Recipes and wiring: [modules/feel-recipes/SKILL.md](modules/feel-recipes/SKILL.md).
Scored review rubric: [modules/feel-audit/SKILL.md](modules/feel-audit/SKILL.md).

## Common mistakes

| Mistake | What it looks like | Fix |
|---|---|---|
| input read in `FixedUpdate` | presses lost at high fps | latch in `Update`, consume in `FixedUpdate` |
| buffered-jump timestamp initialised to `0` | phantom jump on first landing in the first 0.2 s | initialise to `float.MinValue` |
| hit stop implemented with `Thread.Sleep` or `timeScale` never restored | frozen or slow-motion game | unscaled end time, restore the previous scale |
| overlapping hit stops multiplied | 50 ms + 60 ms freezes 110 ms | extend one end time (measured 0.080 s) |
| shake built from `Random` per frame | buzz, not impact | Perlin or impulse decay, round to art pixels |
| shake with no off switch | motion sickness, unreadable pixel art | `Multiplier` in settings, default on |
| one air-friction value for every game | slides off small platforms, or no momentum | choose by level design, state the drift |
| hazard collider equal to the art | death on a 1 px graze | inset hitbox, ratio below 1 |
| fixed vertical damping on a follow camera | player falls out of frame | asymmetric damping with hysteresis |
| room camera swapped on trigger enter | wrong camera when the player turns back | swap on exit, by exit direction |
| squash/stretch on a collider-bearing root | hitboxes breathe and tunnel | animate a visual child, keep colliders fixed |
| feel claimed from a passing test | team ships something nobody likes | numbers + toggles + human A/B |

## Quality gates

- Each forgiveness/feel trick has a toggle and a PlayMode test that fails when
  the trick is removed.
- One press yields exactly one jump at 30, 60 and 144 fps.
- Hit stop restores the previous time scale and extends rather than stacks;
  `Multiplier = 0` disables it.
- Shake and flash can be disabled from the options menu and the choice persists
  across a build relaunch.
- Every tuning claim is reported as measured value plus source frame or test.

## Handoffs

- Controller, camera rig and juice components: `scenario-unity-2d`
  (`plugins/02-unity-expert-6-3/skills/scenario-unity-2d/`).
- Impact VFX, muzzle/impact/trail readability: `scenario-unity-vfx`.
- Camera rigs, blend tuning, Timeline cinematics: `scenario-unity-animation`.
- Combat audio, mixers, ducking: `scenario-unity-gameplay`.
- Tween-based UI punch and squash: `unity-tweening`
  (`plugins/01-unity-editor-automation/skills/unity-tweening/`).
- Frame cost of the effects: `scenario-unity-performance`; on phones also
  `scenario-unity-mobile`.
