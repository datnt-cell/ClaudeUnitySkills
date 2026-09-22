---
name: unity-networking
description: Set up Netcode for GameObjects from chat — NetworkObject/NetworkBehaviour prefabs, spawning, lifecycle, ownership, RPCs, NetworkVariables, host/server/client roles, and transport config. Use whenever the user wants multiplayer networking wired up through Editor automation, and always check the source-anchored design rules for pitfalls before writing networked code. Read unity-core first for the REST protocol before calling any skill named here.
---

# Unity Networking

Part of the Unity Skills plugin. **Read the [unity-core](../unity-core/SKILL.md) skill first** for the First Contact Checklist (health check, operating mode, dryRun gate).

## Modules in this skill

| Module | Mode | Description | Doc |
|---|:---:|---|---|
| netcode | Mixed* | Netcode for GameObjects setup, prefabs, lifecycle, host/server/client | [modules/netcode/SKILL.md](modules/netcode/SKILL.md) |
| netcode-design | — | Source-anchored rules: lifecycle/ownership/RPC/variables/spawn/scene/transport/pitfalls (advisory, no REST skills) | [modules/netcode-design/SKILL.md](modules/netcode-design/SKILL.md) |

Mode legend defined in unity-core's [MODULE_INDEX.md](../unity-core/MODULE_INDEX.md).

## Routing

Skills are named `netcode_<verb>` (e.g. `netcode_create_network_prefab`, `netcode_configure_transport`). **Read `netcode-design` before writing any networked script** — Netcode has sharp edges around ownership, RPC targets, and spawn timing that schema alone won't surface; the design module's pitfalls file catalogs the common mistakes with source citations.

Open the matching module doc before calling any skill whose exact parameters you don't already hold — dryRun first per the unity-core protocol. `mutatesScene` writes here are still hidden under a `noSceneAuthoring` surface profile — see unity-core's [SKILL_GUIDE.md](../unity-core/SKILL_GUIDE.md).
