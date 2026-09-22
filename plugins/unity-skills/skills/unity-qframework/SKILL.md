---
name: unity-qframework
description: Automate QFramework editor tooling in Unity from chat — architecture-layer codegen, ViewController/UIKit panel codegen, ResKit AssetBundle mark/build, architecture scanning, and API doc queries. Use when the target project uses QFramework (CQRS-style architecture, BindableProperty, ActionKit/SingletonKit) and the user wants QFramework-specific scaffolding or automation. Read unity-core first for the REST protocol before calling any skill named here.
---

# Unity QFramework

Part of the Unity Skills plugin. **Read the [unity-core](../unity-core/SKILL.md) skill first** for the First Contact Checklist (health check, operating mode, dryRun gate).

## Modules in this skill

| Module | Mode | Description | Doc |
|---|:---:|---|---|
| qframework | Mixed* | Architecture-layer codegen, ViewController/UIKit panel codegen, ResKit AssetBundle mark/build, architecture scan, API doc query (no UPM package, reflection-based) | [modules/qframework/SKILL.md](modules/qframework/SKILL.md) |
| qframework-design | — | QFramework v1.0.257 source-anchored rules: layers/CQRS/BindableProperty/event tools/CodeGenKit+UIKit/ResKit/ActionKit+SingletonKit/data kits/pitfalls (advisory, no REST skills) | [modules/qframework-design/SKILL.md](modules/qframework-design/SKILL.md) |

Mode legend defined in unity-core's [MODULE_INDEX.md](../unity-core/MODULE_INDEX.md).

## Routing

Skills are named `qframework_<verb>` (e.g. `qframework_generate_architecture`, `qframework_scan`). QFramework has no UPM package — its reflection-based skills only work if the framework's source is already present in the project; confirm with `qframework_scan` before assuming any of the other skills will resolve types. **Read `qframework-design` before generating architecture code** — it's source-anchored to v1.0.257 and documents layer boundaries (Model/System/Utility) and the CQRS command/query split that generated code must respect.

Open the matching module doc before calling any skill whose exact parameters you don't already hold — dryRun first per the unity-core protocol.
