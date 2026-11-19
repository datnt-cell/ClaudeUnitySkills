---
name: unity-skills-index
description: Index of all Unity Skills modules with per-module mode labels (SA/FA/Mixed). Use to find which module covers a task before loading its doc.
---

## Triggers
- Browsing the module catalog
- Finding which module handles a task
- Checking mode requirements
- 浏览模块目录、查找某事由哪个模块处理、确认模式要求

# Unity Skills - Module Index

Module docs. Start with [./SKILL.md](./SKILL.md) for mode switching and schema-first rules.

> **Multi-instance**: For version-specific projects, call `unity_skills.set_unity_version(...)` first.
> **Schema-first**: Use `GET /skills/schema` or `unity_skills.get_skill_schema()` for exact signatures. Load module docs for workflow guidance and guardrails.

## Modules

> **Mode legend** (v1.9.0+, caller-facing — describes what the caller can do, not the C# attribute):
> - `SA` — module skills mostly run directly in **all three modes** (Approval / Auto / Bypass) without a grant.
> - `FA` — module skills mostly require **user grant** under Approval (single-shot one-step execution); under Auto / Bypass they run directly with audit only.
> - `Mixed` — module is split between SA and FA; check per-skill `mode` before calling (`GET /skills?full=1`, or the scoped `GET /skills/schema?category=<Category>` — bare `GET /skills` is the brief directory and carries no `mode`).
> - Suffix `*` — module contains auto-forbidden skills (Delete / Play Mode / Domain Reload / high-risk). These return `MODE_FORBIDDEN` under Approval and Auto; only **Bypass** runs them, **or** the user can permanently allow them via the Allowlist. Never attempt grant for them.
>
> Labels are guidance only; the per-skill `mode` field (`GET /skills?full=1` / `GET /skills/schema?category=<Category>`) is authoritative.

| Module | Mode | Description | Batch Support |
|--------|:----:|-------------|---------------|
| gameobject (unity-scene-authoring plugin) | FA* | Object create/move/parent | Yes |
| component (unity-scene-authoring plugin) | Mixed* | Component add/remove/configure | Yes |
| material (unity-rendering plugin) | FA | Material property edits | Yes |
| light (unity-rendering plugin) | FA | Light create/configure | Yes |
| prefab (unity-scene-authoring plugin) | FA | Prefab create/apply/spawn | Yes |
| asset (unity-assets-pipeline plugin) | SA* | Asset refresh/find/info | Yes |
| batch (unity-dev-workflow plugin) | SA | Batch and async jobs | Built-in |
| ui (unity-ui plugin) | FA | UGUI Canvas/UI creation | Yes |
| uitoolkit (unity-ui plugin) | Mixed* | UXML/USS/UIDocument | No |
| script (unity-scripting plugin) | SA* | Script create/read/update | Yes |
| scene (unity-scene-authoring plugin) | SA* | Scene load/save/query | No |
| editor (unity-scene-authoring plugin) | SA* | Play/select/undo/redo/change journal | No |
| animator (unity-animation-timeline plugin) | FA | Animator controllers | No |
| shader (unity-rendering plugin) | Mixed* | Shader create/list | No |
| shadergraph (unity-rendering plugin) | Mixed* | Shader Graph create/inspect/blackboard edit/constrained node editing | No |
| graphics (unity-rendering plugin) | Mixed | GraphicsSettings / QualitySettings / SRP assets | No |
| volume (unity-rendering plugin) | Mixed* | Volume / VolumeProfile / VolumeComponent | No |
| postprocess (unity-rendering plugin) | FA* | Modern URP/HDRP post-processing | No |
| urp (unity-rendering plugin) | Mixed* | URP asset / renderer / renderer features | No |
| decal (unity-rendering plugin) | Mixed* | URP Decal Projector workflow | Yes |
| console (unity-dev-workflow plugin) | SA | Log capture/filter | No |
| validation (unity-assets-pipeline plugin) | SA* | Broken reference checks | No |
| importer (unity-assets-pipeline plugin) | Mixed | Texture/audio/model import | Yes |
| cinemachine (unity-animation-timeline plugin) | FA* | VCam operations | No |
| probuilder (unity-physics-navigation plugin) | FA* | ProBuilder mesh edits | No |
| terrain (unity-physics-navigation plugin) | FA | Terrain create/paint | No |
| physics (unity-physics-navigation plugin) | Mixed | Raycast/overlap/gravity | No |
| navmesh (unity-physics-navigation plugin) | Mixed* | NavMesh bake/query | No |
| timeline (unity-animation-timeline plugin) | FA* | Timeline tracks/clips | No |
| workflow (unity-dev-workflow plugin) | SA* | Task snapshots/undo, batch orchestration | No |
| cleaner (unity-assets-pipeline plugin) | SA* | Unused/duplicate assets | No |
| smart (unity-dev-workflow plugin) | FA* | Query/layout/auto-bind | No |
| perception (unity-dev-workflow plugin) | SA | Scene/project analysis | No |
| camera (unity-animation-timeline plugin) | FA | Scene View camera | No |
| event (unity-dev-workflow plugin) | Mixed* | UnityEvent wiring | No |
| package (unity-assets-pipeline plugin) | Mixed* | UPM install/query | No |
| project (unity-dev-workflow plugin) | SA* | Project info/settings | No |
| profiler (unity-dev-workflow plugin) | SA | Perf statistics | No |
| optimization (unity-assets-pipeline plugin) | Mixed | Asset optimization | No |
| sample (unity-dev-workflow plugin) | Mixed* | Demo/test skills | No |
| debug (unity-dev-workflow plugin) | SA* | Compile/system diagnostics | No |
| test (unity-dev-workflow plugin) | Mixed* | Unity Test Runner | No |
| bookmark (unity-dev-workflow plugin) | SA | Scene View bookmarks | No |
| history (unity-dev-workflow plugin) | SA | Undo/redo history | No |
| scriptableobject (unity-scripting plugin) | Mixed* | ScriptableObject assets | No |
| addressables (unity-addressables-yooasset plugin) | Mixed* | Addressables authoring: group CRUD, asset entry assignment, profile switching, content build (com.unity.addressables, reflection-based) | No |
| yooasset (unity-addressables-yooasset plugin) | Mixed* | YooAsset hot-update: build bundles, Collector CRUD, BuildReport asset/dependency analysis, PlayMode runtime validation, Reporter/Debugger/AssetArtScanner tools | Yes |
| dotween (unity-tweening plugin) | Mixed* | DOTween Pro DOTweenAnimation editor-time configuration (add/batch/stagger/tune) | Yes |
| primetween (unity-tweening plugin) | Mixed* | PrimeTween Free inspection, factory discovery, and runtime tween/sequence script generation | No |
| behavior (unity-editor-tools plugin) | Mixed | Unity Behavior graph assets, agents, blackboard variables (com.unity.behavior, reflection-based) | Yes |
| hybridclr (unity-addressables-yooasset plugin) | Mixed* | HybridCLR hot-update settings, codegen, DLL compile/copy pipeline (com.code-philosophy.hybridclr, reflection-based) | Yes |

## Advisory Design Modules

Documentation only — these modules define no REST skills.

| Module | Description |
|--------|-------------|
| project-scout (unity-architecture-advisory plugin) | Inspect existing project |
| architecture (unity-architecture-advisory plugin) | Plan system boundaries |
| adr (unity-architecture-advisory plugin) | Record tradeoffs |
| performance (unity-architecture-advisory plugin) | Review hot paths |
| asmdef (unity-scripting plugin) | Plan asmdef deps |
| blueprints (unity-architecture-advisory plugin) | Small-game blueprints |
| script-roles (unity-scripting plugin) | Assign class roles |
| scene-contracts (unity-architecture-advisory plugin) | Define scene wiring |
| testability (unity-scripting plugin) | Extract testable logic |
| patterns (unity-scripting plugin) | Choose patterns |
| async (unity-scripting plugin) | Choose async model |
| inspector (unity-architecture-advisory plugin) | Design authoring UX |
| scriptdesign (unity-scripting plugin) | Review script structure |
| yooasset-design (unity-addressables-yooasset plugin) | YooAsset v2.3.18 source-anchored rules (init/default-package shortcuts/playmode/handles/loading/update/filesystem/build/pitfalls) |
| addressables-design (unity-addressables-yooasset plugin) | Addressables dual-version (1.22.3 Unity 2022 / 2.9.1 Unity 6) source-anchored rules (init/handles/loading/scene/update/download/assetref/pitfalls) with migration table |
| unitask-design (unity-tweening plugin) | UniTask 2.5.10 source-anchored rules (basics/playerloop/cancellation/composition/conversion/asyncenumerable/triggers/pitfalls) |
| dotween-design (unity-tweening plugin) | DOTween 1.3.015 source-anchored rules (basics/tween/sequence/shortcuts/ease/lifetime/integration/pitfalls) |
| primetween-design (unity-tweening plugin) | PrimeTween 1.4.6 source-anchored rules (factories/handles/sequences/cycles/callbacks/lifetime/integration) |
| shadergraph-design (unity-rendering plugin) | ShaderGraph dual-version source-anchored rules (versions/node subset/recipes/pitfalls/review) |
| yaml-editing (unity-editor-tools plugin) | Safe hand-edit rules for serialized YAML (.unity/.prefab/.asset/.meta/ProjectSettings) when REST cannot reach — reference/fileID repair, .meta/GUID safety, ProjectSettings patch, merge conflict |
| unity-cli (unity-editor-tools plugin) | Experimental Unity CLI on the bound project (opt-in via `Library/UnitySkills/cli_config.json`) — cold start with the Editor closed, headless test/run/build, exit codes, JSON/NDJSON contract, hard DO-NOT list |
| [manual-gameobject](./manual/manual-gameobject/SKILL.md) | Manually create GameObjects, organize the Hierarchy, and adjust Transforms using Unity Editor UI |
| [manual-component](./manual/manual-component/SKILL.md) | Manually add, configure, reorder, and copy components on GameObjects using Unity Editor UI |
| [manual-material](./manual/manual-material/SKILL.md) | Manually create and edit Materials and assign them to objects using Unity Editor UI |
| [manual-scene](./manual/manual-scene/SKILL.md) | Manually navigate, save, and manage scenes using Unity Editor UI |

## Batch-First Rule

When a task touches `2+` objects in Auto / Bypass mode (or after a successful grant under Approval), prefer `*_batch` skills over repeated single-item calls.

## Skill Naming Convention

Skills follow `<module>_<action>` or `<module>_<action>_batch`.
Use schema to verify the exact prefix list.
Special: `scene_analyze`, `hierarchy_describe`, `project_stack_detect` → `perception`; `job_*` → `batch`.
If a skill name does not match a valid prefix or a schema result, do not invent it.
