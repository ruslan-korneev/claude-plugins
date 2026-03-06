# Linear Plugin for Claude Code

Extends the official Linear MCP with kanban board, cycle management, and operations not available in MCP.

## Architecture

**Linear MCP** (standard operations) + **Plugin commands** (unique features).

### What comes from Linear MCP (~21 tools)

- Issues: create, get, list, update, search
- Projects, teams, users management
- Comments: list, create
- Documents and documentation search
- Initiatives, milestones, project updates

### What the plugin adds

| Command | Feature |
|---------|---------|
| `/linear:board` | ASCII kanban board visualization |
| `/linear:cycle` | Cycle management with velocity stats |
| `/linear:delete` | Archive or delete issues |
| `/linear:comment` | Edit/delete comments (MCP only supports create) |

Plus the **issue-enricher agent** that analyzes code context to suggest issue attributes.

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

### 3. Install Plugin

```
/plugin marketplace add ./
/plugin install linear@python-backend-plugins
```

The Linear MCP server is configured automatically via `.mcp.json` in the plugin directory.

To add MCP manually: `claude mcp add --transport http linear-server https://mcp.linear.app/mcp`

## Examples

```
# Kanban board (uses plugin GraphQL)
/linear:board

# Current sprint with velocity stats
/linear:cycle
/linear:cycle action=stats

# Archive an issue
/linear:delete id=TEAM-123

# Edit a comment (MCP doesn't support this)
/linear:comment id=TEAM-123 action=edit comment-id=uuid text="Updated text"

# Standard operations use MCP tools directly:
# "Create an issue titled Fix auth" -> MCP create_issue
# "List my issues" -> MCP list_my_issues
# "Search for pagination bug" -> MCP search
```

## Smart Defaults (plugin commands)

- **Project**: auto-detected from `basename $(pwd)`
- **Team**: from `$LINEAR_TEAM` environment variable
