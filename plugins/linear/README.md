# Linear Plugin for Claude Code

Manage Linear issues, kanban boards, and cycles directly from Claude Code via GraphQL API.

## Setup

### 1. Get API Key

Go to [Linear Settings > API](https://linear.app/settings/api) and create a Personal API Key.

### 2. Configure Environment

Add to `~/.claude/settings.local.json`:

```json
{
  "env": {
    "LINEAR_API_KEY": "lin_api_XXXXXXXXXXXX",
    "LINEAR_TEAM": "YOUR-TEAM-KEY"
  }
}
```

Or export in your shell profile:

```bash
export LINEAR_API_KEY="lin_api_XXXXXXXXXXXX"
export LINEAR_TEAM="YOUR-TEAM-KEY"
```

### 3. Install Plugin

```
/plugin marketplace add ./
/plugin install linear@python-backend-plugins
```

## Commands

| Command | Description |
|---------|-------------|
| `/linear:create` | Create a new issue |
| `/linear:get` | Get issue details |
| `/linear:list` | List issues with filters |
| `/linear:update` | Update an issue |
| `/linear:delete` | Archive or delete an issue |
| `/linear:comment` | Manage issue comments |
| `/linear:board` | Kanban board view |
| `/linear:cycle` | Manage cycles (sprints) |

## Examples

```
# Create an issue
/linear:create title="Fix auth token refresh" priority=high labels=bug

# List my issues
/linear:list assignee=me status="In Progress"

# View kanban board
/linear:board

# Current sprint status
/linear:cycle

# Add a comment
/linear:comment id=TEAM-123 action=add text="Fixed in commit abc123"
```

## Smart Defaults

- **Project**: auto-detected from `basename $(pwd)`
- **Team**: from `$LINEAR_TEAM` environment variable
- **Sort**: by priority (urgent first)
- **Active only**: completed/canceled issues excluded by default

## Architecture

- All API calls via `curl` + `jq` (no external dependencies)
- GraphQL endpoint: `https://api.linear.app/graphql`
- Auth: `Authorization: $LINEAR_API_KEY` header
- Issue enricher agent suggests attributes from code context
