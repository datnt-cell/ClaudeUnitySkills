---
name: unity-xr
description: Set up Unity XR Interaction Toolkit (XRI) and PICO Unity Integration SDK from chat — VR/AR/MR rigs, interactors/interactables, and PICO-specific rendering, interaction, mixed reality, and SecureMR setup. Use when the user is building VR/AR/MR experiences, especially targeting PICO headsets. Read unity-core first for the REST protocol before calling any skill named here.
---

# Unity XR

Part of the Unity Skills plugin. **Read the [unity-core](../unity-core/SKILL.md) skill first** for the First Contact Checklist (health check, operating mode, dryRun gate).

## Modules in this skill

| Module | Mode | Description | Doc |
|---|:---:|---|---|
| xr | FA | XRI setup | [modules/xr/SKILL.md](modules/xr/SKILL.md) (+ [API_REFERENCE.md](modules/xr/API_REFERENCE.md)) |
| pico-design | — | PICO Unity Integration SDK v3.4.0 doc-anchored rules: setup/rendering/interaction/MR/SecureMR/platform/API signatures/version diffs 2.x-3.4/pitfalls (advisory, no REST skills) | [modules/pico-design/SKILL.md](modules/pico-design/SKILL.md) |

Mode legend defined in unity-core's [MODULE_INDEX.md](../unity-core/MODULE_INDEX.md).

## Routing

`xr` provides the generic XRI REST skills (named `xr_<verb>`, e.g. `xr_create_rig`, `xr_add_interactable`). `pico-design` is documentation only — read it for PICO-specific API signatures and version differences (2.x vs 3.4) before writing PICO SDK code, since that SDK isn't exposed as REST skills.

Open the `xr` module doc before calling any skill whose exact parameters you don't already hold — dryRun first per the unity-core protocol.
