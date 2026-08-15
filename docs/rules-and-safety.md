# Rules & Safety Protocol

Loop Engineering enforces strict safety controls to prevent code regression, security leaks, or architectural drift.

---

## Enforced Safe-Modification Rules

1. **Inspect Before Edit**: Always execute `view_file` on target source files before modifying them.
2. **Minimal Focused Diffs**: Modify only the lines strictly necessary to satisfy the PRD acceptance criteria.
3. **No Weakening of Tests**: Never delete, comment out, or relax existing test assertions to bypass failures.
4. **No Type / Lint Suppression**: Suppressions like `@ts-ignore` or `eslint-disable` require an explicit inline justification.
5. **No Secrets Hardcoded**: Credentials, API keys, tokens, and private URLs must be injected via environment variables (`.env`).
6. **Lint & Formatting Discipline**: Strict adherence to the repository's existing indentations, semicolon preferences, quotes, and naming schemes.

---

## Failure Recovery & Escalation Protocol

```
+-------------------------------------------------------------+
|                     Test / Lint Failure                     |
+-------------------------------------------------------------+
                               |
                               v
+-------------------------------------------------------------+
|                 Read Full Stack Trace / Logs                |
+-------------------------------------------------------------+
                               |
                               v
+-------------------------------------------------------------+
|                 Identify Root Cause (Not Symptom)           |
|         Grep all callers before applying shared fix         |
+-------------------------------------------------------------+
                               |
                               v
+-------------------------------------------------------------+
|                     Attempt Fix & Retest                    |
+-------------------------------------------------------------+
           |                                     |
    [Success: Green]                  [Failure (Max 3 Attempts)]
           |                                     |
           v                                     v
+-----------------------+             +-----------------------+
|  Advance to Next Step |             | Transition to BLOCKED |
|     (Cross-Verify)    |             |  Emit Blocker Report  |
+-----------------------+             +-----------------------+
```
