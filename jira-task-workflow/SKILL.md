---
name: jira-task-workflow
description: Manage the complete Jira Task and Subtask lifecycle through Jira MCP tools, including finding and reading work, creating issues from project metadata, editing fields, adding comments, assigning users, and performing workflow transitions. Use when a user asks to create, inspect, update, comment on, assign, start, close, or otherwise transition a Jira Task or Subtask.
---

# Jira Task Workflow

Manage Jira Tasks and Subtasks through the Jira MCP backend. Discover project-specific fields and transitions at runtime, preview every write, and verify the resulting state.

## Scope

- Operate only on issue types identified by Jira metadata as Task or Subtask.
- Refuse to create or modify Bugs, Stories, Epics, or other issue types and explain that they are outside this skill's scope.
- Respond in the user's language. Preserve Jira issue keys, status names, field names, and other server-provided identifiers exactly.
- Use Jira MCP tools only. Never substitute CLI commands or direct HTTP requests.

## Core Tools

| Intent | Tool |
| --- | --- |
| Resolve the Jira site | `mcp__jira__getAccessibleAtlassianResources` |
| Find projects and issue types | `mcp__jira__getVisibleJiraProjects`, `mcp__jira__getJiraProjectIssueTypesMetadata` |
| Discover fields | `mcp__jira__getJiraIssueTypeMetaWithFields` |
| Search work | `mcp__jira__search`, `mcp__jira__searchJiraIssuesUsingJql` |
| Read an issue | `mcp__jira__getJiraIssue` |
| Create a Task or Subtask | `mcp__jira__createJiraIssue` |
| Edit fields | `mcp__jira__editJiraIssue` |
| Resolve an assignee | `mcp__jira__lookupJiraAccountId` |
| Read and apply transitions | `mcp__jira__getTransitionsForJiraIssue`, `mcp__jira__transitionJiraIssue` |
| Add a comment | `mcp__jira__addCommentToJiraIssue` |

Use `mcp__jira__search` for natural-language discovery. Use `mcp__jira__searchJiraIssuesUsingJql` when the user provides JQL or explicitly needs a deterministic JQL filter.

## Safety Contract

For every write operation:

1. Fetch the current issue or relevant project metadata.
2. Show the exact proposed content and identify the target project or issue.
3. Ask for explicit approval.
4. Treat the approval as authorizing only the displayed operation. Ask again after any material change.
5. Execute one write and refetch the result to verify it.

Do not request approval for read-only searches, metadata lookup, or issue inspection. Never bulk-modify issues without showing every target and receiving explicit approval for the batch.

If a write times out or returns an ambiguous result, inspect Jira before retrying. Do not risk creating a duplicate Task, duplicate comment, or repeated transition.

## Resolve Context

1. Resolve `cloudId`. If the user supplied an Atlassian URL, try its hostname as documented by the tool; otherwise list accessible resources. Ask the user to choose only when multiple sites remain plausible.
2. Resolve the project from the request or visible-project metadata. Never guess between multiple matching projects.
3. When an issue key is present, fetch it and verify that its Jira-reported type is Task or Subtask before continuing.
4. Resolve people with `mcp__jira__lookupJiraAccountId`; never assign by display name alone. Ask the user to choose if the lookup is ambiguous.

## Create a Task or Subtask

1. Resolve the project and call `mcp__jira__getJiraProjectIssueTypesMetadata` to obtain the server's Task or Subtask issue type and its ID.
2. Call `mcp__jira__getJiraIssueTypeMetaWithFields` with `requiredFieldsOnly: true`. Follow pagination until all required fields are known.
3. If the user supplied optional or custom fields, call the same tool with `requiredFieldsOnly: false` and resolve each field from returned metadata. Never hardcode or invent custom-field IDs, allowed values, or payload shapes.
4. Collect only missing required values. Validate selections against metadata-provided allowed values.
5. For a Subtask, require a parent key. Fetch the parent and use metadata and Jira validation to confirm that the parent relationship is valid; never invent a parent.
6. Build the creation preview with project, issue type, summary, parent when applicable, assignee, description, and additional fields.
7. Obtain approval, call `mcp__jira__createJiraIssue`, then fetch the returned issue key and report the verified result.

Pass fields not represented by dedicated tool parameters through `additional_fields`, using the identifiers and shapes returned by metadata.

## Read and Search

- Fetch a mentioned issue key directly with `mcp__jira__getJiraIssue`.
- Use natural-language search for discovery and JQL only under the tool-selection rule above.
- Paginate when the requested result set is larger than one response.
- Clearly distinguish Jira-reported values from summaries or inferences.

## Edit, Assign, or Comment

1. Fetch the issue and reject it if it is not a Task or Subtask.
2. Fetch field metadata when resolving a custom field, its ID, or allowed values.
3. Preserve unrelated fields. Show a current-versus-proposed comparison for every edit.
4. Resolve a new assignee to an account ID before presenting the preview.
5. Preview comments verbatim, including the target issue.
6. Obtain approval, execute the edit or comment, and refetch the issue to verify the result.

Never overwrite a description without showing its current value. Clear a field only when the preview explicitly shows that it will become empty or `null`.

## Transition or Close Work

1. Fetch the issue and verify its type and current status.
2. Call `mcp__jira__getTransitionsForJiraIssue` immediately before the proposed transition.
3. Match only an available server-returned transition. Never hardcode a transition ID or assume that statuses such as `In Progress` or `Done` exist.
4. If the requested destination requires intermediate transitions, show the complete sequence but request approval before each transition.
5. Execute the approved transition by ID, refetch the issue, and confirm the new status.

Treat "close", "complete", and similar requests as intent, not as literal status names. If no available transition satisfies the intent, report the current status and available choices without writing.

## Failure Handling

- If Jira MCP is unavailable or unauthenticated, stop and tell the user to configure or reconnect the Jira integration.
- If metadata is missing, localized, inconsistent, or paginated, inspect the returned identifiers and continue only when the required field and payload shape are unambiguous.
- If Jira rejects a value, surface the error and refresh metadata before proposing a correction. Do not silently drop required or user-provided fields.
- If permissions block an operation, report the denied operation and leave the issue unchanged.
- If verification differs from the approved proposal, report the discrepancy and do not make compensating changes without new approval.
