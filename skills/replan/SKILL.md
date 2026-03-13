---
name: replan
description: "Gap analysis: compare CHOICES.md against current state, generate PLAN.md for next implementation phase."
allowed-tools: Read, Glob, Grep, Bash, Edit
---

# Replan — Gap Analysis & Plan Generation

Compare CHOICES.md goals against current implementation. Generate PLAN.md for next phase.

## When to Use

- Current PLAN.md is 100% complete
- Major scope change requires new planning
- User asks to "replan" or "next phase"
- Starting fresh implementation cycle

## Workflow

### 1. Read CHOICES.md

Parse all 8 sections, extract:
- Choice ID, title, decision
- `Supports:` dependencies
- Section ordering (M > UX > F > O > D > A > T > I)

### 2. Analyze Current State

Check implementation status:

```bash
# What's been committed?
git log --oneline -20

# What's the current structure?
find . -type f -name "*.ts" -o -name "*.py" -o -name "*.js" | head -50

# Any existing PLAN.md?
cat PLAN.md 2>/dev/null || echo "No PLAN.md"

# Memory/log of what's done?
cat MEMORY.md 2>/dev/null || cat .pi/memory.md 2>/dev/null || echo "No memory file"
```

### 3. Categorize Choices

| Status | Criteria |
|--------|----------|
| **Fulfilled** | Code exists, tests pass, documented |
| **Partial** | Started but incomplete |
| **Not started** | No implementation evidence |

### 4. Prioritize Unfulfilled

Order by:
1. Section priority (M > UX > F > O > D > A > T > I)
2. Dependencies (unblocked first)
3. Impact (high-impact choices first)

### 5. Generate PLAN.md

Use [plan template](../templates/PLAN.md). Group into phases:

```markdown
## Phase 1: {Focused Goal}
### Steps
- [ ] 1.1 {Action verb} {specific thing}
- [ ] 1.2 {Action verb} {specific thing}

### Gates
- [ ] {Testable criterion}
```

**Rules:**
- 3-7 steps per phase
- 1-3 gates per phase
- Gates must be testable (can run command, check output)
- Max 300 lines total

### 6. Validate Plan

Run plan validation checks:
- [ ] Line count ≤ 300
- [ ] Objective section present
- [ ] Each phase has Steps + Gates
- [ ] Steps are actionable (start with verb)
- [ ] Gates are testable (not subjective)
- [ ] Scope defined (In/Out)

### 7. Output

```
Gap Analysis Complete

CHOICES.md: {N} choices
  ✓ Fulfilled: {a}
  ◐ Partial: {b}
  ✗ Not started: {c}

PLAN.md generated: {M} phases, {K} tasks
  Phase 1: {name} ({n} steps)
  Phase 2: {name} ({n} steps)
  ...

Ready to implement. Run /choose-wisely:replan again when complete.
```

## Example

```
User: /choose-wisely:replan

Analyzing CHOICES.md...
  M-0001: Real-time chat platform — fulfilled ✓
  UX-0001: Channel-based messaging — fulfilled ✓
  F-0003: Video integration — partial ◐
  F-0008: Search — not started ✗
  O-0012: Monitoring — not started ✗

Gap Analysis Complete

CHOICES.md: 23 choices
  ✓ Fulfilled: 8
  ◐ Partial: 2
  ✗ Not started: 13

PLAN.md generated: 3 phases, 24 tasks
  Phase 1: Search implementation (8 steps)
  Phase 2: Video completion (6 steps)
  Phase 3: Monitoring setup (5 steps)

Ready to implement.
```

## Integration

After PLAN.md is 100% complete:
1. Run `/choose-wisely:replan` again for next phase
2. Or update CHOICES.md with new decisions first
3. Cycle continues until all choices fulfilled
