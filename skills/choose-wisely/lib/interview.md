# Interview Module — Planning Questions

Structured interview for discovering architectural decisions.

## When to Use

- New project with no CHOICES.md
- Major replanning needed
- User explicitly requests planning

## Interview Categories

Ask questions in this order, grouped by section.

### Mission (M-*)

Core questions:
1. **Problem** — What problem does this solve?
2. **Users** — Who are the primary users?
3. **Value** — What's the core value proposition?
4. **Scope** — What's explicitly in/out of scope?

Example prompts:
- "What pain point are you addressing?"
- "Who wakes up excited to use this?"
- "In one sentence, why does this exist?"

### User Experiences (UX-*)

Core questions:
1. **Workflow** — What's the primary user journey?
2. **Interactions** — How do users interact with this?
3. **Easy vs Powerful** — What should be effortless? What should be configurable?
4. **Errors** — How should failures appear to users?

Example prompts:
- "Walk me through a typical user session"
- "What's the happy path? What's the sad path?"

### Features (F-*)

Core questions:
1. **Core capabilities** — What must it do?
2. **Nice-to-have** — What would be nice but not required?
3. **Differentiators** — What makes this different from alternatives?
4. **Integrations** — What must it connect to?

Example prompts:
- "If you could only have 3 features, which ones?"
- "What would make users switch from competitors?"

### Operations (O-*)

Core questions:
1. **Deployment** — Where does this run?
2. **Scaling** — How does it handle growth?
3. **Monitoring** — How do you know it's healthy?
4. **Recovery** — What happens when things break?

Example prompts:
- "How many concurrent users at peak?"
- "What's acceptable downtime?"

### Data (D-*)

Core questions:
1. **Storage** — What data must persist?
2. **Access patterns** — How is data read/written?
3. **Privacy** — What's sensitive? Who can see what?
4. **Retention** — How long is data kept?

Example prompts:
- "What would be catastrophic to lose?"
- "Does data have different access speeds?"

### Architecture (A-*)

Core questions:
1. **Structure** — Monolith or distributed?
2. **Communication** — Sync or async?
3. **Boundaries** — How do components interact?
4. **Resilience** — Single points of failure?

Example prompts:
- "Do components need to scale independently?"
- "What happens if one piece goes down?"

### Technology (T-*)

Core questions:
1. **Stack** — What languages/frameworks?
2. **Databases** — What storage systems?
3. **Infrastructure** — Where is it hosted?
4. **Constraints** — Any technology mandates or prohibitions?

Example prompts:
- "What does your team already know well?"
- "Any tech debt or legacy constraints?"

### Implementation (I-*)

Core questions:
1. **Process** — How is code written and reviewed?
2. **Testing** — What testing strategy?
3. **CI/CD** — How is code deployed?
4. **Standards** — Any coding conventions?

Example prompts:
- "How do you ensure code quality?"
- "What's your release cadence?"

## Question Format

For each decision point:

```
Q: {question}?

Options:
A) {option} — {tradeoff}
B) {option} — {tradeoff}
C) {option} — {tradeoff}

[Your choice]: _
```

## Recording Decisions

For each answer:

```markdown
### X-0001: {title}
Supports: {parent IDs}

{rationale derived from answer}

Alternatives considered: {options not chosen}
```

## Interview Flow

1. **Discovery** — Scan existing code/docs for context
2. **Questions** — Ask by section, 1-3 per section
3. **Clarification** — Ask follow-ups if answers are vague
4. **Synthesis** — Convert answers to CHOICES.md format
5. **Review** — Show summary, ask for corrections
6. **Generate** — Write CHOICES.md + PLAN.md

## Example

```
## Mission Questions

Q1: What problem does this solve?
    A) Developers need faster API development
    B) Teams need better collaboration tools
    C) Users need simpler data visualization

User: A

Q2: Who are the primary users?
    A) Backend developers
    B) Full-stack teams
    C) API product managers

User: B

Recording:
  M-0001: Accelerate API development for full-stack teams
  Supports: (none - top level)

  Full-stack teams spend too much time on boilerplate.
  This tool reduces API setup from hours to minutes.

[Continues through sections...]

## Summary

Generated 18 choices across 8 sections:
- Mission: 2
- User Experiences: 3
- Features: 5
- Operations: 2
- Data: 2
- Architecture: 2
- Technology: 1
- Implementation: 1

Review CHOICES.md? [y/n]
```
