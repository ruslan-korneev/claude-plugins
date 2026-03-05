---
name: "linear:create"
description: Create a new Linear issue
allowed_tools:
  - Bash
  - Read
  - Grep
  - Glob
  - AskUserQuestion
arguments:
  - name: title
    description: "Issue title"
    required: true
  - name: description
    description: "Issue description (markdown)"
    required: false
  - name: priority
    description: "Priority: urgent, high, medium, low"
    required: false
  - name: status
    description: "Initial status (e.g., Backlog, Todo, In Progress)"
    required: false
  - name: assignee
    description: "Assignee name or email"
    required: false
  - name: labels
    description: "Comma-separated label names"
    required: false
  - name: estimate
    description: "Story points estimate"
    required: false
  - name: due
    description: "Due date (YYYY-MM-DD)"
    required: false
  - name: project
    description: "Project name (default: basename of pwd)"
    required: false
  - name: cycle
    description: "Cycle number to add to"
    required: false
---

# Command /linear:create

Create a new issue in Linear.

## Instructions

### Step 0: Validate Configuration

```bash
if [ -z "$LINEAR_API_KEY" ]; then
  echo "ERROR: LINEAR_API_KEY is not set."
  echo ""
  echo "Set it in ~/.claude/settings.local.json:"
  echo '  { "env": { "LINEAR_API_KEY": "lin_api_XXXX", "LINEAR_TEAM": "TEAM" } }'
  echo ""
  echo "Or: export LINEAR_API_KEY=lin_api_..."
  exit 1
fi
```

Check `LINEAR_TEAM` similarly.

### Step 1: Resolve Team ID

Use the `teams` query from [graphql-queries.md](../skills/linear-api/references/graphql-queries.md) to resolve `$LINEAR_TEAM` key into UUID.

### Step 2: Resolve Optional References

For each provided optional argument, resolve to UUID:
- **project**: If not provided, use `basename $(pwd)`. Resolve via `projects` query with `containsIgnoreCase` filter. If no match found, skip (don't fail).
- **assignee**: Resolve via `users` query matching by name, displayName, or email.
- **status**: Resolve via `workflowStates` query matching by name or type.
- **labels**: Split by comma, resolve each via `issueLabels` query.
- **cycle**: Resolve via `cycles` query matching by number.
- **priority**: Map word to number (urgent=1, high=2, medium=3, low=4).

### Step 3: Enrich with Agent (if no description)

If `description` is not provided and the user is working in a code context, invoke the **issue-enricher** agent to analyze current code and suggest:
- Description with file/function references
- Suggested labels
- Estimated complexity
- Priority recommendation

Present suggestions to the user with `AskUserQuestion` and let them accept or modify.

### Step 4: Build Mutation Input

Construct `IssueCreateInput` JSON with all resolved UUIDs:

```bash
INPUT='{"teamId": "'$TEAM_ID'", "title": "'$TITLE'"}'

# Add optional fields only if provided
[ -n "$DESCRIPTION" ] && INPUT=$(echo "$INPUT" | jq --arg d "$DESCRIPTION" '. + {description: $d}')
[ -n "$PRIORITY" ] && INPUT=$(echo "$INPUT" | jq --argjson p "$PRIORITY_NUM" '. + {priority: $p}')
[ -n "$STATE_ID" ] && INPUT=$(echo "$INPUT" | jq --arg s "$STATE_ID" '. + {stateId: $s}')
[ -n "$ASSIGNEE_ID" ] && INPUT=$(echo "$INPUT" | jq --arg a "$ASSIGNEE_ID" '. + {assigneeId: $a}')
[ -n "$PROJECT_ID" ] && INPUT=$(echo "$INPUT" | jq --arg p "$PROJECT_ID" '. + {projectId: $p}')
[ -n "$CYCLE_ID" ] && INPUT=$(echo "$INPUT" | jq --arg c "$CYCLE_ID" '. + {cycleId: $c}')
[ -n "$LABEL_IDS" ] && INPUT=$(echo "$INPUT" | jq --argjson l "$LABEL_IDS" '. + {labelIds: $l}')
[ -n "$ESTIMATE" ] && INPUT=$(echo "$INPUT" | jq --argjson e "$ESTIMATE" '. + {estimate: $e}')
[ -n "$DUE_DATE" ] && INPUT=$(echo "$INPUT" | jq --arg d "$DUE_DATE" '. + {dueDate: $d}')
```

### Step 5: Execute Mutation

Use `issueCreate` mutation from [graphql-queries.md](../skills/linear-api/references/graphql-queries.md).

### Step 6: Display Result

```markdown
## Issue Created

| Field | Value |
|-------|-------|
| ID | TEAM-123 |
| Title | {{ title }} |
| Status | Backlog |
| Priority | High |
| URL | https://linear.app/team/issue/TEAM-123 |
```
