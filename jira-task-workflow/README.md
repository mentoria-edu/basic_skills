# Jira Task Workflow Skill

`jira-task-workflow` manages the complete lifecycle of Jira Tasks and Subtasks through Jira MCP tools. It supports searching and reading work, metadata-driven creation, field updates, assignment, comments, and workflow transitions.

Every Jira write is previewed for explicit approval and verified after execution.

## Prerequisites

- A configured and authenticated Jira MCP integration.
- Access to the target Jira site and project.
- Permission to read or modify the requested Tasks and Subtasks.

Credentials must remain in the integration configuration. Do not include API tokens, passwords, or other secrets in prompts or issue content.

## Usage

Invoke the skill explicitly with `$jira-task-workflow`:

```text
Use $jira-task-workflow to create a Task in project PROJ for documenting the deployment process.
```

```text
Use $jira-task-workflow to create a Subtask under PROJ-123 for adding integration tests.
```

```text
Use $jira-task-workflow to assign PROJ-456 to Alex and move it to the appropriate in-progress status.
```

```text
Use $jira-task-workflow to close PROJ-789 if an appropriate transition is available.
```

The skill also activates for requests to inspect, update, comment on, assign, start, or close a Jira Task or Subtask.

## Workflow

For creation, the skill:

1. resolves the Jira site, project, and issue type;
2. retrieves required and requested field metadata;
3. asks only for missing required information;
4. validates custom values and the parent of a Subtask;
5. shows the complete creation preview;
6. requests approval, creates the issue, and verifies it.

For an existing issue, it first reads the current state and confirms that the issue is a Task or Subtask. Edits show a before-and-after comparison. Transitions are selected from Jira's currently available transitions rather than from hardcoded status names or IDs.

## Approval and Safety

- Read-only searches and metadata lookup do not require approval.
- Creating, editing, assigning, commenting, and transitioning always require explicit approval.
- One approval covers only the displayed write operation.
- Intermediate workflow transitions require separate approvals.
- Ambiguous write results are checked before any retry to avoid duplicates.
- Bulk changes require the full target list and explicit approval.

## Dynamic Jira Configuration

The skill discovers required and custom fields from project metadata. It does not hardcode custom-field IDs, allowed values, payload formats, or transition IDs. This allows it to follow each project's Jira configuration.

Assignees are resolved to Jira account IDs before assignment. Subtasks require an existing, valid parent issue.

## Scope and Limitations

The skill operates only on Tasks and Subtasks. It refuses to create or modify Bugs, Stories, Epics, and other issue types.

Available fields, issue types, transitions, and operations depend on the connected account's permissions and the target project's configuration. Issue linking, worklogs, sprint management, and administration are outside this skill's scope.

## Troubleshooting

- **No Jira tools are available:** configure or reconnect the Jira MCP integration.
- **Multiple Jira sites or projects match:** provide the site URL or exact project key.
- **A required field is missing:** provide the value requested from Jira's project metadata.
- **An assignee is ambiguous:** provide an email address or another unique identifier.
- **A transition is unavailable:** review the current status and the available transitions reported by Jira.
- **A write is denied:** confirm that the connected account has permission for that operation.
