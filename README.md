<h1 align="center">Loop Engineering</h1>

<p align="center">
  <em>Senior architect + full-stack + QA + code reviewer, in one autonomous agent.</em>
</p>

<p align="center">
  <img src="https://img.shields.io/github/v/release/zetameld/antigravity-loop-engineering?style=flat-square&color=111111&label=release" alt="Release">
  <img src="https://img.shields.io/badge/works%20with-Antigravity%20%7C%20Gemini%20CLI%20%7C%20Claude%20Code%20%7C%2015+%20agents-111111?style=flat-square" alt="Works with 15+ agents">
  <img src="https://img.shields.io/badge/license-MIT-111111?style=flat-square" alt="MIT license">
</p>

---

You drop PRDs into your repo. Loop Engineering finds them, builds a
dependency-ordered queue, and processes each one end-to-end — inspect,
plan, implement, test, fix, verify, review — without stopping to ask
unnecessary questions.

It integrates **[UI/UX Pro Max](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill)**
as its mandatory design-intelligence source and
**[Ponytail](https://github.com/DietrichGebert/ponytail)** as its mandatory
YAGNI/minimal-code philosophy.

## The Loop

```
Phase 0  INSPECT      detect stack, PM, src, tests, DB/API, auth, design system
Phase 1  DISCOVER     find all PRD/spec files, topological sort by dependencies
Phase 2  For each PRD (one at a time):
           READ        goals, flows, acceptance criteria, screens, entities,
                       DB/API, rules, permissions, validations, errors,
                       loading, security, a11y, responsiveness, testing, NFRs
           PLAN        smallest correct architecture (Ponytail Ladder)
           IMPLEMENT   inspect before edit; reuse first; no secrets hardcoded
           TEST        auto-detect unit/type/lint/build/e2e commands from repo
           FIX         root-cause only; never weaken tests
           RETEST      confirm green
           CROSS-VERIFY acceptance criteria, security, a11y, responsive, perf
           REVIEW      strip dead code, duplication, speculative abstractions
           COMPLETE    emit summary; advance to next PRD
```

Persists a lightweight `.loop-engineering-state.json` so restarts resume
from where they left off (always re-inspecting the live repo first).

## Install

### Antigravity CLI (`agy`)

```bash
agy plugin install https://github.com/zetameld/antigravity-loop-engineering
```

Or the longer form that also works while Antigravity is still called Gemini CLI:

```bash
gemini extensions install https://github.com/zetameld/antigravity-loop-engineering
```

Skills become available as `/loop-engineering` and `/loop-engineering-review`
in the chat.

**Global skill only (no plugin):**
```bash
# Copy the SKILL.md into your global Antigravity skills directory
mkdir -p ~/.gemini/config/skills/loop-engineering
curl -fsSL https://raw.githubusercontent.com/zetameld/antigravity-loop-engineering/main/skills/loop-engineering/SKILL.md \
  -o ~/.gemini/config/skills/loop-engineering/SKILL.md
```

### Claude Code

```
/plugin marketplace add zetameld/antigravity-loop-engineering
```
```
/plugin install loop-engineering@loop-engineering
```
(Send as two separate prompts.)

### Codex

```bash
codex plugin marketplace add zetameld/antigravity-loop-engineering
codex plugin add loop-engineering@loop-engineering
```

### Qoder

Qoder auto-loads `AGENTS.md` from the repo root. Run from a checkout, or
copy `AGENTS.md` into your project root — zero additional setup.

For full plugin support, add to your `.qoder/settings.json`:
```json
{
  "hooks": {
    "UserPromptSubmit": "LOOP_ENGINEERING_DIR/hooks/qoder-hooks.json"
  }
}
```

### OpenCode

Add to `opencode.json`:
```json
{ "plugin": ["https://github.com/zetameld/antigravity-loop-engineering"] }
```

### Cursor / Windsurf / Cline / Copilot Chat / Kiro / Zed

These tools auto-load `AGENTS.md`. Copy it to your project root:

```bash
curl -fsSL https://raw.githubusercontent.com/zetameld/antigravity-loop-engineering/main/AGENTS.md \
  >> .agents/AGENTS.md
# or directly to project root:
curl -fsSL https://raw.githubusercontent.com/zetameld/antigravity-loop-engineering/main/AGENTS.md \
  -o AGENTS.md
```

For **Kiro** global steering:
```bash
mkdir -p ~/.kiro/steering
curl -fsSL https://raw.githubusercontent.com/zetameld/antigravity-loop-engineering/main/AGENTS.md \
  -o ~/.kiro/steering/loop-engineering.md
```

For **Copilot Chat** global instructions:
```bash
cat >> ~/.copilot/copilot-instructions.md \
  < <(curl -fsSL https://raw.githubusercontent.com/zetameld/antigravity-loop-engineering/main/AGENTS.md)
```

### GitHub Copilot CLI

```bash
copilot plugin marketplace add zetameld/antigravity-loop-engineering
copilot plugin install loop-engineering@loop-engineering
```

### Devin

```bash
devin plugins install zetameld/antigravity-loop-engineering
```

### Grok Build

```bash
grok plugin install zetameld/antigravity-loop-engineering --trust
```

Enable in config:
```toml
[plugins]
enabled = ["loop-engineering"]
```

### CodeWhale / Swival / Aider / any AGENTS.md-aware tool

```bash
# Works from a clone or by copying AGENTS.md to your project root
git clone https://github.com/zetameld/antigravity-loop-engineering
# then run your agent from the cloned dir, or copy AGENTS.md to your project
```

### Manual (any agent, guaranteed to work)

```bash
# Clone
git clone https://github.com/zetameld/antigravity-loop-engineering ~/.local/share/loop-engineering

# Copy SKILL.md to your global Antigravity skills folder
mkdir -p ~/.gemini/config/skills/loop-engineering
cp ~/.local/share/loop-engineering/skills/loop-engineering/SKILL.md \
   ~/.gemini/config/skills/loop-engineering/SKILL.md

# Copy AGENTS.md to your project (instruction-only fallback for all agents)
cp ~/.local/share/loop-engineering/AGENTS.md ./AGENTS.md
```

## PRD Format

Create a file anywhere the skill looks (`prd/`, `docs/prd/`, `specs/`,
`requirements/`, `features/`, etc.) with this shape:

```markdown
---
type: prd
id: FEAT-001
title: My Feature
depends_on: []   # list other PRD IDs this one needs first
---

# PRD: My Feature

## Goals
...

## User Flows
...

## Acceptance Criteria
- [ ] Testable condition 1
- [ ] Testable condition 2

## Out of Scope
...
```

See [`examples/example-prd.md`](examples/example-prd.md) for a complete example.

## Skills Included

| Skill | Trigger | What it does |
|-------|---------|--------------|
| `loop-engineering` | Any meaningful coding task | Full PRD loop |
| `loop-engineering-review` | `/loop-engineering-review [path]` | Structured code review: correctness, dead code, duplication, unsafe shortcuts, test gaps, a11y, security |

## Philosophy

Loop Engineering enforces two philosophies and refuses to deviate:

**[Ponytail](https://github.com/DietrichGebert/ponytail) — YAGNI/minimal-code:**
Climbs the 7-rung ladder before writing any code. Marks deliberate
simplifications with `// loop: <desc>, <ceiling>, <upgrade path>`. Never
cuts validation, error handling, security, or accessibility.

**[UI/UX Pro Max](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill) — design intelligence:**
Queries the local search tool for every visual decision. WCAG 2.1 AA is the
minimum floor, not a stretch goal.

## Safe-Modification Rules

Always enforced, no exceptions:

| Rule | Detail |
|------|--------|
| Inspect before edit | Read the file before any write |
| Minimal diff | Change only what the PRD requires |
| No test weakening | Never delete, skip, or relax assertions |
| No type suppression | No `@ts-ignore` without explanation |
| No secret hardcoding | Env vars / repo's existing pattern |
| No speculation | Build what the PRD says, nothing more |
| Convention matching | Match lint, format, naming, import style |
| State file gitignored | `.loop-engineering-state.json` → `.gitignore` |

## Maintenance

> **Periodic manual check (no network required during normal runs):**
> Re-check [UI/UX Pro Max](https://github.com/nextlevelbuilder/ui-ux-pro-max-skill)
> and [Ponytail](https://github.com/DietrichGebert/ponytail) for meaningful upstream
> updates (new domains, revised rules, changed CLI flags). This is a human-triggered,
> optional review — the skill runs fully offline.

## License

MIT
