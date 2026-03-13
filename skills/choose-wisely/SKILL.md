---
name: choose-wisely
description: "Clarify project vision, mission, UX, operations, and architectural decisions in CHOICES.md. Auto-bootstraps from existing docs, validates with cascading impact review."
---

# Choose Wisely — Architectural Decision Management

Manage project choices in CHOICES.md. Auto-bootstraps if missing, runs cascading audit after changes.

## Quick Routing

| User Input | Action |
|------------|--------|
| No CHOICES.md | Bootstrap from template + existing docs |
| "audit" / "check" | Run full validation, report issues |
| "init" / "bootstrap" | Scan docs, extract choices |
| Describing a change | Apply with 3-check cascade |
| Empty/new project | Offer planning interview |

## Workflow

### 1. Discover

Find CHOICES.md (project root, docs/, .pi/). If missing, create from template:

```
Template: {{SKILL_DIR}}/../templates/CHOICES.md
Target: {{PROJECT_ROOT}}/CHOICES.md
```

### 2. Parse State

If CHOICES.md exists, parse into:
- Sections (Mission → Implementation)
- Choices with ID, title, rationale
- `Supports:` dependency graph

### 3. Route by Input

#### Bootstrap (no CHOICES.md or "init")

Load [bootstrap module](lib/bootstrap.md) to scan existing docs:
- README.md, ARCHITECTURE.md, DESIGN.md
- docs/*.md, specs/*.md
- ADR files

Extract choices → assign IDs → write CHOICES.md.

#### Audit ("audit" / "check")

Load [audit module](lib/audit.md) and run 8 validation checks. Report issues tersely:

> 23 choices audited. 1 stale reference in "timeout operations" choice. No gaps, no contradictions. Clean.

#### Change (describing modification)

Apply edit, then run [cascade module](lib/cascade.md):

**Three checks per change:**
1. **Upward**: Does change still support its `Supports:` targets?
2. **Lateral**: Any contradictions/redundancies in same section?
3. **Downward**: Do choices referencing this one still hold?

Cascade silently to convergence. Summarize in 2-3 sentences by title.

#### Planning (empty project / "plan")

Load [interview module](lib/interview.md) and conduct structured interview:
- Mission questions (why, who, value)
- UX questions (workflow, interactions)
- Architecture questions (patterns, communication)
- Technology questions (frameworks, tools)

Generate CHOICES.md with decisions + PLAN.md for next phase.

### 4. Always Audit

After any change, run audit checks. Only surface if issues found.

### 5. Commit

Commit CHOICES.md independently with descriptive message.

## CHOICES.md Format

```markdown
### X-0001: Title of choice
Supports: X-0000, Y-0000

One to two lines of rationale. Not a spec.
```

**Sections (fixed order):**
1. Mission (M-)
2. User Experiences (UX-)
3. Features (F-)
4. Operations (O-)
5. Data (D-)
6. Architecture (A-)
7. Technology (T-)
8. Implementation (I-)

**Rules:**
- Position = Priority (higher constrains lower)
- `Supports:` required on every choice except top
- Architecture is tool-agnostic, Technology names tools

## Modules

- [lib/bootstrap.md](lib/bootstrap.md) — Scan docs, extract choices
- [lib/audit.md](lib/audit.md) — 8 validation checks
- [lib/cascade.md](lib/cascade.md) — 3-check impact review
- [lib/interview.md](lib/interview.md) — Planning questions
