# Pi Choose Wisely

A pi-package for architectural decision management using the CHOICES.md framework.

## Installation

```bash
pi install git:github.com/aaron/pi-choose-wisely
# or
pi install /path/to/pi-choose-wisely
```

## Skills

### `/choose-wisely`

Clarify project vision, mission, UX, operations, and architectural decisions in CHOICES.md.

- Auto-bootstraps from existing docs if CHOICES.md missing
- Validates with cascading impact review after changes
- Runs structural audit automatically

```
/choose-wisely                              # Bootstrap or show status
/choose-wisely add OAuth authentication     # Add a choice
/choose-wisely change database to Postgres  # Modify a choice
/choose-wisely audit                        # Full validation
```

### `/choose-wisely:replan`

Gap analysis: compare CHOICES.md against current implementation state, generate PLAN.md for next phase.

```
/choose-wisely:replan                       # Analyze gaps, generate plan
```

## CHOICES.md Structure

8 sections in priority order:

| Section | Prefix | What goes here |
|---------|--------|----------------|
| Mission | M- | Why we exist, values, principles |
| User Experiences | UX- | Core user journeys, interactions |
| Features | F- | Specific capabilities |
| Operations | O- | Automation, SLAs, workflows |
| Data | D- | Storage decisions, schemas |
| Architecture | A- | System design (tool-agnostic) |
| Technology | T- | Tech stack, libraries (names tools) |
| Implementation | I- | Dev practices, standards |

## Choice Format

```markdown
### A-0012: Event-driven architecture for order processing
Supports: F-0003, O-0002

Use message queue for order events. Decouples billing, inventory,
and shipping services. Enables async processing for traffic spikes.
```

**Rules:**
- Position = Priority (higher constrains lower)
- `Supports:` required on every choice (except top)
- Architecture is tool-agnostic, Technology names tools

## Helper Modules

Loaded on-demand by the main skill:

- `lib/bootstrap.md` — Scan docs, extract choices
- `lib/audit.md` — 8 validation checks
- `lib/cascade.md` — 3-check impact review
- `lib/interview.md` — Planning questions by category

## Templates

- `templates/CHOICES.md` — Empty template with 8 sections
- `templates/PLAN.md` — Phased implementation plan

## License

MIT
