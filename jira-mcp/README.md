# Jira MCP Skill

The **Jira MCP** skill allows you to query and manage Jira Kanban board issues using natural language, exclusively using the Atlassian MCP backend and respecting custom field rules, approvals, and security.

It can search, view, create, and update issues, check transitions, change status, add comments, locate accounts for assignment, and list visible projects. Before any change, the skill checks the current state and requests user approval.

## Table of Contents

- [What the skill does](#what-the-skill-does)
- [Prerequisites](#prerequisites)
- [How to use](#how-to-use)
- [MCP operations and tools](#mcp-operations-and-tools)
- [Issue identification](#issue-identification)
- [Search with JQL](#search-with-jql)
- [Issue creation](#issue-creation)
- [Custom fields & Language Enforcement](#custom-fields--language-enforcement)
- [Issue update](#issue-update)
- [Status transitions](#status-transitions)
- [Comments and assignment](#comments-and-assignment)
- [ADF format](#adf-format)
- [Security rules](#security-rules)
- [Limitations](#limitations)
- [Troubleshooting](#troubleshooting)
- [Skill references](#skill-references)

## What the skill does

The skill offers interaction with Jira to:

- search for issues using JQL;
- query an issue by its key;
- list user issues, backlog, or other requested filters;
- create Tasks, Subtasks, Stories, Epics, Bugs, and other types available in the project;
- fill in standard fields and custom fields;
- update the title, assignee, and other fields of an existing issue;
- query the available transitions in the current state;
- move an issue between valid workflow states;
- add comments;
- locate a person's account ID before assigning them;
- list visible Jira projects;
- query issue types and required fields for a project;
- explain Atlassian MCP configuration or authentication problems.

The skill is optimized for Kanban boards and does not use command-line interface commands to operate Jira.

## Prerequisites

The Atlassian MCP must be configured, authenticated, and active in the environment. The configuration needs to provide access to Jira and the necessary credentials, such as URL, email, and API token.

Do not include tokens, passwords, or other credentials in prompts, examples, cards, or comments. Credentials must remain in the secure MCP configuration.

The connected account's permissions determine which projects, issues, fields, and transitions will be available.

**Important:** Your personal Atlassian account language *must* be set to English. (See [Language Enforcement](#custom-fields--language-enforcement)).

## How to use

Activate the skill with `$jira-mcp` and describe the desired operation:

```text
Use $jira-mcp to show the issue PROJ-123.
```

The skill can also be triggered by requests that mention Jira, issue, ticket, Kanban, board, or a key in the `PROJ-123` pattern.

### Request examples

Search for issues:

```text
Use $jira-mcp to list my open, high-priority issues in the PROJ project.
```

Create a Task:

```text
Use $jira-mcp to create a Task in the PROJ project with the title
"Configure development environment" and Change Level Structural. Also include What, Why, and Acceptance Criteria.
```

Update an issue:

```text
Use $jira-mcp to update the Acceptance Criteria field of issue PROJ-123.
Show the current value and the proposed change before applying.
```

Change the status:

```text
Use $jira-mcp to move PROJ-123 to Done, if this transition is available.
```

## MCP operations and tools

The skill selects the Atlassian MCP tool corresponding to the intent:

| Intent | MCP Tool |
| --- | --- |
| Search issues with JQL | `mcp__atlassian__searchJiraIssuesUsingJql` |
| Query an issue | `mcp__atlassian__getJiraIssue` |
| Create an issue | `mcp__atlassian__createJiraIssue` |
| Update an issue | `mcp__atlassian__editJiraIssue` |
| Query available transitions | `mcp__atlassian__getTransitionsForJiraIssue` |
| Execute a transition | `mcp__atlassian__transitionJiraIssue` |
| Add comment | `mcp__atlassian__addCommentToJiraIssue` |
| Locate account ID | `mcp__atlassian__lookupJiraAccountId` |
| List visible projects | `mcp__atlassian__getVisibleJiraProjects` |
| Query project types and fields | `mcp__atlassian__getJiraProjectIssueTypesMetadata` |

Before executing a tool that modifies data, the skill shows the proposed operation and asks for explicit approval.

## Issue identification

Issue keys follow the pattern:

```text
[A-Z]+-[0-9]+
```

Valid examples include `PROJ-123` and `ABC-1`. When a key appears in the conversation, the skill queries the corresponding issue with `mcp__atlassian__getJiraIssue` to get the current state, rather than relying solely on information provided by the user.

## Search with JQL

The skill uses `mcp__atlassian__searchJiraIssuesUsingJql` for searches that require filters. The basic syntax is:

```jql
field operator value [AND|OR field operator value]
```

### Query examples

Open and high-priority issues assigned to the current user:

```jql
assignee = currentUser() AND status NOT IN (Done, Closed) AND priority >= High
```

## Issue creation

The creation flow is:

1. research the context when the request mentions code, other issues, or pull requests;
2. identify the project, issue type, and required fields;
3. apply specific rules for the issue type;
4. prepare the content and present it to the user;
5. request approval;
6. create the issue with `mcp__atlassian__createJiraIssue`;
7. verify the result after creation.

### Tasks and Subtasks (Strictly Required Fields)

For Tasks and Subtasks, the fields **Change Level**, **What?**, **Why?**, and **Acceptance Criteria** are strictly mandatory.

The accepted values for Change Level are: `Trivial`, `Operational`, `Functional`, `Structural`, and `Emergency`.

The skill never infers these values. If the creation of a Task or Subtask is requested without *any* of these fields, it interrupts the flow and asks the user to provide the missing information.

### Epics and Stories

For Epics and Stories:
- The **Change Level** field must **NOT** be used.
- The fields **What?**, **Why?**, and **Acceptance Criteria** are **OPTIONAL**. If not provided in the request, the skill simply omits them without asking.

## Custom fields & Language Enforcement

### Language Enforcement (CRITICAL TRAP)

The Jira API returns field names translated based on the user's personal account preferences. Because the skill relies on English field names to identify custom fields, it constantly monitors the payload. 

If the skill detects field names in a language other than English (e.g., Portuguese terms like "Nível de Mudança", "O que?", "Por quê?", "Critérios de Aceitação"), it will **STOP IMMEDIATELY**. It will not attempt to proceed or map the fields, and will instruct the user to change their Atlassian Account Settings language preference to 'English (US)' or 'English (UK)'.

### Custom Fields Mapping

The official Jira field names always remain in English. Fields known by the skill:

| Official name | ID | Type | Rule |
| --- | --- | --- | --- |
| **Acceptance Criteria** | `customfield_10044` | Rich Text | Mandatory for Tasks/Subtasks; Optional for Epics/Stories |
| **Change Level** | `customfield_10042` | Single-choice dropdown | Mandatory for Tasks/Subtasks; Forbidden for Epics/Stories |
| **What?** | `customfield_10045` | Rich Text | Mandatory for Tasks/Subtasks; Optional for Epics/Stories |
| **Why?** | `customfield_10043` | Rich Text | Mandatory for Tasks/Subtasks; Optional for Epics/Stories |

When the user provides another custom field not hardcoded above, the skill locates its ID using a regex match (`^customfield_\d+$`) before interacting with it.

## Issue update

Before changing an issue, the skill:

1. queries the issue with `mcp__atlassian__getJiraIssue`;
2. preserves and presents the relevant original content;
3. shows the comparison between the current state and the proposed change;
4. requests explicit approval;
5. executes `mcp__atlassian__editJiraIssue`;
6. verifies the result.

The description is never changed without showing the original content. Bulk edits require explicit approval.

## Status transitions

Transition IDs and names are not fixed. They depend on the project, workflow, issue type, and current state.

To change a status, the skill:

1. queries the issue and its current status;
2. calls `mcp__atlassian__getTransitionsForJiraIssue`;
3. identifies an available transition that matches the user's goal;
4. shows the current state and the proposed transition;
5. obtains approval;
6. calls `mcp__atlassian__transitionJiraIssue` with the returned ID.

## Comments and assignment

### Comments
Comments are added with `mcp__atlassian__addCommentToJiraIssue`. Before adding a comment, the skill shows the text and asks for approval.

### Assignment
The MCP requires an account ID for assignment. The skill never sends just the display name. It uses `mcp__atlassian__lookupJiraAccountId` to find the correct ID first.

## ADF format

Rich Text fields may require Atlassian Document Format (ADF) when Jira rejects plain text. Conversion to ADF is used when necessary for Rich Text fields, preserving the content approved by the user.

## Security rules

Before any operation, the skill evaluates the current state, affected users/issues, and reversibility.

Mandatory rules:
- exclusively use Atlassian MCP tools, never CLI commands;
- show the proposed operation or call before execution and obtain approval;
- query the current state and available transitions before modifying an issue;
- preserve and show original information before editing descriptions;
- clearly report authentication problems or unavailability.

## Limitations

- The Atlassian MCP does not offer issue link creation in this skill.
- Operations cannot be performed when the Atlassian MCP backend is unavailable.
- Display names cannot be used directly for assignment; it is necessary to obtain the account ID.

## Troubleshooting

If the Atlassian MCP tools are not available or fail to load, confirm that the Atlassian MCP server is configured and running, and that credentials are provided in the secure configuration. The skill will expose the authentication or configuration error so the user can fix it.

## Skill references

The operational instructions are in `SKILL.md`.
The detailed reference for tools, JQL, payloads, ADF, and workflows is in `references/mcp.md`.