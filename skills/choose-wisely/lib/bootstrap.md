# Bootstrap Module — Extract Choices from Docs

Scan existing documentation and extract implicit architectural decisions.

## Sources to Scan

Priority order:
1. `README.md` — project overview, mission
2. `ARCHITECTURE.md` — system design
3. `DESIGN.md` — design decisions
4. `CLAUDE.md` — AI agent instructions
5. `.pi/agent/AGENTS.md` — project guidelines
6. `docs/*.md` — additional docs
7. `specs/*.md` — specifications
8. ADR files (`docs/adr/*.md`, `adr/*.md`)

## Extraction Mapping

| Found In | Section |
|----------|---------|
| Mission statements, values, principles | **Mission** (M-) |
| User journeys, interaction patterns | **User Experiences** (UX-) |
| Specific capabilities, behaviors | **Features** (F-) |
| Automation, SLAs, moderation | **Operations** (O-) |
| Schema decisions, storage patterns | **Data** (D-) |
| System design, module structure | **Architecture** (A-) |
| Named technologies, libraries | **Technology** (T-) |
| Dev practices, coding standards | **Implementation** (I-) |

## Extraction Process

For each document:

1. **Identify decisions** — statements of intent, choices made, patterns adopted
2. **Classify by section** — which of 8 sections does this belong to?
3. **Extract rationale** — why was this chosen? (1-2 sentences)
4. **Note alternatives** — what else was considered?
5. **Find dependencies** — what does this support / depend on?

## Output Format

```yaml
choices:
  - section: Mission
    title: Core value proposition
    rationale: Build X because Y
    source: README.md:12
    supports: []
  - section: Technology
    title: Use PostgreSQL for primary database
    rationale: ACID compliance needed for financial data
    source: ARCHITECTURE.md:45
    supports: [D-0001]
```

## Deduplication

- Same concept, different wording → merge, keep clearest rationale
- Similar but distinct → keep separate
- Contradictory → flag as conflict

## Conflict Marking

```markdown
### A-0012: Data storage approach
Supports: F-0003

<!-- CONFLICT: README.md says PostgreSQL, ARCHITECTURE.md says MongoDB -->
Use PostgreSQL for transactional data, MongoDB for documents.
```

## Gap Detection

After extraction, flag:

1. **Undefined terms** — used in 3+ choices but not explained
2. **Missing scope** — Mission doesn't answer: who, what, platform
3. **Phantom references** — "9 data stores" but only 3 listed
4. **Opaque names** — component/module names without explanation

## ID Assignment

Within each section:
- Order by priority (most constraining first)
- Assign sequential IDs: M-0001, M-0002, UX-0001, F-0001, etc.

## Final Steps

1. Write all choices to CHOICES.md
2. Present summary (N choices, M sources, P conflicts, G gaps)
3. Walk through conflicts with user
4. Run audit to validate
