---
name: unity-ui
description: Build Unity UI from chat — UGUI Canvas/widget creation and UI Toolkit (UXML/USS/UIDocument) editing through Editor automation. Use when the user wants menus, HUDs, buttons, panels, or other UI built or edited in the Unity Editor. Read unity-core first for the REST protocol before calling any skill named here.
---

# Unity UI

Part of the Unity Skills plugin.

## Unity Skills protocol (quick reference)

This plugin talks to the Unity Editor through a local REST server. Before the first skill call in a session:

1. **`GET /health`** — discover the server (it binds a port in `8090`–`8100`). Read `currentMode` (`"approval"` / `"auto"` / `"bypass"`) and `surfaceProfile` (`full` / `guide` / `noSceneAuthoring`) — confirm `projectName` matches the project you're editing.
2. Under `approval` mode, the first write to a `FullAuto` skill returns `MODE_RESTRICTED` and needs a user grant; under `auto`/`bypass` writes execute directly.

**dryRun gate**: before executing any skill whose exact parameters you don't already hold, call `POST /skill/<name>?mode=dryRun` and iterate until the response says `valid:true` — never guess parameters from a skill name alone. Then execute the same call without `?mode=dryRun`.

**Batch execution**: when touching 2+ objects, prefer `POST /skills/batch` (`{"steps":[{"skill","args"}],"continueOnError":false}`, up to 50 steps per call, also supports `?mode=dryRun`) over repeated single-item calls.

**Error codes**:
| Code | Meaning |
|---|---|
| `MODE_RESTRICTED` / `MODE_FORBIDDEN` | Needs a user grant, or needs Bypass/Allowlist. |
| `SURFACE_EXCLUDED` | Hidden by the current `surfaceProfile` — a configuration boundary, not a failure; don't retry or route around it. |
| `MISSING_PARAM` / `TARGET_NOT_FOUND` | Bad or unresolvable arguments — dryRun for the schema, locate the target first. |

For the full protocol detail, error code catalog, and guide-mode `manual-*` docs, optionally install the `unity-core` plugin — it's not required, this summary covers what you need for this skill.

## Modules in this skill

| Module | Mode | Description | Doc |
|---|:---:|---|---|
| ui | FA | UGUI Canvas/UI creation | [modules/ui/SKILL.md](modules/ui/SKILL.md) (+ [UI_REFERENCE.md](modules/ui/UI_REFERENCE.md)) |
| uitoolkit | Mixed* | UXML/USS/UIDocument | [modules/uitoolkit/SKILL.md](modules/uitoolkit/SKILL.md) (+ [USS_REFERENCE.md](modules/uitoolkit/USS_REFERENCE.md)) |

Mode legend: SA = runs without grant in all modes, FA = needs grant under Approval, Mixed = check per-skill, `*` = contains auto-forbidden skills.

## Routing

Skills are named `<module>_<verb>` (e.g. `ui_create_button`, `uitoolkit_set_uxml`). UGUI (`ui`) and UI Toolkit (`uitoolkit`) are different Unity UI systems — check which one the target project actually uses (UI Toolkit for Editor windows/modern runtime UI, UGUI for legacy Canvas-based UI) before picking a module. The reference docs for each module cover Inspector field names and USS property specifics that schema omits.

Open the matching module doc before calling any skill whose exact parameters you don't already hold — dryRun first. For simple one-off UI tweaks, guidance mode (talking the user through Inspector/menu steps instead of calling REST skills) is covered in more depth by the optional unity-core plugin.
