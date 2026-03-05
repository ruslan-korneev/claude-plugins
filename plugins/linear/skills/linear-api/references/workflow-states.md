# Workflow States Model

Linear organizes issue states into typed categories that define the workflow.

## State Types

| Type | Description | Typical States |
|------|-------------|----------------|
| `triage` | New issues awaiting triage | Triage |
| `backlog` | Triaged but not planned | Backlog |
| `unstarted` | Planned but not started | Todo |
| `started` | Work in progress | In Progress, In Review |
| `completed` | Done | Done, Merged |
| `canceled` | Won't do | Canceled, Duplicate |

## Default Workflow

```
Triage → Backlog → Todo → In Progress → In Review → Done
                                                   ↘ Canceled
```

## State Properties

Each state has:
- **id** — UUID, used in mutations
- **name** — Display name (e.g., "In Progress")
- **type** — Category (one of the 6 types above)
- **position** — Ordering within the type
- **color** — Hex color for UI

## Querying States

```bash
curl -s -X POST https://api.linear.app/graphql \
  -H "Content-Type: application/json" \
  -H "Authorization: $LINEAR_API_KEY" \
  -d '{"query": "{ workflowStates(filter: { team: { key: { eq: \"'$LINEAR_TEAM'\" } } }) { nodes { id name type position color } } }"}' \
  | jq '.data.workflowStates.nodes | sort_by(.position)'
```

## State Type Grouping for Board

For kanban board display, group states by type and order by position:

```bash
# Get states sorted by type order then position
STATES=$(echo "$RESPONSE" | jq '[.data.workflowStates.nodes | sort_by(.position)]')

# Group by type for columns
for TYPE in backlog unstarted started completed; do
  echo "$STATES" | jq --arg t "$TYPE" '[.[] | select(.type == $t)]'
done
```

## Mapping User Input to States

When user says "in progress", "done", "todo", etc., match to state:

| User Input | Match Strategy |
|------------|----------------|
| State name exactly | `name == input` |
| State type | `type == input` |
| Common aliases | "wip" → "In Progress", "done" → type "completed" |
| Partial match | `name` contains input (case-insensitive) |

```bash
# Try exact name match first, then type match, then contains
STATE_ID=$(echo "$STATES_JSON" | jq -r --arg s "$STATUS" '
  (.[] | select(.name == $s) | .id) //
  (.[] | select(.type == $s) | .id) //
  (.[] | select(.name | test($s; "i")) | .id) //
  empty
')
```

## Priority Mapping

Linear priorities are numeric but users often use words:

| Value | Label | Description |
|-------|-------|-------------|
| 0 | No priority | Not prioritized |
| 1 | Urgent | Critical, needs immediate attention |
| 2 | High | Important, should be done soon |
| 3 | Medium | Normal priority |
| 4 | Low | Nice to have |

```bash
priority_to_number() {
  case "$(echo "$1" | tr '[:upper:]' '[:lower:]')" in
    urgent|1) echo 1 ;;
    high|2) echo 2 ;;
    medium|3) echo 3 ;;
    low|4) echo 4 ;;
    none|0) echo 0 ;;
    *) echo "$1" ;;
  esac
}
```
