---
name: unity-game-feel-recipes
description: Concrete Unity implementations for game feel — hit stop wiring, screen shake and recoil kick (MonoBehaviour and Cinemachine 3), asymmetric camera damping, coyote time and jump buffering, squash and stretch on a visual child, and an impact feedback layering table.
license: MIT
---

# Feel recipes

Copy these patterns; do not re-derive them. All numbers are the ones shipped by
the `scenario-unity-2d` specialists (see
`plugins/02-unity-expert-6-3/skills/scenario-unity-2d/references/expert-notes.md` §8–9).
`[added]` values are authored, not sourced — retune them per game.

## 1. Wire an impact to hit stop, kick and shake

```csharp
using UnityEngine;
using AgentKit.TwoD;

public class ImpactFeedback : MonoBehaviour
{
    [SerializeField] CameraShake2D shake;      // on the camera (or Cinemachine rig)
    [SerializeField] float hitStopSeconds = 0.03f;   // 0.02–0.07 is the sourced range
    [SerializeField] float shakeAmplitude = 0.08f;   // units; keep >= 1 art pixel
    [SerializeField] float shakeDuration  = 0.12f;
    [SerializeField] float kickDistance   = 0.15f;

    public void OnHit(Vector2 hitDirection, Vector2 shotDirection)
    {
        HitStop.Request(hitStopSeconds);                       // unscaled, extends, restores
        shake.Shake(shakeAmplitude, shakeDuration);
        shake.Kick(shotDirection, kickDistance);               // recoil opposite the shot
    }
}
```

Rules that must hold:

- `HitStop` uses **unscaled** time and restores the **previous** `timeScale`, so
  a hit during slow motion returns to slow motion.
- Overlapping requests **extend** one end time (measured: 0.05 s then 0.06 s at
  +0.02 s freezes 0.080 s, not 0.11 s).
- `HitStop.Multiplier = 0` disables the effect; bind it to a settings slider.
- Never `Thread.Sleep`. Never leave `timeScale` at 0 on disable or destroy.

## 2. Shake with Cinemachine 3 instead of a transform component

`CameraShake2D` fights the Cinemachine Brain for the transform. With CM 3 use an
impulse source on the impact and a listener on the vcam:

```csharp
using Unity.Cinemachine;
using UnityEngine;

[RequireComponent(typeof(CinemachineImpulseSource))]
public class ImpactImpulse : MonoBehaviour
{
    /// <summary>Settings-menu gain: 0 disables impulse shake entirely (accessibility).</summary>
    public static float Multiplier = 1f;

    CinemachineImpulseSource m_Source;
    void Awake() => m_Source = GetComponent<CinemachineImpulseSource>();

    public void Shake(Vector3 velocity)
    {
        if (Multiplier <= 0f) return;
        m_Source.GenerateImpulseWithVelocity(velocity * Multiplier);
    }
}
```

Add `CinemachineImpulseListener` to the vcam, keep the noise profile short
(0.1–0.2 s) and amplitude at least one art pixel for pixel art. Reduce further
with the listener's `Gain`, and keep the `Multiplier` a **project-owned** static —
Cinemachine has no global shake multiplier of its own.

## 3. Pixel-correct shake

The Pixel Perfect Camera rounds the camera to the art grid, so sub-pixel shake
disappears. Round the offset yourself and warn if the amplitude is under a pixel:

```csharp
float u = 1f / pixelsPerUnit;                       // 1/16 for 16 PPU
off = new Vector2(Mathf.Round(off.x / u) * u, Mathf.Round(off.y / u) * u);
Debug.Assert(Mathf.Abs(amplitude) >= u, "Shake smaller than one art pixel is invisible");
```

## 4. Asymmetric vertical damping (follow camera)

Loose while rising so the player can see below, tight once falling fast, with
hysteresis so small vertical jitter does not flip modes:

| Parameter | Value |
|---|---|
| damping rising | 0.6 s |
| damping falling | 0.1 s |
| switch threshold | vertical speed below −15 u/s |
| return threshold | vertical speed ≥ 0 |
| blend | 0.2 s, lerp the **damping**, not the camera |

Measured in a 40-unit drop: fixed 0.6 s let the feet reach the bottom edge; the
switch kept 3.2–3.4 units visible.

## 5. Coyote time and jump buffer as timestamp windows

```csharp
float m_LastGroundedTime, m_LastPressedTime;
const float Coyote = 0.15f, Buffer = 0.2f;

void Awake() => m_LastPressedTime = float.MinValue;   // never 0: phantom jump on landing

void Update()
{
    if (jumpPressed) m_LastPressedTime = Time.time;   // latch input in Update
    if (grounded)    m_LastGroundedTime = Time.time;  // re-arm on landing
}

bool CanJump => Time.time - m_LastPressedTime <= Buffer
             && Time.time - m_LastGroundedTime <= Coyote;

void FixedUpdate()
{
    if (CanJump) { Jump(); m_LastPressedTime = float.MinValue; m_LastGroundedTime = float.MinValue; }
}
```

Give coyote, buffer, variable jump, fall clamp, apex hang and corner correction
each their own toggle so a human can A/B them.

## 6. Variable jump height: pick one, state it

| Method | Feels like | Use when |
|---|---|---|
| cut velocity 50 % on release | precise, snappy (Super Meat Boy) | tight platforming |
| raise gravity ×3 on release | smooth arc | general purpose |
| sustain upward speed while held | long controllable rise (Celeste reaches the same apex with a lower base impulse) | airy, forgiving jumps |

Mixing methods silently is the usual cause of "floaty".

## 7. Squash and stretch without breaking collisions

Animate a **visual child**; keep colliders and the root transform fixed.

```csharp
// DOTween / PrimeTween both work; 0.06 s of anticipation, 0.12 s of recovery [added]
visual.localScale = new Vector3(0.8f, 1.25f, 1f);   // stretch on takeoff
visual.localScale = new Vector3(1.25f, 0.75f, 1f);  // squash on land
// then tween back to Vector3.one with an OutBack ease
```

## 8. Impact layering table

Add channels in this order, and stop as soon as the hit reads clearly.

| Order | Channel | Typical value | Cost |
|---|---|---|---|
| 1 | hit stop | 0.02–0.07 s | free |
| 2 | directional camera kick | 0.1–0.2 units, decay 12/s [added] | free |
| 3 | camera shake | 1–3 art pixels, 0.1 s | free, needs opt-out |
| 4 | VFX impact / muzzle / flash | 1–3 frames minimum | fill rate |
| 5 | audio | layered, pitch-varied | voices |
| 6 | UI punch (health bar, combo) | scale 1 → 1.1 → 1 | canvas rebuild |
| 7 | knockback | hit-stop-synchronised | gameplay state |

Anything beyond the first three must justify its frame cost in
`scenario-unity-performance` terms, and everything must survive a shake-off
setting.

## 9. Verify

- PlayMode tests with simulated input (`ut_run.run_tests(P, "PlayMode", graphics=True)`).
- Measure: apex height and rise time, jump count per press at 30/60/144 fps,
  air drift in body widths, hit-stop wall-clock duration and restored
  `timeScale`.
- Capture before/after clips for the human, then ask for the verdict.
