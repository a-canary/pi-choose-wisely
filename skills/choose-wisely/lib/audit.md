# Audit Module — CHOICES.md Validation

Run 8 validation checks to ensure structural integrity and consistency.

## Process

Run checks iteratively — up to 10 passes. Apply fixes, re-run until convergence (zero new issues).

## Checks (in order)

### 1. Structural Integrity

- [ ] Sections in correct order: Mission > UX > Features > Operations > Data > Architecture > Technology > Implementation
- [ ] Each choice ID uses correct prefix for its section
- [ ] IDs are globally unique (no duplicates)
- [ ] Every choice (except top-most) has `Supports:` line
- [ ] `Supports:` references only valid IDs that exist
- [ ] `Supports:` only points upward (higher priority choices)

### 2. Support Integrity

For each choice:
- [ ] Rationale connects to `Supports:` targets?
- [ ] Orphaned choices flagged (no other choice references them)

### 3. Standalone Comprehensibility

**3a. Undefined terms**
- Flag domain terms used in 3+ choices but not defined where first used

**3b. Opaque names**
- Flag module/component names without explanation

**3c. Scope completeness**
- Mission answers: content scope, audience, platform scope

**3d. Dangling counts**
- Flag numbers without traceable items ("9 data stores" → list them)

### 4. Section Coherence

Within each section:
- [ ] No contradictions between choices
- [ ] No redundancies (same decision stated twice)
- [ ] Ordering is logical (constraining choices first)

### 5. Architecture Tool-Agnosticism

- [ ] Architecture section contains no named technologies
- Architecture describes patterns, Technology names tools
- If found, suggest move to Technology section

### 6. Bottom-Up: Implicit Choices

Look for patterns in lower sections (Data→Implementation):
- [ ] Cluster choices with shared themes
- [ ] Propose abstractions for clusters without explicit parent choice

### 7. Top-Down: Implementation Gaps

- [ ] Every choice in Mission→Operations appears in at least one `Supports:` line in Data→Implementation
- Flag high-level choices without supporting implementation

### 8. Cross-File (if applicable)

- [ ] Parent choices correctly inherited
- [ ] No contradictions with parent
- [ ] No ID collisions

## Auto-Fixes

When issues found, offer to fix:

| Issue | Auto-Fix |
|-------|----------|
| Missing `Supports:` | Suggest based on content |
| Invalid reference | Remove or suggest correct ID |
| Wrong section | Move to correct section |
| Tech in Architecture | Move to Technology |
| Duplicate ID | Renumber |

## Output Format

Run silently. When converged, report tersely:

```
Audit: 23 choices, converged in 2 passes.

[✓] Structure valid
[✓] Support chains intact
[✓] No contradictions
[~] 1 warning: "session timeout" choice references undefined term "JWT"

Fixed: 1 stale reference in timeout operations.
```

If issues remain after 10 passes, list as unresolved with suggestions.
