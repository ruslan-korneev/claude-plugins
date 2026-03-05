---
name: "linear:comment"
description: Manage comments on a Linear issue
allowed_tools:
  - Bash
  - Read
  - Grep
arguments:
  - name: id
    description: "Issue identifier (e.g., TEAM-123)"
    required: true
  - name: action
    description: "Action: list, add, edit, delete (default: list)"
    required: false
  - name: text
    description: "Comment text in markdown (for add/edit)"
    required: false
  - name: comment-id
    description: "Comment UUID (for edit/delete)"
    required: false
---

# Command /linear:comment

Manage comments on a Linear issue.

## Instructions

### Step 0: Validate Configuration

Check `LINEAR_API_KEY` and `LINEAR_TEAM` are set.

### Step 1: Resolve Issue

Parse `id` to extract team key and number. Fetch issue UUID.

### Step 2: Execute Action

#### action=list (default)

Fetch issue with comments using `issue` query. Display:

```markdown
## Comments on TEAM-123: Issue Title

### John Doe (2026-03-03 14:30)

Comment text here with **markdown** support.

---

### Jane Smith (2026-03-05 09:15)

Another comment here.

---

*2 comments total*
```

If no comments, display "No comments on this issue."

#### action=add

Requires `text` argument. Use `commentCreate` mutation from [graphql-queries.md](../skills/linear-api/references/graphql-queries.md):

```json
{
  "issueId": "issue-uuid",
  "body": "Comment text"
}
```

Display confirmation with comment preview.

#### action=edit

Requires `comment-id` and `text`. Use `commentUpdate` mutation:

```json
{
  "id": "comment-uuid",
  "input": { "body": "Updated text" }
}
```

#### action=delete

Requires `comment-id`. Use `commentDelete` mutation.

Display confirmation of deletion.
