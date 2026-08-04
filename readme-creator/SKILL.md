---
name: readme-creator
description: Create, audit, and update English repository README.md files against a standardized governance template. Use when Codex needs to draft a new README, review an existing README for compliance, or revise repository documentation covering quickstart, usage, project structure, contribution rules, releases, and optional team responsibilities.
---

# README Creator

Create or review repository README files from verified project evidence.

Read [references/readme_standard.md](references/readme_standard.md) completely before drafting,
auditing, or updating a README.

## Choose the operation

- Treat requests to create, rewrite, fix, or update a README as authorization to edit it.
- Treat requests to review, audit, assess, or report on a README as read-only. Return findings
  without editing unless the user separately asks for changes.
- Preserve accurate repository-specific sections that add value and do not conflict with the
  standard.

## Inspect before writing

1. Inspect the existing README and repository instructions.
2. Inspect manifests, lockfiles, task runners, CI configuration, test configuration, source
   layout, documentation, release files, and ownership files that can support README claims.
3. Determine the repository's purpose, intended users, problem solved, supported use cases,
   inputs and outputs, setup workflow, minimal execution, test command, important directories,
   contribution process, release strategy, and team boundaries.
4. Distinguish verified facts from missing information. Ask the user only for required facts
   that cannot be discovered after a reasonable inspection.

Do not invent commands, files, owners, infrastructure, access requirements, coverage targets,
release policies, inputs, or outputs. Do not insert automatic placeholders as substitutes for
missing facts.

## Stop for missing mandatory facts

Before creating or updating a README, confirm that evidence or user input supplies every fact
needed by the required sections. Treat missing purpose, audience, problem, setup, execution,
tests, supported use cases, contribution process, quality policy, versioning, release process, or
breaking-change policy as blocking information.

- Stop before editing and ask focused questions for the missing facts.
- Do not create a best-effort README that describes required information as absent.
- Do not convert an empty package, import check, directory name, or lack of configuration into a
  supported use case, audience, setup workflow, or contribution policy.
- Do not add generic recommendations such as default branch practices or test expectations unless
  the repository or user defines them.
- Treat absence of evidence as unknown information, not proof that no credentials, services,
  dependencies, policies, or requirements exist.

During a review-only audit, report these gaps as findings instead of asking questions unless the
user also requests a corrected README.

## Apply the standard

- Write all README headings and prose in English.
- Use the required sections and ordering from the reference.
- Include `Personas and Responsibilities` only when evidence shows that multiple teams or
  personas collaborate in the repository.
- Keep the index synchronized with the headings actually present. Use valid lowercase Markdown
  anchors.
- Describe the real repository structure rather than copying the example tree.
- Keep runtime, dependency, and hardware version numbers out of the README. Link to verified
  dedicated sources such as `runtime.md`, manifests, lockfiles, release tags, or other
  repository documentation instead.
- Use relative repository links for files such as `CHANGELOG.md` and `runtime.md`.
- Mention environment-variable names when necessary, but never include secret values, tokens,
  personal data, or working credentials.
- Include only commands that are supported by repository evidence.

## Create or update

1. Draft a one-sentence introduction that states what the repository provides, who should use
   it, and the problem it solves.
2. Build the required sections from verified evidence.
3. Resolve all missing mandatory facts with the user before editing the README.
4. Preserve useful existing content, adapting its location and language when needed.
5. Remove stale or contradictory instructions only when the repository evidence demonstrates
   that they are wrong.
6. Edit only the README and explicitly requested companion documentation.

## Audit

Compare the README with the reference and repository evidence. Report:

- Missing required or conditionally required content.
- Inaccurate, stale, contradictory, or unverified claims.
- Commands, links, anchors, and paths that do not match the repository.
- Version-specific runtime, dependency, or hardware information that belongs elsewhere.
- Secret-like values or sensitive information that must not remain in documentation.

Separate confirmed defects from questions that require user input. Do not rewrite the file
during a review-only request.

## Validate

- Check headings, index anchors, relative links, paths, and fenced code blocks.
- Run relevant local, safe, non-destructive commands when practical to validate installation,
  minimal execution, and test instructions.
- Do not install dependencies, use credentials, contact external services, start persistent
  processes, or modify infrastructure without explicit authorization.
- Treat commands that cannot be executed as unverified. Explain the limitation in the final
  response rather than adding validation logs or status claims to the README.
- Do not present a failing or unexecuted command as known to work. Investigate, correct the
  documentation, or request the missing information.
- Do not invent undocumented workarounds such as custom environment variables, path manipulation,
  or direct module imports merely to produce an executable example.

## Deliver

For an edit, summarize the README changes, list the validation commands and outcomes, and call
out any unresolved facts. For an audit, return concise findings with supporting repository
evidence and recommended corrections.
