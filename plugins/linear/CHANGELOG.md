# Changelog

## 2.0.0 (2026-03-06)

### Changed

- **BREAKING**: Migrated standard CRUD operations to official Linear MCP server
- Removed `/linear:create`, `/linear:get`, `/linear:list`, `/linear:update` commands (use Linear MCP tools)
- Removed `comment:add` and `comment:list` actions (use MCP `create_comment` and `list_comments`)
- Trimmed `graphql-queries.md` to only board/cycle/delete/comment-edit-delete queries
- Updated skill to document MCP + plugin hybrid architecture

### Added

- `.mcp.json` for automatic Linear MCP server configuration
- Documentation for hybrid MCP + plugin approach

### Kept

- `/linear:board` — ASCII kanban board (unique, no MCP equivalent)
- `/linear:cycle` — cycle management with velocity stats (no MCP equivalent)
- `/linear:delete` — archive/delete issues (no MCP equivalent)
- `/linear:comment` — edit/delete comments (MCP only supports create)
- `issue-enricher` agent — code-aware issue enrichment

## 1.0.0 (2026-03-06)

### Added

- `/linear:create` — create issues with smart defaults and code-aware enrichment
- `/linear:get` — view issue details by identifier or text search
- `/linear:list` — list and filter issues with compact table output
- `/linear:update` — update issues with before/after diff display
- `/linear:delete` — archive or permanently delete issues
- `/linear:comment` — list, add, edit, delete comments on issues
- `/linear:board` — ASCII kanban board grouped by workflow state
- `/linear:cycle` — cycle management with velocity stats and progress tracking
- `issue-enricher` agent — analyzes code context to suggest issue attributes
- Skill with references for GraphQL queries, filtering syntax, and workflow states
