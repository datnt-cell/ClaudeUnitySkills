# Unity Project Scout

**Use when:**
- First approaching an unfamiliar project
- Before proposing structural changes
- Auditing existing setup
- 初次接触陌生项目、在提结构性改动前、盘点现有配置

Use this before recommending architecture changes in an existing project.

## Inspect First

Collect only the information needed to avoid clashing with the current project:

- Unity version and render pipeline
- Installed packages and notable dependencies
- `asmdef` layout, if any
- Folder structure under `Assets/`
- Whether the project already uses:
  - `ScriptableObject` config
  - service/singleton patterns
  - event-driven flows
  - custom inspectors/property drawers
  - tests
- Existing naming and code organization style

## Suggested Tools / Inputs

- Unity project info and project settings
- Script/file search for patterns
- Local inspection of `Packages/manifest.json`, `Assets/`, and `*.asmdef`

## Output Format

- Technical baseline
- Existing architectural signals
- Existing conventions worth preserving
- Existing risks or inconsistencies
- Constraints for future suggestions
- Unknowns that still need confirmation

## Guardrails

- Do not propose a clean-slate architecture if the project already has a consistent pattern.
- Do not recommend new dependencies until the current stack is clear.
