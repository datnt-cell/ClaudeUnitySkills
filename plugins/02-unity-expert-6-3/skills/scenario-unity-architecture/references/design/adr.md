# Unity ADR

**Use when:**
- Choosing between technical approaches
- Comparing libraries or patterns
- Documenting design rationale
- 技术方案选型、库/模式对比、记录设计决策来龙去脉

Use this when architecture choices may be revisited later or when multiple plausible options exist.

## Output Format

- Decision
- Context
- Options considered
- Chosen option
- Why this option won
- Consequences
- Revisit triggers

## Example Use Cases

- Coroutine vs UniTask
- Direct reference vs event-driven communication
- ScriptableObject config vs in-scene authoring
- One assembly vs multiple `asmdef`
- Runtime logic in `MonoBehaviour` vs pure C# service

## Guardrails

- Keep ADRs short.
- Record only decisions that materially affect code generation or architecture direction.
