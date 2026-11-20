# Design checklists (advisory, version-agnostic)

Short decision frameworks for planning and reviewing Unity C# architecture. Unlike the rest of this
skill they are **not measured on 6.3** — they are review shapes and output formats. Use them to frame
the plan; prove the result with the procedures in [`../procedures.md`](../procedures.md).

| Need | Read | Produces |
|---|---|---|
| Picking up an unfamiliar project | [project-scout.md](project-scout.md) | baseline, conventions to keep, constraints |
| Module boundaries, startup order, Update guards | [architecture-plan.md](architecture-plan.md) | project tier, 3–7 modules, bootstrap plan |
| Starting a small game / vertical slice | [blueprints.md](blueprints.md) | core loop, scenes, initial script list |
| Which class is MonoBehaviour / SO / plain C# | [script-roles.md](script-roles.md) | role per script |
| Reviewing generated or existing scripts | [script-review.md](script-review.md) | keep / simplify / refactor; field lifecycle table |
| Choosing a pattern (events, SO, pool, FSM, services) | [patterns.md](patterns.md) | 1–3 patterns + Decision Lab comparison |
| Update vs coroutine vs Awaitable vs UniTask | [async-model.md](async-model.md) | scheduling model + lifecycle owner |
| Whether and how to split asmdefs | [asmdef.md](asmdef.md) | assemblies, dependency direction, migration |
| Moving logic out for tests | [testability.md](testability.md) | seams, EditMode / PlayMode candidates |
| Required scene objects and wiring | [scene-contracts.md](scene-contracts.md) | scene contract, validation rules |
| Inspector authoring UX | [inspector.md](inspector.md) | field exposure, attributes, validation |
| Recording a decision | [adr.md](adr.md) | short ADR with revisit triggers |

Code-level performance red flags (Update sprawl, hidden allocations, logging in release) live with
the performance skill: `scenario-unity-performance` → `references/code-red-flags.md`.

To apply the result in the open Editor (create scripts, ScriptableObjects, wire components), hand off
to plugin 01: `unity-scripting`, `unity-scene-authoring`.
