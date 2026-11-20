# Feel audit rubric

Score each row `pass`, `fail` or `n/a`, and demand evidence: a measured number, a
test name, or a capture. A row with no evidence is `fail` regardless of intent.
Sections A–E are ordered by how much players blame them.

## A. Responsiveness (must all pass)

| ID | Requirement | Evidence |
|---|---|---|
| A1 | Input is latched in `Update` and consumed in `FixedUpdate` | code path shown |
| A2 | One press produces exactly one action at 30, 60 and 144 fps | PlayMode test, three rates |
| A3 | Fixed timestep chosen and stated (2D default 0.02 s) | project settings |
| A4 | No `Input.GetAxis` legacy calls in a project using the Input System | grep result |
| A5 | Nothing blocks the frame on impact (no `Thread.Sleep`, no synchronous load) | profiler capture |

## B. Movement rule

| ID | Requirement | Evidence |
|---|---|---|
| B1 | Profile declared: precision, momentum, physics-rich or top-down | one sentence in the report |
| B2 | Accel, decel, air decel, max speed, gravity, jump impulse listed as numbers | table |
| B3 | Variable jump method chosen explicitly (cut 50 %, gravity ×3, or sustain) | setting + test |
| B4 | Air drift reported in body widths | measured number |
| B5 | Coyote, buffer, fall clamp, apex hang, corner correction each have a toggle | flags + A/B test |
| B6 | Hazard hitbox smaller than the art (ratio < 1) | inset values |
| B7 | Buffered-jump timestamp initialised to `float.MinValue`, not `0` | code |

## C. Camera

| ID | Requirement | Evidence |
|---|---|---|
| C1 | Camera model declared (static rooms, follow with bias, hybrid) | one sentence |
| C2 | Vertical damping asymmetric with hysteresis, values stated | 0.6 s / 0.1 s / −15 u/s / 0.2 s blend or equivalent |
| C3 | Player never leaves frame at max fall speed | capture of a 40-unit drop |
| C4 | Room cameras swap on trigger **exit** by direction | test both ways plus turn-back |
| C5 | Follow uses an eased, decoupled target; camera not parented to the player | hierarchy shown |
| C6 | No camera motion is applied that cannot be disabled | settings screen |

## D. Impact feedback

| ID | Requirement | Evidence |
|---|---|---|
| D1 | Hit stop in 0.02–0.07 s, on unscaled time | constant + test |
| D2 | Overlapping hit stops extend, not stack | measured total (0.080 s case) |
| D3 | Hit stop restores the **previous** time scale | test during a slow-motion state |
| D4 | Shake from noise or impulse decay, rounded to art pixels (pixel art) | capture pair at 2× |
| D5 | Camera kick opposes the fire direction | direction test |
| D6 | Each channel justified by what the player must read | one line per channel |
| D7 | Audio and VFX land on the same frame as the hit stop | frame-by-frame capture |

## E. Presentation

| ID | Requirement | Evidence |
|---|---|---|
| E1 | Squash/stretch or anticipation on takeoff and landing, on a visual child | hierarchy + clip |
| E2 | Hit flash or knockback readable at the lowest target resolution | capture at 720p |
| E3 | UI feedback (health, combo, counter) punches without a layout rebuild spike | profiler frame |
| E4 | Easing curves named and shared, not per-component one-offs | list |

## F. Accessibility (must all pass)

| ID | Requirement | Evidence |
|---|---|---|
| F1 | Shake intensity slider, including off (`Multiplier = 0`) | settings screen |
| F2 | Hit-stop scale or disable | settings screen |
| F3 | Screen-flash / strobe option | settings screen |
| F4 | Camera motion reduction option | settings screen |
| F5 | Choices persist across a build relaunch | manual check in a player |
| F6 | Default settings are comfortable, not maximal | defaults shown |

## G. Evidence discipline

| ID | Requirement |
|---|---|
| G1 | Every "feels better" claim paired with a before/after number or capture |
| G2 | Verified and Assumed listed separately (GUI steps and taste are Assumed) |
| G3 | Cheat sheets or debug overlay removed or gated from the shipped build |

## Reporting

```
Feel audit — <project> — <date>
Profile: precision platformer, 16 PPU, 320×180, 60 Hz physics
A Responsiveness : 5/5
B Movement       : 6/7  B4 fail — air drift not measured
C Camera         : 5/6  C3 fail — feet reach bottom edge on a 40-unit drop
D Impact         : 7/7  D2 measured 0.080 s
E Presentation   : 3/4
F Accessibility  : 6/6
G Evidence       : 2/3
First fix: C3 (asymmetric damping), then B4 (measure drift)
Assumed: shake amplitude taste judged by the art lead
```

Fix order: any `fail` in A or F blocks the rest; then C3-class frame losses;
then B; then D and E polish.
