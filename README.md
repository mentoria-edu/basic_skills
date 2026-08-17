# Basic Skills

This repository centralizes reusable Codex skills for anyone who wants to adopt a consistent development standard.

## Index

- [Quickstart](#quickstart)
- [Usage](#usage)
- [Project Structure](#project-structure)
- [Contribution Guidelines](#contribution-guidelines)
- [Versioning and Releases](#versioning-and-releases)

## Quickstart

### Prerequisites

- Git
- Codex with support for installing and running local skills
- Read access to this public repository

### Installation

Clone the repository:

```bash
git clone https://github.com/mentoria-edu/basic_skills.git
cd basic_skills
```

Then ask Codex to install the desired skill from the cloned repository. You can identify a skill by its exact name, use keywords from its title, or ask Codex to recommend the most appropriate skill for your task.

For example:

```text
Install the c4-model skill from this cloned repository.
```

### Minimal Execution Example

After installing `c4-model`, submit a prompt that provides the requirements, project stack, and objective:

```text
Using the requirements I collected below and the project's stack (describe the stack),
with the goal of (describe the objective), create an example flow using $c4-model.
```

The skill returns a C4 architecture diagram in Mermaid syntax at the abstraction level that best matches the request.

### Running Tests

The repository uses manual behavioral validation instead of an automated test command. Before a skill is accepted:

1. Exercise every capability described in its `SKILL.md`.
2. Confirm that the observed behavior and output match the skill's documented contract.
3. Submit the skill through the required pull request review and approval workflow.

## Usage

### Supported Use Cases

#### Create or review C4 diagrams

Use `c4-model` when a request contains system requirements, a project stack, and an architectural objective. The skill accepts those details as input and produces or reviews a Mermaid C4 Context, Container, or Component diagram. It can use a Mermaid flowchart when C4 syntax is unavailable.

Example invocation:

```text
Use $c4-model to create a C4 Context diagram from these requirements: ...
```

#### Create or audit repository READMEs

Use `readme-creator` with repository files and project governance information as input. The skill inspects the available evidence, collects missing mandatory facts, and creates, updates, or audits an English `README.md` against its bundled standard.

Example invocation:

```text
Use $readme-creator to create the README for this repository.
```

## Project Structure

```text
.
├── .github/
│   ├── CODEOWNERS                 # Repository ownership configuration placeholder
│   └── workflows/
│       └── notify.yml             # Optional Discord notification for opened pull requests
├── c4-model/
│   └── SKILL.md                   # C4 Model diagram skill instructions
├── readme-creator/
│   ├── agents/
│   │   └── openai.yaml            # Codex-facing skill interface metadata
│   ├── references/
│   │   └── readme_standard.md     # Normative README structure and rules
│   └── SKILL.md                   # README creation and audit workflow
└── README.md
```

Each skill lives in its own top-level directory. Skills created with Codex must contain both `SKILL.md` and `agents/openai.yaml`; supporting references may be added when the skill requires them.

## Contribution Guidelines

### Pull Request Workflow

1. Clone the repository and create a branch for the skill addition or change.
2. Name the branch using `feat/<verb>_<skill-description>`, such as `feat/create_readme`.
3. Implement the skill and complete its manual behavioral validation.
4. Open a pull request targeting `main` and describe the change and validation performed.
5. Obtain approval from two reviewers. At least one approver must be a current repository owner; the owner's individual name is intentionally not fixed in this document.
6. Merge only after both required approvals are recorded.

When the `DISCORD_WEBHOOK` repository secret is configured, the existing GitHub Actions workflow sends a Discord notification when a pull request is opened. This notification is optional and is not a quality check.

### Code Quality Standards

Every skill created with Codex must preserve the required directory architecture:

```text
skill-name/
├── agents/
│   └── openai.yaml
└── SKILL.md
```

Reviewers must verify that:

- The required directory and both required files are present in the pull request.
- `SKILL.md` describes the skill's supported behavior and expected output.
- Manual validation covers every capability documented by the skill.

## Versioning and Releases

The repository's target versioning strategy is Semantic Versioning. The release mechanism is currently provisional and has not yet been initialized in the repository.

For each release under the provisional process:

1. Clone the repository and prepare the release change through the standard pull request workflow.
2. Record the release version in a `VERSION` file as the source of the current version.
3. After the pull request is approved and merged, create a matching Git tag using the `v<major>.<minor>.<patch>` format.

The `VERSION` file is intentionally not present yet because this repository is currently being used to validate the README creation workflow.

During the provisional process, a pull request containing a breaking change must identify the incompatibility and provide both its impact and migration instructions. Once Semantic Versioning is formally activated, breaking changes must increment the major version and be communicated in the release notes.
