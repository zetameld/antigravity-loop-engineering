# Integration Guide: UI/UX Pro Max & Ponytail

Loop Engineering seamlessly combines **Design Intelligence** (UI/UX Pro Max) with **Minimal-Code Architecture** (Ponytail).

---

## 1. UI/UX Pro Max Integration

When any visual, UI, layout, or interaction work is required, Loop Engineering invokes the local UI/UX Pro Max search engine:

```bash
python "~/.gemini/config/skills/ui-ux-pro-max/scripts/search.py" "<query>" --domain <domain>
```

### Supported Query Domains
- `ux` — Accessibility, touch target sizing, layout shifts, form validation patterns.
- `style` — Visual design styles (Glassmorphism, Bento Grid, Modern Minimal, Dark Mode).
- `color` — Accessible color palettes and HSL tokens.
- `typography` — Font pairing profiles and Google Fonts.
- `chart` — Accessible data visualization recommendations.
- `gsap` — Animation presets and scroll transitions.
- `<stack>` — Stack-specific UI implementation guidelines (`nextjs`, `react`, `vue`, `laravel`, `flutter`, `swiftui`, `react-native`, etc.).

---

## 2. Ponytail Integration (The Minimal-Code Ladder)

Before writing new code, Loop Engineering enforces the 7-rung Ponytail Ladder:

| Rung | Evaluation | Action |
|------|------------|--------|
| **1** | Does this need to exist? | Skip speculative features (YAGNI). |
| **2** | Already in codebase? | Reuse existing helper, component, or type. |
| **3** | Stdlib covers it? | Use built-in language standard library. |
| **4** | Native platform feature covers it? | Use native HTML5 (`<input type="date">`), CSS grid/flex, or DB constraints. |
| **5** | Already-installed dependency solves it? | Use installed package; avoid adding new ones. |
| **6** | Can it be one line? | Write the shortest working implementation. |
| **7** | Only then | Write minimal custom code with tests. |

### Simplification Annotations
When a deliberate simplification is chosen, it is marked directly in source code:

```typescript
// loop: simple O(n) memory scan; convert to indexed DB query if records > 10,000
```
