# Unity Scene Contracts

**Use when:**
- Defining required scene objects
- Planning bootstrap/wiring
- Documenting scene dependencies
- 界定场景必备对象、规划启动/装配、记录场景依赖

Use this skill when scene setup needs to be explicit instead of relying on hidden runtime lookups.

## Define

- Required root objects
- Required components on each root
- Which references are assigned in Inspector
- Which objects act as bootstrap/installers
- Which objects are runtime-spawned
- Which assumptions should be validated early

## Output Format

- Scene object contract
- Bootstrap sequence
- Inspector wiring rules
- Validation rules
- Hidden dependency risks

## Guardrails

- Prefer explicit scene wiring over chains of runtime `Find`.
- Keep bootstrap objects small and focused.
