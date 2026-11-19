---
name: audio-post-production
description: "Use when generated or recorded audio must become a finished, mixed, game-ready set: picking takes, gain staging and loudness targets, seamless loops, crossfades and transitions, layering music/ambience/SFX/voice with ducking, stem and file naming, mono versus stereo decisions, Unity AudioSource import settings, AudioMixer groups and snapshots, Audio Random Container variations, and a delivery QA checklist. Triggers also include 'ghép âm thanh', 'mix lại', 'audio bị rè/chồng tiếng', 'vòng lặp bị giật', 'loop bị click', 'âm lượng không đều', 'âm thanh trong Unity nghe nhỏ', 'nhiều tiếng quá chồng'."
license: MIT
---

# Audio post-production (assembly, mix, delivery)

Assembly is an ordered pipeline, not an ear-pass at the end: **intake → gain
staging → edit and loop → layer → duck → mix bus → verify → export**. Every step
leaves a number or a file, and nothing ships because it "sounds fine" once.

This skill consumes output from the Scenario generation skills in this plugin
(`scenario-audio`, `scenario-elevenlabs`, `scenario-sonilo`, `scenario-ace-step`,
`scenario-minimax-music`, `scenario-video-assembly`). Values marked **[conv]** are
audio-engineering conventions — confirm them against the target platform's
specification rather than treating them as absolutes.

## 1. Intake: what a usable asset looks like

| Check | Requirement | Why |
|---|---|---|
| Format | keep the master as WAV; MP3 only for previews | every re-encode loses transients |
| Sample rate | 48 kHz for a project mixed at 48 kHz; resample once, at import | mismatched rates cause resampling per playback |
| Channels | mono for positional (3D) SFX and voice; stereo for music, ambience, UI beds | mono plays correctly through 3D panning; stereo collapses |
| Peak | no sample at 0 dBFS; leave ≥ 1 dB of headroom before processing | clipping is unrecoverable |
| Loop flag | stated explicitly (looped or one-shot) in the filename metadata | loops need a different edit |
| Provenance | model id, seed/params, and the prompt kept beside the file | regenerating a variation needs the exact contract |

Reject and regenerate rather than repair: a clipped take, a take with a spoken
tag (`[whispers]` read aloud), or a loop whose start and end do not match.

## 2. Gain staging and loudness

Work in this order: trim peaks → set per-clip target → set bus levels → master.
Never raise a fader to fix a quiet source; normalise the clip.

| Bus | Typical integrated target **[conv]** | Notes |
|---|---|---|
| Dialogue / voice | −18 to −16 LUFS | the anchor; everything else ducks under it |
| SFX (one-shots) | −20 to −16 LUFS short-term, peaks ≤ −1 dBTP | varied by importance, not uniformly |
| Music bed | −24 to −18 LUFS | drops further under dialogue via ducking |
| Ambience | −28 to −22 LUFS | must not mask gameplay cues |
| Master out | platform-defined (commonly −16 to −14 LUFS for mobile/PC, −23 LUFS for broadcast-style) | verify per store/platform spec |

Measure with a real LUFS/true-peak meter; Unity has no built-in loudness meter, so
measure the exported WAV in an editor or with `ffmpeg -af ebur128` before import.

## 3. Edit and loop

- **Trim to the transient**, not to silence, for one-shots; keep 2–5 ms of
  pre-transient so the attack is not clipped.
- **Seamless loop**: cut at a zero crossing, then crossfade the tail into the head
  (short: 1–5 ms; music/ambience: 20–100 ms **[conv]**) and verify by looping 20+
  times — a click, a level dip or a rhythmic stutter means the seam is wrong.
- **Tail matching** for ambience: the reverb tail must continue into the loop
  start, or the room "resets" audibly every cycle.
- **DC offset / rumble**: high-pass at 20–30 Hz on voice and most SFX **[conv]**.
- **Null test** an edited loop against the original region to prove nothing else
  changed.

## 4. Layering and ducking

Layer in this priority: voice → gameplay-critical SFX → music → ambience → UI.
Every layer above the bottom must earn its bandwidth.

- **Sidechain ducking**: music and ambience drop under dialogue. Typical duck
  4–8 dB, attack 10–50 ms, release 200–500 ms **[conv]**. In Unity this is a
  *Duck Volume* effect on the music group with the voice group as sidechain.
- **Frequency separation**: carve 1–4 kHz out of the music bed instead of just
  lowering it; speech intelligibility lives there.
- **Variation before repetition**: 3–5 variations per frequent SFX, randomised by
  pitch (±5 %) and volume (±2 dB) **[conv]**; in Unity 6 use an *Audio Random
  Container* rather than a code array where the version supports it.
