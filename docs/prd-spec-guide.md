# PRD Specification Guide

This guide explains how to write PRDs and specifications compatible with the **Loop Engineering** agent.

---

## PRD Discovery & File Locations

Loop Engineering recursively searches these locations for specification documents:
- `prd/` or `PRD/`
- `docs/prd/` or `docs/PRD/`
- `specs/` or `docs/specs/`
- `requirements/` or `docs/requirements/`
- `features/`
- Root markdown files (`*.prd.md`, `PRD-*.md`, `FEAT-*.md`, `SPEC-*.md`)

---

## Standard YAML Front-Matter Schema

Every PRD document should include a YAML front-matter block:

```yaml
---
type: prd                  # Required: prd | spec | feature | story
id: FEAT-102               # Required: Unique identifier
title: User Dashboard      # Required: Descriptive title
depends_on:                # Optional: List of prerequisite PRD IDs
  - FEAT-101
---
```

---

## Document Structure & Essential Sections

To allow complete autonomous processing, your PRD should cover the following sections:

```markdown
# PRD: [Feature Name]

## 1. Goals & Objectives
- High-level business objective
- Success metrics

## 2. User Flows
- Step-by-step persona navigation paths
- Edge cases and alternative flows

## 3. Acceptance Criteria
- [ ] Criterion 1 (Must be quantifiable and testable)
- [ ] Criterion 2
- [ ] Criterion 3

## 4. UI / UX Requirements
- Screen layout and component requirements
- Loading, empty, and error states
- Mobile and desktop responsive breakpoints

## 5. API & Data Requirements
- Endpoints, request/response payload schemas
- Database tables, migrations, relationships

## 6. Security & Accessibility (WCAG 2.1 AA)
- Authentication and permission matrix
- ARIA semantics, color contrast, keyboard navigation

## 7. Out of Scope
- Explicitly excluded items
```

---

## Dependency Queue Example

Consider a project with three specifications:

```
+-------------------+       +-------------------+
| FEAT-001          | ----> | FEAT-002          |
| Database Schema   |       | Auth API          |
+-------------------+       +-------------------+
                                      |
                                      v
                            +-------------------+
                            | FEAT-003          |
                            | Auth UI Screen    |
                            +-------------------+
```

Loop Engineering automatically sorts these into execution order: `FEAT-001` -> `FEAT-002` -> `FEAT-003`.
