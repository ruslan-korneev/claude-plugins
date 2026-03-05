---
name: "linear:delete"
description: Archive or delete a Linear issue
allowed_tools:
  - Bash
  - Read
  - Grep
arguments:
  - name: id
    description: "Issue identifier (e.g., TEAM-123)"
    required: true
  - name: archive
    description: "Archive instead of delete (default: true)"
    required: false
---

# Command /linear:delete

Archive or permanently delete a Linear issue. Archives by default.

## Instructions

### Step 0: Validate Configuration

Check `LINEAR_API_KEY` and `LINEAR_TEAM` are set.

### Step 1: Fetch Issue

Parse `id` to extract team key and number. Fetch issue to confirm it exists and get its UUID and title.

### Step 2: Execute Action

**If archive (default, `archive` is not "false"):**

Use `issueArchive` mutation from [graphql-queries.md](../skills/linear-api/references/graphql-queries.md).

**If permanent delete (`archive=false`):**

Use `issueDelete` mutation. This is irreversible.

### Step 3: Display Result

```markdown
## Issue Archived

**TEAM-123**: Issue title here

The issue has been archived and moved to the archive view.
```

Or for permanent delete:

```markdown
## Issue Deleted

**TEAM-123**: Issue title here

The issue has been permanently deleted. This action cannot be undone.
```
