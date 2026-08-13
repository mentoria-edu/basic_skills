---
name: jira-mcp
description: Manage Jira issues via Atlassian MCP on a Kanban board. Handles custom fields, specific ticket creation rules, and dynamic workflows. Triggers on keywords like "jira", "issue", "ticket", "kanban", "board", or issue key patterns.
---

# Jira MCP

Natural language interaction with Jira exclusively through the Atlassian MCP backend. Optimized for Kanban boards.

## Quick Reference (MCP)

| Intent | MCP Tool |
|--------|----------|
| Search issues | `mcp__atlassian__searchJiraIssuesUsingJql` |
| View issue | `mcp__atlassian__getJiraIssue` |
| Create issue | `mcp__atlassian__createJiraIssue` |
| Update issue | `mcp__atlassian__editJiraIssue` |
| Get transitions | `mcp__atlassian__getTransitionsForJiraIssue` |
| Transition | `mcp__atlassian__transitionJiraIssue` |
| Add comment | `mcp__atlassian__addCommentToJiraIssue` |
| User lookup | `mcp__atlassian__lookupJiraAccountId` |
| List projects | `mcp__atlassian__getVisibleJiraProjects` |

## Triggers

- "create a jira ticket"
- "show me PROJ-123"
- "list my tickets"
- "move ticket to done"
- "what is in the backlog"

## Issue Key Detection

Issue keys follow the pattern: `[A-Z]+-[0-9]+` (e.g., PROJ-123, ABC-1).
When a user mentions an issue key in conversation, use `mcp__atlassian__getJiraIssue` with the key.

## Custom Fields & Localization Rules (CRITICAL)

**1. Language Enforcement (CRITICAL TRAP):**
- The Jira API returns field names translated based on the user's personal account preferences.
- When fetching issue details (`mcp__atlassian__getJiraIssue`) or metadata (`mcp__atlassian__getJiraProjectIssueTypesMetadata`), **ALWAYS inspect the field names in the payload**.
- If you detect field names in a language other than English (e.g., Portuguese terms like "Nível de Mudança", "O que?", "Por quê?", "Critérios de Aceitação"), **STOP IMMEDIATELY**.
- Do NOT attempt to proceed, map, or translate the fields yourself.
- Reply to the user instructing them to fix their language:

```text
I detected that your Jira account language is not set to English. To use this integration correctly, please go to your Atlassian Account Settings, change your language preference to 'English (US)' or 'English (UK)', and then we can try again.
```

**2. Hardcoded Field IDs:**
When working with these specific fields, use the mapped IDs directly to save time and tokens:
- **Acceptance Criteria** -> `customfield_10044` (Rich Text)
- **Change Level** -> `customfield_10042` (Single-choice dropdown: Trivial, Operational, Functional, Structural, Emergency)
- **What?** -> `customfield_10045` (Rich Text)
- **Why?** -> `customfield_10043` (Rich Text)

**3. Creation Rules:**
- **Tasks & Subtasks (REQUIRED Fields):** The fields `Change Level` (`customfield_10042`), `What?` (`customfield_10045`), `Why?` (`customfield_10043`), and `Acceptance Criteria` (`customfield_10044`) are strictly **REQUIRED**.
  - Valid `Change Level` options: `Trivial`, `Operational`, `Functional`, `Structural`, `Emergency`.
  - **Do NOT infer or guess** these values. If the user requests a task/subtask creation but omits any of these fields, **STOP** and ask the user to provide the missing information before proceeding.
- **Epics & Stories:**
  - Do **NOT** use the `Change Level` field.
  - The fields `What?`, `Why?`, and `Acceptance Criteria` are **OPTIONAL**. If the user does not provide them in their request, simply skip them. Do NOT ask the user to provide them.

**4. Dynamic Field Resolution:**
If the user provides a field name that is not hardcoded above (or if the ID is missing), you must find its ID first before interacting with it. All custom field IDs must match the regex `^customfield_\d+$`.

## Workflow & Transitions

**Creating tickets:**
1. Research context if user references code/tickets/PRs.
2. Check required fields based on the issue type (enforce Change Level for Tasks/Subtasks).
3. Draft ticket content and review with user.
4. Create using `mcp__atlassian__createJiraIssue`.

**Updating & Transitioning tickets:**
1. Fetch issue details first (`mcp__atlassian__getJiraIssue`).
2. If transitioning, **ALWAYS** get available transitions first (`mcp__atlassian__getTransitionsForJiraIssue`). Do not hardcode transition IDs or assume names like "Done" or "In Progress" are universally standard or available from the current state.
3. Show current vs proposed changes.
4. Get approval before updating.
5. Add comment explaining changes.

## Before Any Operation

Ask yourself:
1. **What's the current state?** — Always fetch the issue first. Don't assume status, assignee, or fields are what user thinks they are.
2. **Who else is affected?** — Check watchers, linked issues, parent epics.
3. **Is this reversible?** — Transitions may have one-way gates. Some workflows require intermediate states.
4. **Do I have the right identifiers?** — Issue keys, transition IDs, account IDs. Display names don't work for assignment (MCP).

## NEVER

- **NEVER use CLI commands.** This environment relies strictly on Atlassian MCP tools.
- **NEVER transition without fetching current status & transitions.** Workflows require specific intermediate states.
- **NEVER assign using display name.** Only account IDs work. Always call `lookupJiraAccountId` first.
- **NEVER edit description without showing original.** Jira has no undo.
- **NEVER bulk-modify without explicit approval.** Each ticket change notifies watchers.

## Safety

- Always show the command/tool call before running it
- Always get approval before modifying tickets
- Preserve original information when editing
- Verify updates after applying
- Always surface authentication issues clearly so the user can resolve them

## No Backend Available

If the Atlassian MCP tools are not available or failing to load, guide the user to check their configuration:

```text
To use Jira in this environment, the **Atlassian MCP** must be configured and active.
Please verify your MCP settings and ensure your Atlassian credentials (Jira URL, email, and API token) are properly provided and that the MCP server is running.
```

## Deep Dive

**LOAD reference when:**
- Building JQL queries beyond simple filters
- Troubleshooting errors or authentication issues
- Working with transitions, linking, or specific MCP payloads

**Do NOT load reference for:**
- Simple view/list operations (Quick Reference above is sufficient)
- Basic status checks (`mcp__atlassian__getJiraIssue`)
- Ticket creation (Rules and custom fields are handled dynamically)

| Task | Load Reference? |
|------|-----------------|
| View single issue | No |
| List my tickets | No |
| Create Task/Epic | No |
| Transition issue | **Yes** — need transition workflow |
| JQL search | **Yes** — for complex queries |

References:
- MCP patterns and JQL: `references/mcp.md`
