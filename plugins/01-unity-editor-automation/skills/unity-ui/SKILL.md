---
name: unity-ui
description: Build Unity UI from chat — UGUI Canvas/widget creation and UI Toolkit (UXML/USS/UIDocument) editing through Editor automation. Use when the user wants menus, HUDs, buttons, panels, or other UI built or edited in the Unity Editor. Not for choosing uGUI vs UI Toolkit or fixing scaling, layout, sort order or missing glyphs — that is scenario-unity-ui (plugin 02). Read unity-core first for the REST protocol before calling any skill named here.
---

# Unity UI

## Protocol

Read [`unity-core`](../unity-core/SKILL.md) before the first REST call of the session (health check + `projectName` match, operating mode, schema layers, error codes). Rules that hold for every module below:

- **Never guess parameters.** `POST /skill/<name>?mode=dryRun` until `valid:true`, then run the same call for real. Module docs hold guardrails and traps; exact signatures come from `GET /skills/recommend?includeSchema=true` or `/skills/schema`.
- **2+ objects → `POST /skills/batch`** (≤50 steps, also takes `?mode=dryRun`).
- `MODE_RESTRICTED` → ask the user for a grant. `SURFACE_EXCLUDED` is a configuration boundary: don't retry or route around it — switch to guide mode ([`unity-core/SKILL_GUIDE.md`](../unity-core/SKILL_GUIDE.md)).

## Modules in this skill

| Module | Mode | Description | Doc |
|---|:---:|---|---|
| ui | FA | UGUI Canvas/UI creation | [modules/ui/SKILL.md](modules/ui/SKILL.md) (+ [UI_REFERENCE.md](modules/ui/UI_REFERENCE.md)) |
| uitoolkit | Mixed* | UXML/USS/UIDocument | [modules/uitoolkit/SKILL.md](modules/uitoolkit/SKILL.md) (+ [USS_REFERENCE.md](modules/uitoolkit/USS_REFERENCE.md)) |

Mode legend: SA = runs without grant in all modes, FA = needs grant under Approval, Mixed = check per-skill, `*` = contains auto-forbidden skills.

## Routing

Skills are named `<module>_<verb>` (e.g. `ui_create_button`, `uitoolkit_set_uxml`). UGUI (`ui`) and UI Toolkit (`uitoolkit`) are different Unity UI systems — check which one the target project actually uses (UI Toolkit for Editor windows/modern runtime UI, UGUI for legacy Canvas-based UI) before picking a module. The reference docs for each module cover Inspector field names and USS property specifics that schema omits.

Open the matching module doc before calling any skill whose exact parameters you don't already hold — dryRun first. For simple one-off UI tweaks, guidance mode (talking the user through Inspector/menu steps instead of calling REST skills) is covered in [`unity-core/SKILL_GUIDE.md`](../unity-core/SKILL_GUIDE.md).
