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
| [gameobject](../unity-scene-authoring/modules/gameobject/SKILL.md) | FA* | Object create/move/parent | Yes |
| [component](../unity-scene-authoring/modules/component/SKILL.md) | Mixed* | Component add/remove/configure | Yes |
| [material](../unity-rendering/modules/material/SKILL.md) | FA | Material property edits | Yes |
| [light](../unity-rendering/modules/light/SKILL.md) | FA | Light create/configure | Yes |
| [prefab](../unity-scene-authoring/modules/prefab/SKILL.md) | FA | Prefab create/apply/spawn | Yes |
| [asset](../unity-assets-pipeline/modules/asset/SKILL.md) | SA* | Asset refresh/find/info | Yes |
| [batch](../unity-dev-workflow/modules/batch/SKILL.md) | SA | Batch and async jobs | Built-in |
| [ui](../unity-ui/modules/ui/SKILL.md) | FA | UGUI Canvas/UI creation | Yes |
| [uitoolkit](../unity-ui/modules/uitoolkit/SKILL.md) | Mixed* | UXML/USS/UIDocument | No |
| [script](../unity-scripting/modules/script/SKILL.md) | SA* | Script create/read/update | Yes |
| [scene](../unity-scene-authoring/modules/scene/SKILL.md) | SA* | Scene load/save/query | No |
| [editor](../unity-scene-authoring/modules/editor/SKILL.md) | SA* | Play/select/undo/redo/change journal | No |
| [animator](../unity-animation-timeline/modules/animator/SKILL.md) | FA | Animator controllers | No |
| [shader](../unity-rendering/modules/shader/SKILL.md) | Mixed* | Shader create/list | No |
| [shadergraph](../unity-rendering/modules/shadergraph/SKILL.md) | Mixed* | Shader Graph create/inspect/blackboard edit/constrained node editing | No |
| [graphics](../unity-rendering/modules/graphics/SKILL.md) | Mixed | GraphicsSettings / QualitySettings / SRP assets | No |
| [volume](../unity-rendering/modules/volume/SKILL.md) | Mixed* | Volume / VolumeProfile / VolumeComponent | No |
| [postprocess](../unity-rendering/modules/postprocess/SKILL.md) | FA* | Modern URP/HDRP post-processing | No |
| [urp](../unity-rendering/modules/urp/SKILL.md) | Mixed* | URP asset / renderer / renderer features | No |
| [decal](../unity-rendering/modules/decal/SKILL.md) | Mixed* | URP Decal Projector workflow | Yes |
| [console](../unity-dev-workflow/modules/console/SKILL.md) | SA | Log capture/filter | No |
| [validation](../unity-assets-pipeline/modules/validation/SKILL.md) | SA* | Broken reference checks | No |
| [importer](../unity-assets-pipeline/modules/importer/SKILL.md) | Mixed | Texture/audio/model import | Yes |
| [cinemachine](../unity-animation-timeline/modules/cinemachine/SKILL.md) | FA* | VCam operations | No |
| [probuilder](../unity-physics-navigation/modules/probuilder/SKILL.md) | FA* | ProBuilder mesh edits | No |
| [xr](../unity-xr/modules/xr/SKILL.md) | FA | XRI setup | No |
| [terrain](../unity-physics-navigation/modules/terrain/SKILL.md) | FA | Terrain create/paint | No |
| [physics](../unity-physics-navigation/modules/physics/SKILL.md) | Mixed | Raycast/overlap/gravity | No |
| [navmesh](../unity-physics-navigation/modules/navmesh/SKILL.md) | Mixed* | NavMesh bake/query | No |
| [timeline](../unity-animation-timeline/modules/timeline/SKILL.md) | FA* | Timeline tracks/clips | No |
| [workflow](../unity-dev-workflow/modules/workflow/SKILL.md) | SA* | Task snapshots/undo, batch orchestration | No |
| [cleaner](../unity-assets-pipeline/modules/cleaner/SKILL.md) | SA* | Unused/duplicate assets | No |
| [smart](../unity-dev-workflow/modules/smart/SKILL.md) | FA* | Query/layout/auto-bind | No |
| [perception](../unity-dev-workflow/modules/perception/SKILL.md) | SA | Scene/project analysis | No |
| [camera](../unity-animation-timeline/modules/camera/SKILL.md) | FA | Scene View camera | No |
| [event](../unity-dev-workflow/modules/event/SKILL.md) | Mixed* | UnityEvent wiring | No |
| [package](../unity-assets-pipeline/modules/package/SKILL.md) | Mixed* | UPM install/query | No |
| [project](../unity-dev-workflow/modules/project/SKILL.md) | SA* | Project info/settings | No |
| [profiler](../unity-dev-workflow/modules/profiler/SKILL.md) | SA | Perf statistics | No |
| [optimization](../unity-assets-pipeline/modules/optimization/SKILL.md) | Mixed | Asset optimization | No |
| [sample](../unity-dev-workflow/modules/sample/SKILL.md) | Mixed* | Demo/test skills | No |
| [debug](../unity-dev-workflow/modules/debug/SKILL.md) | SA* | Compile/system diagnostics | No |
| [test](../unity-dev-workflow/modules/test/SKILL.md) | Mixed* | Unity Test Runner | No |
| [bookmark](../unity-dev-workflow/modules/bookmark/SKILL.md) | SA | Scene View bookmarks | No |
| [history](../unity-dev-workflow/modules/history/SKILL.md) | SA | Undo/redo history | No |
| [scriptableobject](../unity-scripting/modules/scriptableobject/SKILL.md) | Mixed* | ScriptableObject assets | No |
| [netcode](../unity-networking/modules/netcode/SKILL.md) | Mixed* | Netcode for GameObjects setup, prefabs, lifecycle, host/server/client | Yes |
| [addressables](../unity-addressables-yooasset/modules/addressables/SKILL.md) | Mixed* | Addressables authoring: group CRUD, asset entry assignment, profile switching, content build (com.unity.addressables, reflection-based) | No |
| [yooasset](../unity-addressables-yooasset/modules/yooasset/SKILL.md) | Mixed* | YooAsset hot-update: build bundles, Collector CRUD, BuildReport asset/dependency analysis, PlayMode runtime validation, Reporter/Debugger/AssetArtScanner tools | Yes |
| [dotween](../unity-tweening/modules/dotween/SKILL.md) | Mixed* | DOTween Pro DOTweenAnimation editor-time configuration (add/batch/stagger/tune) | Yes |
| [primetween](../unity-tweening/modules/primetween/SKILL.md) | Mixed* | PrimeTween Free inspection, factory discovery, and runtime tween/sequence script generation | No |
| [behavior](../unity-editor-tools/modules/behavior/SKILL.md) | Mixed | Unity Behavior graph assets, agents, blackboard variables (com.unity.behavior, reflection-based) | Yes |
| [hybridclr](../unity-addressables-yooasset/modules/hybridclr/SKILL.md) | Mixed* | HybridCLR hot-update settings, codegen, DLL compile/copy pipeline (com.code-philosophy.hybridclr, reflection-based) | Yes |
| [qframework](../unity-qframework/modules/qframework/SKILL.md) | Mixed* | QFramework editor automation: architecture-layer codegen, ViewController/UIKit panel codegen, ResKit AssetBundle mark/build, architecture scan, API doc query (no UPM package, reflection-based) | Yes |

