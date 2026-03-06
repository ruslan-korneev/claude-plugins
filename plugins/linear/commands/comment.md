---
name: "linear:comment"
description: Edit or delete comments on a Linear issue (create/list via Linear MCP)
allowed_tools:
  - Bash
  - Read
  - Grep
arguments:
  - name: id
    description: "Issue identifier (e.g., TEAM-123)"
    required: true
  - name: action
    description: "Action: edit, delete"
    required: true
  - name: text
    description: "New comment text in markdown (for edit)"
    required: false
  - name: comment-id
    description: "Comment UUID (required for edit/delete)"
    required: true
---

# Command /linear:comment

Edit or delete comments on a Linear issue.

> For **listing** and **adding** comments, use the Linear MCP tools (`list_comments`, `create_comment`).

## Instructions

### Step 0: Validate Configuration

Check `LINEAR_API_KEY` and `LINEAR_TEAM` are set.

### Step 1: Resolve Issue

Parse `id` to extract team key and number. Fetch issue UUID using `issues` query with filter.

### Step 2: Execute Action

#### action=edit

Requires `comment-id` and `text`. Use `commentUpdate` mutation:

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

Variables:

```json
{
  "id": "comment-uuid",
  "input": { "body": "Updated text" }
}
```

Display confirmation with updated comment preview.

#### action=delete

Requires `comment-id`. Use `commentDelete` mutation:

```graphql
mutation CommentDelete($id: String!) {
  commentDelete(id: $id) {
    success
  }
}
```

Display confirmation of deletion.
