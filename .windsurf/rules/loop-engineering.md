---
description: "Loop Engineering - Autonomous PRD-processing loop with UI/UX Pro Max design intelligence and Ponytail YAGNI minimal-code rules."
globs: "**/*"
alwaysApply: true
---

# Loop Engineering Rules

Activates automatically on meaningful coding tasks (implementing features, refactoring, fixing bugs, writing tests, reviewing code).

## Core Directives

1. **Ponytail Minimal-Code Ladder**:
   - Stop at the first rung that holds:
     1. Does this feature need to exist? (YAGNI)
     2. Already in codebase? → Reuse existing components/hooks/utils/services/types.
     3. Stdlib / platform native covers it? → Use it.
     4. Already-installed dependency solves it? → Use it.
     5. Can it be one line / minimal diff? → Write shortest correct implementation.
     6. Only then write new code.
   - Annotate deliberate simplifications: `// loop: <description>, <ceiling>, <upgrade path>`

2. **UI/UX Pro Max Design Intelligence**:
   - Query `~/.gemini/config/skills/ui-ux-pro-max/scripts/search.py` before making visual design choices.
   - Enforce WCAG 2.1 AA contrast (4.5:1 minimum), touch target sizes (≥44×44px), ARIA semantics, and keyboard navigation.

3. **Safe-Modification Rules**:
   - Inspect files (`view_file`) before making modifications.
   - Minimal focused diffs only.
   - Never weaken, disable, or delete existing tests or lint/type rules.
   - Never hardcode secrets or credentials — use environment variables.
   - Respect repo lint, formatting, and file structure conventions.

4. **PRD Processing Loop**:
   - Process one PRD at a time in dependency order:
     `DISCOVER → READ → PLAN → IMPLEMENT → TEST → FIX → RETEST → CROSS-VERIFY → REVIEW → COMPLETE`
   - Store lightweight queue state in `.loop-engineering-state.json` (must be gitignored).
   - Re-inspect live repo on every restart.