## Advisory Design Modules

Documentation only — these modules define no REST skills.

| Module | Description |
|--------|-------------|
| [project-scout](../unity-architecture-advisory/modules/project-scout/SKILL.md) | Inspect existing project |
| [architecture](../unity-architecture-advisory/modules/architecture/SKILL.md) | Plan system boundaries |
| [adr](../unity-architecture-advisory/modules/adr/SKILL.md) | Record tradeoffs |
| [performance](../unity-architecture-advisory/modules/performance/SKILL.md) | Review hot paths |
| [asmdef](../unity-scripting/modules/asmdef/SKILL.md) | Plan asmdef deps |
| [blueprints](../unity-architecture-advisory/modules/blueprints/SKILL.md) | Small-game blueprints |
| [script-roles](../unity-scripting/modules/script-roles/SKILL.md) | Assign class roles |
| [scene-contracts](../unity-architecture-advisory/modules/scene-contracts/SKILL.md) | Define scene wiring |
| [testability](../unity-scripting/modules/testability/SKILL.md) | Extract testable logic |
| [patterns](../unity-scripting/modules/patterns/SKILL.md) | Choose patterns |
| [async](../unity-scripting/modules/async/SKILL.md) | Choose async model |
| [inspector](../unity-architecture-advisory/modules/inspector/SKILL.md) | Design authoring UX |
| [scriptdesign](../unity-scripting/modules/scriptdesign/SKILL.md) | Review script structure |
| [netcode-design](../unity-networking/modules/netcode-design/SKILL.md) | Netcode source-anchored rules (lifecycle/ownership/RPC/variables/spawn/scene/transport/pitfalls) |
| [yooasset-design](../unity-addressables-yooasset/modules/yooasset-design/SKILL.md) | YooAsset v2.3.18 source-anchored rules (init/default-package shortcuts/playmode/handles/loading/update/filesystem/build/pitfalls) |
| [addressables-design](../unity-addressables-yooasset/modules/addressables-design/SKILL.md) | Addressables dual-version (1.22.3 Unity 2022 / 2.9.1 Unity 6) source-anchored rules (init/handles/loading/scene/update/download/assetref/pitfalls) with migration table |
| [unitask-design](../unity-tweening/modules/unitask-design/SKILL.md) | UniTask 2.5.10 source-anchored rules (basics/playerloop/cancellation/composition/conversion/asyncenumerable/triggers/pitfalls) |
| [dotween-design](../unity-tweening/modules/dotween-design/SKILL.md) | DOTween 1.3.015 source-anchored rules (basics/tween/sequence/shortcuts/ease/lifetime/integration/pitfalls) |
| [primetween-design](../unity-tweening/modules/primetween-design/SKILL.md) | PrimeTween 1.4.6 source-anchored rules (factories/handles/sequences/cycles/callbacks/lifetime/integration) |
| [shadergraph-design](../unity-rendering/modules/shadergraph-design/SKILL.md) | ShaderGraph dual-version source-anchored rules (versions/node subset/recipes/pitfalls/review) |
| [pico-design](../unity-xr/modules/pico-design/SKILL.md) | PICO Unity Integration SDK v3.4.0 doc-anchored rules (setup/rendering/interaction/MR/SecureMR/platform/API signatures/version diffs 2.x-3.4/pitfalls) |
| [qframework-design](../unity-qframework/modules/qframework-design/SKILL.md) | QFramework v1.0.257 source-anchored rules (layers/CQRS/BindableProperty/event tools/CodeGenKit+UIKit/ResKit/ActionKit+SingletonKit/data kits/pitfalls) |
| [yaml-editing](../unity-editor-tools/modules/yaml-editing/SKILL.md) | Safe hand-edit rules for serialized YAML (.unity/.prefab/.asset/.meta/ProjectSettings) when REST cannot reach — reference/fileID repair, .meta/GUID safety, ProjectSettings patch, merge conflict |
| [unity-cli](../unity-editor-tools/modules/unity-cli/SKILL.md) | Experimental Unity CLI on the bound project (opt-in via `Library/UnitySkills/cli_config.json`) — cold start with the Editor closed, headless test/run/build, exit codes, JSON/NDJSON contract, hard DO-NOT list |
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
