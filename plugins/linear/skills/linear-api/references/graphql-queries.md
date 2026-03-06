# GraphQL Queries & Mutations

Queries and mutations used by the plugin commands. For standard CRUD operations (create, get, list, update issues, add comments), use the Linear MCP tools instead.

## Queries

### teams — Resolve Team ID

```graphql
query Teams {
  teams {
    nodes {
      id
      name
      key
    }
  }
}
```

### workflowStates — Team States (for board)

```graphql
query WorkflowStates($teamId: String!) {
  workflowStates(filter: { team: { id: { eq: $teamId } } }) {
    nodes {
      id
      name
      type
      position
      color
    }
  }
}
```

### issues — List with Filters (for board)

```graphql
query Issues($filter: IssueFilter, $first: Int, $after: String) {
  issues(filter: $filter, first: $first, after: $after) {
    nodes {
      id
      identifier
      title
      priority
      priorityLabel
      estimate
      dueDate
      state {
        name
        type
      }
      assignee {
        name
      }
      project {
        name
      }
      labels {
        nodes { name }
      }
    }
    pageInfo {
      hasNextPage
      endCursor
    }
  }
}
```

### issue — Get Single Issue (for delete/comment resolution)

```graphql
query Issue($teamKey: String!, $number: Float!) {
  issues(filter: { team: { key: { eq: $teamKey } }, number: { eq: $number } }) {
    nodes {
      id
      identifier
      title
      url
      state { name type }
    }
  }
}
```

### cycles — List Cycles

```graphql
query Cycles($teamId: String!) {
  cycles(filter: { team: { id: { eq: $teamId } } }) {
    nodes {
      id
      name
      number
      startsAt
      endsAt
      completedAt
      progress
      scopeCompleted: completedScopeCount
      scopeTotal: scopeCount
      issues {
        nodes {
          id
          identifier
          title
          state { name type }
          assignee { name }
          estimate
        }
      }
    }
  }
}
```

## Mutations

### issueArchive

```graphql
mutation IssueArchive($id: String!) {
  issueArchive(id: $id) {
    success
  }
}
```

### issueDelete

```graphql
mutation IssueDelete($id: String!) {
  issueDelete(id: $id) {
    success
  }
}
```

### commentUpdate

```graphql
mutation CommentUpdate($id: String!, $input: CommentUpdateInput!) {
  commentUpdate(id: $id, input: $input) {
    success
    comment {
      id
      body
    }
  }
}
```

### commentDelete

```graphql
mutation CommentDelete($id: String!) {
  commentDelete(id: $id) {
    success
  }
}
```

### cycleCreate

```graphql
mutation CycleCreate($input: CycleCreateInput!) {
  cycleCreate(input: $input) {
    success
    cycle {
      id
      name
      number
      startsAt
      endsAt
    }
  }
}
```

**Variables** (`$input`):

```json
{
  "teamId": "uuid",
  "name": "Sprint 42",
  "startsAt": "2026-03-10",
  "endsAt": "2026-03-24"
}
```

## Common Patterns

### Resolve Team ID by Key

```bash
TEAM_ID=$(curl -s -X POST https://api.linear.app/graphql \
  -H "Content-Type: application/json" \
  -H "Authorization: $LINEAR_API_KEY" \
  -d '{"query": "{ teams { nodes { id key } } }"}' \
  | jq -r --arg key "$LINEAR_TEAM" '.data.teams.nodes[] | select(.key == $key) | .id')
```

### Check for API Errors

```bash
if echo "$RESPONSE" | jq -e '.errors' > /dev/null 2>&1; then
  ERROR=$(echo "$RESPONSE" | jq -r '.errors[0].message')
  echo "Linear API error: $ERROR"
fi
```
