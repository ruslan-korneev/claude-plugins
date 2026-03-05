---
name: "linear:board"
description: Display kanban board view of Linear issues
allowed_tools:
  - Bash
  - Read
  - Grep
arguments:
  - name: project
    description: "Filter by project (default: basename of pwd)"
    required: false
  - name: assignee
    description: "Filter by assignee name or 'me'"
    required: false
  - name: cycle
    description: "Filter by cycle number or 'current'"
    required: false
---

# Command /linear:board

Display a kanban board view grouped by workflow state.

## Instructions

### Step 0: Validate Configuration

Check `LINEAR_API_KEY` and `LINEAR_TEAM` are set.

### Step 1: Fetch Workflow States

Use `workflowStates` query to get all states for the team. Sort by type order: backlog → unstarted → started → completed. Exclude triage and canceled from board display.

### Step 2: Fetch Issues

Build filter:
- Team: `$LINEAR_TEAM`
- Project: argument or `basename $(pwd)`
- Assignee: if provided
- Cycle: if provided, resolve "current" to active cycle
- Exclude archived issues

Use `issues` query with filter.

### Step 3: Group by State

Group fetched issues by their state into columns.

### Step 4: Render Board

Display as ASCII kanban board:

```markdown
## Board: backend-api (Sprint 42)

| Backlog (3) | Todo (2) | In Progress (4) | In Review (1) | Done (8) |
|-------------|----------|------------------|---------------|----------|
| TEAM-130    | TEAM-128 | TEAM-123         | TEAM-127      | TEAM-110 |
| Fix cache   | Add logs | Auth flow        | API tests     | Setup CI |
| P:Med 2pts  | P:Low 1p | P:Urg 5pts J.D. | P:Hi 3pts J.S | P:Med 2p |
|             |          |                  |               |          |
| TEAM-131    | TEAM-129 | TEAM-124         |               | TEAM-111 |
| Update deps | Refactor | Pagination       |               | Add CORS |
| P:Low 1pt   | P:Med 2p | P:Hi 3pts J.S.  |               | P:Low 1p |
|             |          |                  |               |          |
| TEAM-132    |          | TEAM-125         |               | ...+6    |
| Add metrics |          | User roles       |               |          |
| P:Low 1pt   |          | P:Med 2pts       |               |          |
```

**Formatting rules:**
- Each card shows: identifier, title (truncated to column width), priority + estimate + assignee initials
- Column header includes state name and issue count
- Max 5 cards per column, then show "+N more"
- Skip empty columns unless they are "started" type (always show In Progress)

### Step 5: Summary

Below the board, show summary stats:

```markdown
### Summary
Total: 18 issues | Backlog: 3 | Todo: 2 | In Progress: 4 | In Review: 1 | Done: 8
Progress: [████████████████░░░░] 44%
```
