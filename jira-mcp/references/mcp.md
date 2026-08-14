# MCP Reference

Complete reference for Atlassian Jira operations via MCP, customized for Kanban and custom fields.

## MCP Tool Reference

### Search Operations

#### `mcp__atlassian__searchJiraIssuesUsingJql`
Search Jira using JQL (Jira Query Language).

**Parameters:**
- `jql` (required): JQL query string
- `maxResults`: Maximum results (default: 50)
- `startAt`: Pagination offset
- `fields`: Comma-separated fields to return

**Example:**
```
mcp__atlassian__searchJiraIssuesUsingJql(jql: "project = PROJ AND status = 'In Progress'")
```

### Issue Operations

#### `mcp__atlassian__getJiraIssue`
Retrieve full issue details by key.

**Parameters:**
- `issueKey` (required): Issue key (e.g., "PROJ-123")
- `expand`: Additional data (changelog, transitions, renderedFields)

**Example:**
```
mcp__atlassian__getJiraIssue(issueKey: "PROJ-123")
```

#### `mcp__atlassian__createJiraIssue`
Create a new issue. Standard description fields are NOT used.

**Parameters:**
- `projectKey` (required): Target project
- `issueType` (required): Issue type (Story, Bug, Task, Epic, etc.)
- `summary` (required): Issue title
- `assignee`: Account ID (use lookupJiraAccountId first)
- `priority`: Priority name (Highest, High, Medium, Low, Lowest)
- `labels`: Array of labels
- `components`: Array of component names
- `customfield_10042`: Change Level (REQUIRED for Tasks/Subtasks, do NOT use for Epics/Stories). Format: `{"value": "Level"}`
- `customfield_10045`: What? (Rich text / ADF)
- `customfield_10043`: Why? (Rich text / ADF)
- `customfield_10044`: Acceptance Criteria (Rich text / ADF)

#### `mcp__atlassian__editJiraIssue`
Update an existing issue.

**Parameters:**
- `issueKey` (required): Issue to update
- Any field to update (summary, assignee, custom fields, etc.)

**Example:**
```
mcp__atlassian__editJiraIssue(
  issueKey: "PROJ-123",
  customfield_10045: "Updated What? description with more details..."
)
```

### Transition Operations

#### `mcp__atlassian__getTransitionsForJiraIssue`
Get available status transitions for an issue.

**Parameters:**
- `issueKey` (required): Issue key

**Returns:** List of available transitions with IDs and names.

#### `mcp__atlassian__transitionJiraIssue`
Change issue status.

**Parameters:**
- `issueKey` (required): Issue key
- `transitionId` (required): Transition ID from getTransitions
- `comment`: Optional comment for the transition

### Comment Operations

#### `mcp__atlassian__addCommentToJiraIssue`
Add a comment to an issue.

**Parameters:**
- `issueKey` (required): Issue key
- `body` (required): Comment text (supports Jira markdown)

### User Operations

#### `mcp__atlassian__lookupJiraAccountId`
Find user account ID for assignments.

**Parameters:**
- `query` (required): Search by display name, email, or username

**Example:**
```
mcp__atlassian__lookupJiraAccountId(query: "user@example.com")
```

### Project Operations

#### `mcp__atlassian__getVisibleJiraProjects`
List available Jira projects.

#### `mcp__atlassian__getJiraProjectIssueTypesMetadata`
Get issue types and required fields for a project.

---

## JQL (Jira Query Language) Reference

### Basic Syntax

```
field operator value [AND|OR field operator value]
```

### Common Fields

| Field | Description | Example |
|-------|-------------|---------|
| `project` | Project key | `project = "PROJ"` |
| `issuetype` | Issue type | `issuetype = Bug` |
| `status` | Issue status | `status = "In Progress"` |
| `assignee` | Assigned user | `assignee = currentUser()` |
| `reporter` | Issue creator | `reporter = "jobarksdale"` |
| `priority` | Priority level | `priority = High` |
| `labels` | Issue labels | `labels = "backend"` |
| `created` | Creation date | `created >= -30d` |
| `updated` | Last update | `updated >= -7d` |

### Complex Query Examples

```jql
# My open issues, high priority
assignee = currentUser() AND status NOT IN (Done, Closed) AND priority >= High

# Bugs created this week
issuetype = Bug AND created >= startOfWeek() ORDER BY priority DESC

# Epics in progress with stories
issuetype = Epic AND status = "In Progress" AND issueFunction in hasLinks("is parent of")

# Issues updated by me recently
updatedBy = currentUser() AND updated >= -7d ORDER BY updated DESC

# Blocked issues
status = Blocked OR "Flagged" = "Impediment"
```

---

## Issue Linking

### Limitation
The Atlassian MCP does not currently support creating issue links. 

### Link Types
| Link Type | Inward | Outward | Use Case |
|-----------|--------|---------|----------|
| Depends On | is dependency of | depends on | Task dependencies |
| Blocks | is blocked by | blocks | Blocking relationships |
| Relates To | relates to | relates to | General relationships |
| Clones | is cloned by | clones | Cloned issues |
| Duplicates | is duplicated by | duplicates | Duplicate issues |

---

## Description Formatting

### Atlassian Document Format (ADF)

For `createJiraIssue`, rich text custom fields (What?, Why?, Acceptance Criteria) may require ADF format if plain text is rejected:
```json
{
  "type": "doc",
  "version": 1,
  "content": [
    {
      "type": "paragraph",
      "content": [
        {"type": "text", "text": "Content goes here..."}
      ]
    }
  ]
}
```

---

## Common Workflows

### Create a Task or Subtask (Requires Change Level)

```
1. Lookup User ID (if assigning):
   mcp__atlassian__lookupJiraAccountId(query: "john@example.com")

2. Create Issue:
   mcp__atlassian__createJiraIssue(
     projectKey: "PROJ",
     issueType: "Task",
     summary: "Configurar ambiente de desenvolvimento",
     assignee: "account_id_from_step_1",
     customfield_10042: {"value": "Structural"},
     customfield_10045: "Preparar variáveis de ambiente e docker-compose.",
     customfield_10043: "Preparar o ambiente necessário para o desenvolvimento da aplicação.",
     customfield_10044: "- O container sobe sem erros\n- As variáveis de ambiente estão documentadas"
   )
```

### Create an Epic or Story (NO Change Level)

```
mcp__atlassian__createJiraIssue(
  projectKey: "PROJ",
  issueType: "Epic",
  summary: "Desenvolver uma calculadora simples",
  customfield_10045: "Criar as operações básicas de matemática (soma, subtração, divisão, multiplicação).",
  customfield_10043: "Permitir que o usuário realize cálculos rápidos na interface principal.",
  customfield_10044: "- Botões de 0 a 9 funcionais\n- Operações retornam o valor correto"
)
```

### Move Ticket to Done

```
1. Get available transitions:
   mcp__atlassian__getTransitionsForJiraIssue(issueKey: "PROJ-123")
   → Returns list with transition IDs

2. Find the appropriate transition ID from the response (e.g., "31" for Done).

3. Execute transition:
   mcp__atlassian__transitionJiraIssue(
     issueKey: "PROJ-123",
     transitionId: "31"
   )
```
