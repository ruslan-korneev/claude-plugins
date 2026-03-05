# Linear API Skill

Manage Linear issues, boards, and cycles directly from Claude Code via GraphQL API.

## When to Use This Skill

Use this skill when the user wants to:
- Create, view, update, or delete issues in Linear
- View a kanban board of current tasks
- Manage sprints/cycles and track velocity
- Add comments to issues
- Search or filter issues by project, status, priority, assignee

**Trigger phrases:**
- "Create an issue in Linear"
- "Show my Linear tasks"
- "What's on the board?"
- "Current sprint status"
- "Update issue TEAM-123"
- "List issues for this project"
- "Add a comment to TEAM-456"

## Configuration

### Required Environment Variables

| Variable | Description | Example |
|----------|-------------|---------|
| `LINEAR_API_KEY` | Personal API Key from Linear Settings > API | `lin_api_XXXXXXXXXXXX` |
| `LINEAR_TEAM` | Default team key | `ENG` |

Set in `~/.claude/settings.local.json`:

```json
{
  "env": {
    "LINEAR_API_KEY": "lin_api_XXXXXXXXXXXX",
    "LINEAR_TEAM": "ENG"
  }
}
```

Or via shell: `export LINEAR_API_KEY=lin_api_...`

### Smart Defaults

- **Team**: `$LINEAR_TEAM` environment variable
- **Project**: `basename $(pwd)` — auto-detected from current directory

## API Pattern

All calls use Linear's GraphQL API:

```bash
curl -s -X POST https://api.linear.app/graphql \
  -H "Content-Type: application/json" \
  -H "Authorization: $LINEAR_API_KEY" \
  -d '{"query": "...", "variables": {...}}'
```

### Error Handling

1. Check `LINEAR_API_KEY` is set — show setup instructions if missing
2. Parse response with `jq`
3. Check for errors: `echo "$RESPONSE" | jq -e '.errors' > /dev/null 2>&1`
4. If errors exist, extract message: `echo "$RESPONSE" | jq -r '.errors[0].message'`

### Pagination

Linear uses cursor-based pagination:

```graphql
issues(first: 50, after: $cursor) {
  nodes { ... }
  pageInfo { hasNextPage endCursor }
}
```

Loop while `hasNextPage` is true, passing `endCursor` as `after`.

## References

- [GraphQL Queries](references/graphql-queries.md) — All queries and mutations
- [Filtering](references/filtering.md) — Filter syntax and operators
- [Workflow States](references/workflow-states.md) — Status model and transitions
