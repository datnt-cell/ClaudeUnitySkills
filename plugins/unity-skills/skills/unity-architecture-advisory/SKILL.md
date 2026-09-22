---
name: unity-architecture-advisory
description: Design-time advisory guidance for Unity projects — inspecting an existing project, planning system/asmdef boundaries, recording architecture decision records (ADRs), reviewing performance hot paths, sketching small-game blueprints, defining scene wiring contracts, and designing authoring UX. Use when the user wants planning, review, or documentation of a Unity project's architecture rather than direct Editor automation — these modules define no REST skills. Read unity-core first if the task also needs Editor automation.
---

# Unity Architecture Advisory

Part of the Unity Skills plugin. These modules are documentation-only — they define no REST skills and don't require the unity-core First Contact Checklist. If the task also needs Editor automation (creating files, querying the scene), read [unity-core](../unity-core/SKILL.md) too.

## Modules in this skill

| Module | Description | Doc |
|---|---|---|
| project-scout | Inspect existing project | [modules/project-scout/SKILL.md](modules/project-scout/SKILL.md) |
| architecture | Plan system boundaries | [modules/architecture/SKILL.md](modules/architecture/SKILL.md) |
| adr | Record tradeoffs | [modules/adr/SKILL.md](modules/adr/SKILL.md) |
| performance | Review hot paths | [modules/performance/SKILL.md](modules/performance/SKILL.md) |
| blueprints | Small-game blueprints | [modules/blueprints/SKILL.md](modules/blueprints/SKILL.md) |
| scene-contracts | Define scene wiring | [modules/scene-contracts/SKILL.md](modules/scene-contracts/SKILL.md) |
| inspector | Design authoring UX | [modules/inspector/SKILL.md](modules/inspector/SKILL.md) |

## Routing

Start with `project-scout` when picking up an unfamiliar project — it establishes the baseline the other advisories build on. Use `architecture` and `adr` together when a design decision needs both a plan and a recorded rationale. `performance` and `scene-contracts` are useful before large automation passes from other unity-* skills, to avoid baking in a design that needs rework.

None of these modules call REST endpoints — they're read/write-to-docs guidance you produce directly, not automation you dispatch through the Unity Editor server.
