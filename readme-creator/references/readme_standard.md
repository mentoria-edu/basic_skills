# Standardized Repository README

This reference defines the normative README structure for libraries, applications, frameworks,
and multi-persona platforms.

## Contents

- [Governing rules](#governing-rules)
- [Required structure](#required-structure)
- [Repository title and summary](#repository-title-and-summary)
- [Index](#index)
- [Quickstart](#quickstart)
- [Usage](#usage)
- [Project Structure](#project-structure)
- [Contribution Guidelines](#contribution-guidelines)
- [Versioning and Releases](#versioning-and-releases)
- [Personas and Responsibilities](#personas-and-responsibilities)
- [Completion checklist](#completion-checklist)

## Governing rules

- Write the README in English.
- Base every repository-specific claim on inspected evidence or user-confirmed information.
- Stop and request missing mandatory facts before creating or updating the README. Do not publish
  a best-effort document that fills required sections with statements that information is absent.
- Do not put runtime, dependency, or hardware version numbers in the README. Keep version-specific
  information in dedicated sources such as `runtime.md`, manifests, lockfiles, release tags, or
  equivalent documentation, and link to those sources when useful.
- Do not include secret values, tokens, personal data, or working credentials. Document required
  environment-variable names only.
- Use relative links for files stored in the repository.
- Keep examples minimal, executable, and validated when local safe execution is possible.
- Include all required sections. Include `Personas and Responsibilities` only for repositories
  with multiple collaborating teams or distinct personas.
- Give every heading direct, repository-specific content that fulfills exactly what the heading
  promises and meets the requirements for that section in this reference. Child headings do not
  count as content for their parent heading.
- Treat a heading as empty when it is followed immediately by another heading or end of file, or
  when its body does not answer the subject indicated by the heading. Warnings, disclaimers,
  editorial notes, future promises, `TODO`, `TBD`, `N/A`, placeholders, and statements that
  information is missing never count as section content.
- When evidence cannot supply the content promised by a required heading, stop before writing and
  ask the user one focused question whose answer will provide the missing information. Resolve
  multiple gaps one question at a time. Omit an optional heading when it is not applicable.
- Treat absence of evidence as unknown information, not proof that a dependency, credential,
  service, workflow, policy, or requirement does not exist.

## Required structure

Use these top-level sections in this order:

1. Repository title and one-sentence summary.
2. `Index`.
3. `Quickstart`.
4. `Usage`.
5. `Project Structure`.
6. `Contribution Guidelines`.
7. `Versioning and Releases`.
8. `Personas and Responsibilities`, only when conditionally required.

Additional repository-specific sections may follow when they add verified, useful information
and do not duplicate or contradict this structure.

## Repository title and summary

Start with the repository name as the H1:

```markdown
# Repository Name
```

Follow it with one concise sentence that answers:

- What does the repository provide?
- Who should use it?
- What problem does it solve?

Example:

> This repository provides a standardized training and inference pipeline for forecasting
> models used by the Data Science team.

Treat the example as a writing pattern, not as repository data.

## Index

List every required top-level section that is present and no section that is absent:

```markdown
## Index

- [Quickstart](#quickstart)
- [Usage](#usage)
- [Project Structure](#project-structure)
- [Contribution Guidelines](#contribution-guidelines)
- [Versioning and Releases](#versioning-and-releases)
- [Personas and Responsibilities](#personas-and-responsibilities)
```

Omit the final entry when the repository does not require the conditional personas section.

## Quickstart

Enable an engineer to prepare, run, and validate the project quickly.

### Prerequisites

List verified required tools, access, credentials, and infrastructure. Name tools without copying
their version numbers into the README. Link to the repository's version source when applicable.

List required environment-variable names without values:

```markdown
- `SERVICE_API_TOKEN`
- `DEPLOYMENT_ENV`
```

Do not imply that example variables or services are required unless repository evidence confirms
them.

### Installation

Document the verified dependency installation or environment setup command. Keep it concise and
link to extended setup documentation when necessary.

If no supported installation workflow can be verified, ask the user before writing. Do not invent
path manipulation, direct-import workarounds, or a statement that installation is undefined.

### Minimal Execution Example

Provide the simplest verified command that demonstrates the repository works. Use the actual
project entry point and required arguments.

Do not treat importing an empty package as a supported execution example unless an authoritative
source defines that behavior as a real use case.

### Running Tests

Provide the repository's supported test command. Include required local preparation only when it
is verified and essential.

## Usage

Explain how the repository is used in real scenarios.

### Supported Use Cases

List verified supported use cases. For each use case, briefly describe the expected input and
output when those concepts apply.

Do not extrapolate planned or unsupported capabilities from names, examples, or unfinished code.

## Project Structure

Show a concise tree containing the repository's important directories and files:

```text
.
├── src/       # Core implementation
├── tests/     # Automated tests
├── docs/      # Extended documentation
└── README.md
```

Replace the example with the real structure. Explain additional important directories after the
tree. Do not list generated, cached, private, or incidental files.

## Contribution Guidelines

### Pull Request Workflow

Document the verified branch, check, review, and ownership workflow. Do not invent naming rules or
reviewers when the repository does not define them.

Request missing contribution requirements before creating or updating the README. In an audit,
report the missing requirements as findings.

### Code Quality Standards

Document the actual formatting, linting, type-checking, testing, and coverage rules. Include a
coverage target only when an authoritative repository source defines one.

## Versioning and Releases

Document verified information about:

- The versioning strategy, such as SemVer, CalVer, or an internal strategy.
- How releases are created and identified.
- The relative location of a changelog, when one exists.
- The relative location of runtime version information, when one exists.
- The breaking-change policy.

Do not claim that `CHANGELOG.md`, `runtime.md`, tags, or a release policy exist without checking.
Ask for missing mandatory governance information rather than fabricating it.

## Personas and Responsibilities

Include this section only when multiple teams or distinct personas collaborate in the repository.

For each verified persona or team, document:

- Owned or allowed directories.
- Responsibilities.
- Restricted directories or changes, when explicitly defined.
- Shared components maintained by the persona or team.

Do not infer organizational ownership solely from directory names or commit history.

## Completion checklist

- Confirm that the title and summary answer what, who, and why.
- Confirm that the index matches the headings and anchors.
- Confirm that all required sections contain repository-specific information.
- Confirm that every heading has direct content that fulfills exactly what it promises before the
  next heading, and that no parent heading relies only on its child headings, warnings, editorial
  notes, future promises, or absence statements for content.
- Confirm that the personas section appears only when applicable.
- Confirm that commands, paths, links, and policies match inspected evidence.
- Confirm that every mandatory fact was verified or provided by the user before writing.
- Confirm that version-specific runtime, dependency, and hardware details live outside the README.
- Confirm that no secrets, credential values, personal data, invented facts, or automatic
  placeholders remain.
