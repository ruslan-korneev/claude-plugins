---
name: "linear:update"
description: Update a Linear issue
allowed_tools:
  - Bash
  - Read
  - Grep
arguments:
  - name: id
    description: "Issue identifier (e.g., TEAM-123)"
    required: true
  - name: title
    description: "New title"
    required: false
  - name: description
    description: "New description (markdown)"
    required: false
  - name: priority
    description: "New priority: urgent, high, medium, low"
    required: false
  - name: status
    description: "New status (e.g., In Progress, Done)"
    required: false
  - name: assignee
    description: "New assignee name or email"
    required: false
  - name: labels
    description: "Comma-separated label names (replaces existing)"
    required: false
  - name: estimate
    description: "Story points estimate"
    required: false
  - name: due
    description: "Due date (YYYY-MM-DD)"
    required: false
  - name: project
    description: "Move to project"
    required: false
  - name: cycle
    description: "Move to cycle number"
    required: false
---

# Command /linear:update

Update an existing Linear issue. Only changed fields are sent.

## Instructions

### Step 0: Validate Configuration

Check `LINEAR_API_KEY` and `LINEAR_TEAM` are set.

### Step 1: Fetch Current Issue

Parse `id` to extract team key and number. Use `issue` query to get current state. Save current values for diff display.

### Step 2: Resolve Changed Fields

For each provided argument, resolve to UUID where needed (same resolution logic as `/linear:create`):
- status → stateId via `workflowStates`
- assignee → assigneeId via `users`
- labels → labelIds via `issueLabels`
- project → projectId via `projects`
- cycle → cycleId via `cycles`
- priority → numeric value

Build `IssueUpdateInput` with only changed fields.

### Step 3: Execute Mutation

Use `issueUpdate` mutation from [graphql-queries.md](../skills/linear-api/references/graphql-queries.md) with the issue UUID and update input.

### Step 4: Display Diff

Show before → after for changed fields:

```markdown
## Updated TEAM-123

| Field | Before | After |
|-------|--------|-------|
| Status | Todo | In Progress |
| Priority | Medium | High |
| Assignee | - | John Doe |

URL: https://linear.app/...
```
