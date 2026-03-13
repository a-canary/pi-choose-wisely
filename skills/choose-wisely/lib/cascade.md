# Cascade Module — Impact Review

Three-check cascade for propagating change impacts through the choice hierarchy.

## Principle

Changing a choice can affect:
1. What it supports (upward)
2. Peers in same section (lateral)
3. What depends on it (downward)

## The Three Checks

### Check 1: Upward — Support Integrity

**Question:** Does the changed choice still support its `Supports:` targets?

```
Before: A-0012 "Use event queue" supports F-0003 "Real-time updates"
Change: A-0012 → "Use synchronous API calls"
Check: Does sync API still support real-time updates? NO
Result: Flag broken support, suggest alternatives
```

Actions:
- Verify support still holds
- If weakened: note degraded support
- If broken: flag and propose alternatives

### Check 2: Lateral — Section Coherence

**Question:** Any contradictions or redundancies with peer choices?

```
Section: Technology
Existing: T-0003 "Use PostgreSQL for transactions"
Change: Add T-0008 "Use MongoDB for all data"
Check: Contradiction! Can't use both for all data
Result: Flag conflict, ask user to resolve
```

Actions:
- Scan same section for contradictions
- Identify redundancies (same decision twice)
- Check ordering (more constraining first)

### Check 3: Downward — Impact Propagation

**Question:** Do choices that reference this one still hold?

```
Changed: A-0012 "Use event queue" → "Use synchronous API"
Dependents:
  - D-0005 "Store events in Kafka" (supports A-0012)
  - O-0003 "Async event processing" (supports A-0012)

Check: These depend on event queue. Sync API invalidates them.
Result: Propose modifications to D-0005, O-0003
```

Actions:
- Find all choices with changed ID in their `Supports:`
- Assess if dependency still valid
- Propose modifications → re-enters cascade

## Cascade Algorithm

```
function cascade(change, depth=0):
  if depth > 10:
    return "Max depth exceeded"

  issues = []

  # Check 1: Upward
  for target in change.supports:
    if not supports_target(change, target):
      issues.append(broken_support(change, target))

  # Check 2: Lateral
  for peer in get_section_peers(change.section):
    if contradicts(change, peer):
      issues.append(contradiction(change, peer))
    if redundant(change, peer):
      issues.append(redundancy(change, peer))

  # Check 3: Downward
  for dependent in get_dependents(change.id):
    if not dependency_holds(dependent, change):
      proposed = propose_fix(dependent, change)
      issues.append(impact(dependent, proposed))
      cascade(proposed, depth + 1)  # Recurse

  return issues
```

## Convergence

Cascade recursively until:
- No new issues found
- Max depth (10) reached

## Output

Run silently. When converged, summarize tersely:

> Updated "event architecture" choice. Cascade found 2 impacts: "Kafka storage" choice now needs revision, "async processing" choice marked for review. Fixed stale reference. 4 choices touched total.

## Example Session

```
User: Change A-0012 from event queue to synchronous API

Analyzing change...
Check 1: A-0012 supports F-0003 "real-time updates"
  → Sync API weakens support. Flag for review.

Check 2: Lateral scan of Architecture section
  → No contradictions found.

Check 3: Finding dependents...
  → D-0005 "Kafka storage" depends on A-0012
  → O-0003 "Async processing" depends on A-0012
  → Both invalidated by sync API change.

Cascade continues...
  → Proposing D-0005 revision: "HTTP request storage"
  → Proposing O-0003 revision: "Synchronous processing"

Converged after 2 passes.

Summary: Changed "event architecture" to synchronous. 3 downstream choices affected:
- D-0005: Kafka → HTTP logs
- O-0003: Async → Sync processing
- F-0003: Real-time capability degraded (warning)

Proceed with these changes?
```
