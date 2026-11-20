# Unity Testability Advisor

**Use when:**
- Improving testability
- Extracting logic for unit tests
- Planning a test strategy
- 提升可测试性、为单元测试抽取逻辑、规划测试策略

Use this skill when deciding what logic should remain in Unity-facing classes and what should move into pure C# code.

## Review Questions

- Can the rule/algorithm run without `Transform`, `GameObject`, or scene state?
- Can config be injected instead of read through static globals?
- Can runtime decisions be moved to a plain C# class and called from a thin `MonoBehaviour`?
- Does this need PlayMode coverage, or is EditMode enough?

## Output Format

- Logic that should move to pure C#
- Logic that should stay Unity-facing
- Suggested seams/interfaces
- Candidate EditMode tests
- Candidate PlayMode tests

## Guardrails

- Do not force test seams everywhere if the script is tiny and scene-bound.
- Prefer a few meaningful seams over abstraction for its own sake.
