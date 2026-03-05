---
name: "linear:cycle"
description: Manage Linear cycles (sprints)
allowed_tools:
  - Bash
  - Read
  - Grep
arguments:
  - name: action
    description: "Action: current (default), list, stats, create"
    required: false
  - name: name
    description: "Cycle name (for create)"
    required: false
  - name: start
    description: "Start date YYYY-MM-DD (for create)"
    required: false
  - name: end
    description: "End date YYYY-MM-DD (for create)"
    required: false
---

# Command /linear:cycle

Manage Linear cycles (sprints).

## Instructions

### Step 0: Validate Configuration

Check `LINEAR_API_KEY` and `LINEAR_TEAM` are set.

### Step 1: Resolve Team ID

Use `teams` query to resolve `$LINEAR_TEAM` to UUID.

### Step 2: Execute Action

#### action=current (default)

Fetch cycles, find the active one (where `startsAt <= today <= endsAt` and `completedAt` is null). Fetch its issues.

Display:

```markdown
## Current Cycle: Sprint 42

**Period**: Mar 4 — Mar 18, 2026
**Progress**: 12/20 issues completed

[████████████░░░░░░░░] 60%

### Breakdown by Status

| Status | Count | Points |
|--------|-------|--------|
| Done | 8 | 21 |
| In Review | 1 | 3 |
| In Progress | 4 | 13 |
| Todo | 2 | 3 |
| Backlog | 3 | 5 |
| **Total** | **18** | **45** |

### Remaining Work

| ID | Title | Status | Assignee | Pts |
|----|-------|--------|----------|-----|
| TEAM-123 | Auth flow | In Progress | John | 5 |
| TEAM-124 | Pagination | In Progress | Jane | 3 |
| ... |
```

#### action=list

Fetch all cycles for team. Display:

```markdown
## Cycles

| # | Name | Period | Issues | Completed | Progress |
|---|------|--------|--------|-----------|----------|
| 42 | Sprint 42 | Mar 4-18 | 20 | 12 | 60% |
| 41 | Sprint 41 | Feb 18-Mar 3 | 18 | 17 | 94% |
| 40 | Sprint 40 | Feb 4-17 | 15 | 15 | 100% |
```

Mark current cycle with an indicator.

#### action=stats

Fetch last 5 completed cycles. Calculate:
- **Velocity**: average completed points per cycle
- **Throughput**: average completed issues per cycle
- **Completion rate**: average % of scope completed

Display:

```markdown
## Cycle Statistics

### Velocity (last 5 cycles)

| Cycle | Points Completed | Issues Completed | Completion |
|-------|------------------|------------------|------------|
| Sprint 41 | 34 | 17 | 94% |
| Sprint 40 | 30 | 15 | 100% |
| Sprint 39 | 28 | 14 | 88% |
| Sprint 38 | 32 | 16 | 94% |
| Sprint 37 | 26 | 13 | 81% |

**Average velocity**: 30 points/cycle
**Average throughput**: 15 issues/cycle
**Average completion**: 91%

### By Assignee (current cycle)

| Assignee | Completed | In Progress | Points |
|----------|-----------|-------------|--------|
| John Doe | 5 | 2 | 18 |
| Jane Smith | 4 | 1 | 12 |
| Bob Wilson | 3 | 1 | 10 |
```

#### action=create

Requires `name`, `start`, `end`. Use `cycleCreate` mutation from [graphql-queries.md](../skills/linear-api/references/graphql-queries.md).

Display confirmation with cycle details.
