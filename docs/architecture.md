# Loop Engineering Architecture

This document describes the internal architecture, engine phases, and execution model of the **Loop Engineering** agent.

---

## High-Level Engine Overview

Loop Engineering operates as an autonomous, multi-phase agent loop designed for deterministic software delivery.

```
+-----------------------------------------------------------------------+
|                         PHASE 0: INSPECTION                           |
|  Detect Stack, PM, Src Dirs, Test Runners, DB/API, Auth, Design Tokens |
+-----------------------------------------------------------------------+
                                   |
                                   v
+-----------------------------------------------------------------------+
|                         PHASE 1: DISCOVERY                            |
|  Scan PRD Files -> Parse Front-Matter -> Topological Graph Queue      |
+-----------------------------------------------------------------------+
                                   |
                                   v
+-----------------------------------------------------------------------+
|                         PHASE 2: EXECUTION LOOP                       |
|                                                                       |
|   +---------------------------------------------------------------+   |
|   | 2.1 READ PRD          | Extract 21 dimensions & AC            |   |
|   | 2.2 PLAN INTERNALLY   | Apply Ponytail Ladder & UI/UX Pro Max |   |
|   | 2.3 IMPLEMENT         | Safe diffs, reuse existing code       |   |
|   | 2.4 TEST IMMEDIATELY  | Auto-detected test runner & linter    |   |
|   | 2.5 INVESTIGATE       | Deep stack-trace root-cause analysis  |   |
|   | 2.6 FIX & RETEST      | Minimal diff, 3-attempt escalation    |   |
|   | 2.7 CROSS-VERIFY      | Matrix against AC, A11y, Security     |   |
|   | 2.8 REVIEW            | Dead-code & speculative abstraction   |   |
|   | 2.9 COMPLETE          | Emit summary & update state file      |   |
|   +---------------------------------------------------------------+   |
|                                  |                                    |
|                   [Advance to next PRD in Queue]                      |
+-----------------------------------------------------------------------+
```

---

## Phase Breakdown

### Phase 0: Autonomous Inspection
Before reading any requirement, Phase 0 inspects live workspace indicators to establish project context:
- **Stack Detection**: Inspects package files (`package.json`, `pubspec.yaml`, `composer.json`, `pyproject.toml`, `Cargo.toml`, `go.mod`, `build.gradle`, `pom.xml`, etc.).
- **Package Manager**: Identifies `npm`, `yarn`, `pnpm`, `bun`, `pip`, `poetry`, `uv`, `composer`, `cargo`, etc.
- **Test Frameworks**: Auto-detects Jest, Vitest, PyTest, PHPUnit, Playwright, Cypress, Karma, Cargo Test, Go Test.
- **Architecture Inventory**: Scans for reusable utilities, shared hooks, UI component primitives, API endpoints, schema definitions, and design tokens.

### Phase 1: Dependency Graph Discovery
1. **Recursive Scan**: Scans standard specification paths (`prd/`, `docs/prd/`, `specs/`, `requirements/`, `features/`, `.prd/`).
2. **Metadata Extraction**: Reads YAML front-matter (`type`, `id`, `depends_on`, `requires`, `after`).
3. **Topological Sort**: Computes an execution queue ensuring parent requirements complete before dependent features begin.
4. **State Initialization**: Writes lightweight state to `.loop-engineering-state.json`.

### Phase 2: Processing Loop
Processes each queue item through 10 strict steps:
```
READ -> PLAN -> IMPLEMENT -> TEST -> FIX -> RETEST -> CROSS-VERIFY -> REVIEW -> COMPLETE
```

If a blocker cannot be resolved after 3 consecutive root-cause fix attempts, the PRD transitions to `BLOCKED` with an actionable summary, and the queue advances to the next independent task.

---

## State Persistence & Resume Protocol

- **State File**: `.loop-engineering-state.json` at project root.
- **Hygiene**: No secrets, tokens, or PII are stored in the state file.
- **Verification Rule**: State serves as a queue skeleton only. On resume, the agent re-inspects live files and re-runs tests to verify actual project state before resuming execution.
