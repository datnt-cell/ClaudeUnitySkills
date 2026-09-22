---
name: unity-ui
description: Build Unity UI from chat — UGUI Canvas/widget creation and UI Toolkit (UXML/USS/UIDocument) editing through Editor automation. Use when the user wants menus, HUDs, buttons, panels, or other UI built or edited in the Unity Editor. Read unity-core first for the REST protocol before calling any skill named here.
---

# Unity UI

Part of the Unity Skills plugin. **Read the [unity-core](../unity-core/SKILL.md) skill first** for the First Contact Checklist (health check, operating mode, dryRun gate).

## Modules in this skill

| Module | Mode | Description | Doc |
|---|:---:|---|---|
| ui | FA | UGUI Canvas/UI creation | [modules/ui/SKILL.md](modules/ui/SKILL.md) (+ [UI_REFERENCE.md](modules/ui/UI_REFERENCE.md)) |
| uitoolkit | Mixed* | UXML/USS/UIDocument | [modules/uitoolkit/SKILL.md](modules/uitoolkit/SKILL.md) (+ [USS_REFERENCE.md](modules/uitoolkit/USS_REFERENCE.md)) |

Mode legend defined in unity-core's [MODULE_INDEX.md](../unity-core/MODULE_INDEX.md).

## Routing

Skills are named `<module>_<verb>` (e.g. `ui_create_button`, `uitoolkit_set_uxml`). UGUI (`ui`) and UI Toolkit (`uitoolkit`) are different Unity UI systems — check which one the target project actually uses (UI Toolkit for Editor windows/modern runtime UI, UGUI for legacy Canvas-based UI) before picking a module. The reference docs for each module cover Inspector field names and USS property specifics that schema omits.

Open the matching module doc before calling any skill whose exact parameters you don't already hold — dryRun first per the unity-core protocol. Guidance mode for simple one-off UI tweaks: see unity-core's [SKILL_GUIDE.md](../unity-core/SKILL_GUIDE.md).