- **Voice budget**: cap simultaneous voices per category; a hundred enemies must
  not mean a hundred AudioSources. Prioritise by distance and importance, and
  cull the rest.

## 5. Naming and structure

```
Audio/
  Music/      mus_<area>_<mood>_<loop|oneshot>_<bpm>.wav
  Ambience/   amb_<place>_<day|night>_loop.wav
  SFX/        sfx_<category>_<source>_<action>_<vv>.wav
  Voice/      vo_<speaker>_<line-id>_<take>.wav
  UI/         ui_<element>_<state>.wav
```

Rules that pay off later: lowercase, ASCII, `_` separators, zero-padded variant
numbers (`_01`…`_05`), no spaces, and one folder per category. Put the generation
prompt and model id in a sidecar `.txt` or a CSV manifest — six months later the
prompt is the only way to regenerate a matching take.

## 6. Unity import settings

| Asset | Load type | Compression | Other |
|---|---|---|---|
| Short SFX (< 1 s), frequent | Decompress On Load | ADPCM or Vorbis low quality | Force To Mono for 3D; Preload Audio Data on |
| Medium SFX / UI | Compressed In Memory | Vorbis, quality tuned by ear | Load In Background off |
| Music, long ambience | Streaming | Vorbis 70–100 | Loop on; Load In Background on |
| Voice lines | Compressed In Memory | Vorbis | Force To Mono; avoid Streaming (latency) |

- Set the **sample rate override** once per platform rather than trusting the file.
- Keep the WAV masters outside `Assets/` (or in a `RawAudio~` folder) so the editor
  does not import them twice.
- Verify the built player, not the editor: compression differences only appear in a
  build.

## 7. Unity mixer routing

```
Master
├── Music      (Duck Volume, sidechain = Voice)
├── Ambience   (low-pass send to a reverb bus)
├── SFX        (limiter)
├── Voice      (priority; no ducking)
└── UI         (bypasses spatialisation)
```

- Expose `Master`, `Music`, `SFX`, `Voice` volume as **exposed parameters** and bind
  them to the settings sliders; store the values.
- Use **snapshots** for states (menu, gameplay, pause, low-health) instead of
  scripting volume changes per object.
- Every slider must reach 0 dB (silence) and the default must be comfortable, not
  maximal.
- Route `AudioSource.outputAudioMixerGroup` explicitly; a source left on Master
  bypasses every setting above.

## 8. Verification checklist

| Gate | Evidence |
|---|---|
| Loudness | measured integrated LUFS and true peak per bus, before and after the mix |
| No clipping | true peak ≤ −1 dBTP on the master |
| Loops | 20× loop in the player, no click, no dip, no stutter |
| Mono compatibility | the mix survives a mono collapse without a level hole |
| Dialogue intelligibility | voice audible with music + SFX at gameplay density |
| 3D behaviour | attenuation curve and spatial blend tested at near/far distances |
| Voice count | worst-case gameplay does not exceed the per-category cap |
| Settings | volume sliders persist across a build relaunch; 0 dB silences |
| Build | verified in a development player, not the editor |
| Provenance | manifest lists model id, parameters and prompt per file |

## 9. Common mistakes

| Mistake | Symptom | Fix |
|---|---|---|
| normalising the master instead of per clip | one loud effect crushes everything | trim and normalise each clip first |
| lowering a fader for a quiet source | noise floor rises with the rest of the bus | normalise the clip, then set the fader |
| loop cut at an arbitrary sample | click every cycle | zero-crossing cut + short crossfade |
| reverb tail truncated at the loop point | room "resets" each cycle | match tail into head, or bake the tail |
| stereo 3D SFX | no panning, wrong direction | Force To Mono |
| streaming for short voice lines | late or dropped trigger | Compressed In Memory |
| every impact on Master | cannot duck, cannot fade, cannot balance | assign a mixer group |
| 20 identical footstep clips | machine-gun sameness | variations + pitch/volume randomisation |
| duplicate tags read aloud in TTS | wrong words in the take | verify tag grammar from the model schema, listen, re-run |
| mixing at editor scale | the build sounds different | verify loudness and compression in the player |

## Handoffs

- Generate or regenerate the source assets: `scenario-audio` and the per-family
  skills in this plugin.
- Lay the finished track over a cut: `scenario-video-assembly` compositor.
- In-engine music states, combat mixers and 3D combat audio:
  `02-unity-expert-6-3/skills/scenario-unity-gameplay`.
- Impact layering and hit feedback timing: `03-game-feel`.
- Frame and voice cost of audio on phones: `02-unity-expert-6-3/skills/scenario-unity-mobile`.
