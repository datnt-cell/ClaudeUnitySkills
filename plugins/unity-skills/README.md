# Unity Skills

A Claude Code plugin that automates the Unity Editor through a local REST API (the `UnitySkills` Editor package) — create and edit scripts, build scenes and prefabs, manage assets/materials/lighting/rendering, run tests, and drive hundreds of Editor operations, grouped into domain skills so Claude only loads what a given task needs.

## Requirements

- Unity Editor 2022.3+ / 6000.x with the `UnitySkills` package installed (runs a local REST server on `localhost:8090`–`8100`)
- Any HTTP client (curl, or your own thin Python/Node wrapper) to call the REST server

This plugin ships documentation and routing only — it does not bundle the Unity package or a REST client.

## Structure

Every skill in this plugin depends on **[`unity-core`](skills/unity-core/SKILL.md)**, which holds the shared protocol: server discovery (`GET /health`), operating mode (approval/auto/bypass), the schema-layer picker, batch execution, the dryRun gate, surface profiles, and error codes. Read it first, before the first REST call in a session.

The remaining skills each own a slice of the 82 Unity modules (54 with REST skills, 28 advisory-only):

| Skill | Covers |
|---|---|
| [unity-scene-authoring](skills/unity-scene-authoring/SKILL.md) | GameObject, Component, Prefab, Scene, Editor control |
| [unity-rendering](skills/unity-rendering/SKILL.md) | Material, Light, Shader, Shader Graph, Graphics/Quality settings, Volume, post-processing, URP, Decal |
| [unity-ui](skills/unity-ui/SKILL.md) | UGUI, UI Toolkit |
| [unity-scripting](skills/unity-scripting/SKILL.md) | Script, ScriptableObject, plus script-role/design/asmdef/testability/pattern/async advisories |
| [unity-assets-pipeline](skills/unity-assets-pipeline/SKILL.md) | Asset, Importer, Cleaner, Optimization, Package, Validation |
| [unity-animation-timeline](skills/unity-animation-timeline/SKILL.md) | Animator, Timeline, Cinemachine, Camera |
| [unity-physics-navigation](skills/unity-physics-navigation/SKILL.md) | Physics, NavMesh, Terrain, ProBuilder |
| [unity-networking](skills/unity-networking/SKILL.md) | Netcode for GameObjects + design advisory |
| [unity-addressables-yooasset](skills/unity-addressables-yooasset/SKILL.md) | Addressables, YooAsset, HybridCLR |
| [unity-tweening](skills/unity-tweening/SKILL.md) | DOTween, PrimeTween, UniTask design advisory |
| [unity-qframework](skills/unity-qframework/SKILL.md) | QFramework editor automation + design advisory |
| [unity-xr](skills/unity-xr/SKILL.md) | XR Interaction Toolkit, PICO SDK design advisory |
| [unity-dev-workflow](skills/unity-dev-workflow/SKILL.md) | Batch/async jobs, workflow snapshots, debug/console, profiler, history, bookmarks, Test Runner, sample tools, smart tools, perception, project settings, UnityEvent |
| [unity-architecture-advisory](skills/unity-architecture-advisory/SKILL.md) | Project-scout, architecture, ADR, performance review, blueprints, scene contracts, inspector UX — advisory only, no REST calls |
| [unity-editor-tools](skills/unity-editor-tools/SKILL.md) | YAML hand-editing fallback, experimental Unity CLI, Behavior graphs |

Each skill's `modules/` subfolder holds the original per-module docs unchanged; each skill's `SKILL.md` is a router that names its modules, their REST mode (SA/FA/Mixed), and when to reach for each one.

## Install

From within Claude Code, add this repository as a marketplace and install the plugin:

```
/plugin marketplace add <this-repo-url-or-path>
/plugin install unity-skills@claude-unity-skills
```

Or for local development, point Claude Code at the plugin directory directly:

```
claude --plugin-dir ./plugins/unity-skills
```
