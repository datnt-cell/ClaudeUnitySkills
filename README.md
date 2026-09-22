# ClaudeUnitySkills

A Claude Code plugin marketplace for automating the Unity Editor from chat. Each domain (rendering, UI, scripting, assets, ...) is a **separate plugin** — add the marketplace once, then install only what you need.

## Install

```
/plugin marketplace add vantv/ClaudeUnitySkills
/plugin install unity-scene-authoring@claude-unity-skills
/plugin install unity-rendering@claude-unity-skills
```

(Replace `vantv/ClaudeUnitySkills` with this repo's actual remote path if different, or use a local path/URL — see [Claude Code plugin marketplace docs](https://code.claude.com/docs/en/plugin-marketplaces).)

## Available plugins

| Plugin | Covers |
|---|---|
| `unity-scene-authoring` | GameObject, Component, Prefab, Scene, Editor control |
| `unity-rendering` | Material, Light, Shader, Shader Graph, Graphics/Quality settings, Volume, post-processing, URP, Decal |
| `unity-ui` | UGUI, UI Toolkit |
| `unity-scripting` | Script, ScriptableObject, plus script-role/design/asmdef/testability/pattern/async advisories |
| `unity-assets-pipeline` | Asset, Importer, Cleaner, Optimization, Package, Validation |
| `unity-animation-timeline` | Animator, Timeline, Cinemachine, Camera |
| `unity-physics-navigation` | Physics, NavMesh, Terrain, ProBuilder |
| `unity-addressables-yooasset` | Addressables, YooAsset, HybridCLR |
| `unity-tweening` | DOTween, PrimeTween, UniTask design advisory |
| `unity-dev-workflow` | Batch/async jobs, workflow snapshots, debug/console, profiler, history, bookmarks, Test Runner, sample tools, smart tools, perception, project settings, UnityEvent |
| `unity-architecture-advisory` | Project-scout, architecture, ADR, performance review, blueprints, scene contracts, inspector UX — advisory only |
| `unity-editor-tools` | YAML hand-editing fallback, experimental Unity CLI, Behavior graphs |
| `unity-core` | Optional deep-dive: full protocol detail, error-code catalog, manual guide-mode docs. Every plugin above is self-contained and works without it. |

Every plugin except `unity-core` embeds its own self-contained summary of the shared REST protocol (server discovery, operating mode, dryRun gate, batch execution, error codes) — install only the domain plugins you need.

## Requirements

Each plugin talks to the `UnitySkills` Editor package's local REST server (Unity 2022.3+/6000.x, `localhost:8090`–`8100`). This marketplace ships documentation and routing only — it does not bundle the Unity package itself.

## What's inside

- `plugins/<name>/` — one directory per plugin, each with its own `.claude-plugin/plugin.json` and `skills/<name>/SKILL.md`.
- [`.claude-plugin/marketplace.json`](.claude-plugin/marketplace.json) — the marketplace manifest listing all plugins.
