<!--
Sync Impact Report
- Version change: none → 1.0.0
- Modified principles:
  - I. Library-First
  - II. CLI & Automated Workflows
  - III. Test-First (NON-NEGOTIABLE)
  - IV. Integration & Contract Testing
  - V. Observability, Versioning & Simplicity
- Added sections: Constraints & Security; Development Workflow
- Removed sections: none
- Templates requiring updates:
  - .specify/templates/constitution-template.md ✅ updated (source template kept for reference)
  - .specify/templates/plan-template.md ⚠ pending — requires constitution-driven "Constitution Check" gate mapping
  - .specify/templates/spec-template.md ⚠ pending — ensure mandatory User Scenarios & Testing align with Test-First principle
  - .specify/templates/tasks-template.md ⚠ pending — ensure task organization enforces test-first and foundational gate
  - .specify/templates/checklist-template.md ⚠ pending — recommend checklist items reflect new governance rules
- Follow-up TODOs:
  - Ensure CI checks include a "Constitution Check" step that validates PRs against governance rules (pending CI update)
-->

# Spec Kit Superpower Spring Boot Example Constitution

## Core Principles

### I. Library-First
Every feature must begin as a self-contained module or Spring Boot component. Modules MUST be independently testable, documented, and have a single, well-defined purpose. Organizational or cross-cutting utilities without a clear consumer are forbidden unless justified in the plan and approved in the PR.

### II. CLI & Automated Workflows
Tooling and developer workflows MUST be exposed via CLI commands and automated pipelines following the specify → plan → tasks → implement model. Tool outputs intended for automation MUST support a machine-readable format (JSON) and a human-readable format for developer consumption.

### III. Test-First (NON-NEGOTIABLE)
TDD is required. Every change MUST include failing tests (unit, integration, or contract as appropriate) before implementation. Tests MUST be added to the PR, run in CI, and pass before merge. Tests are the authoritative specification for behavior.

### IV. Integration & Contract Testing
Changes that affect inter-service interactions, public contracts, or shared schemas MUST include contract and integration tests. Contract tests belong under tests/contract and integration tests under tests/integration. These tests are required for any change that alters surface APIs or shared data models.

### V. Observability, Versioning & Simplicity
All services and modules MUST emit structured logs and expose metrics appropriate for the runtime environment. Versioning MUST follow MAJOR.MINOR.PATCH semantics; breaking changes require a MAJOR bump and migration notes. Favor simple designs; any added complexity MUST be justified in the plan and documented.

## Constraints & Security
- Technology stack: Spring Boot (primary framework), Spec Kit 0.10.2, GitHub Copilot integrations, Superpowers extensions.
- Secrets and credentials MUST NOT be committed to repository files. Use environment configuration or secret managers endorsed by the organization.
- Performance and resource constraints MUST be captured in plan.md when relevant (p95 targets, memory limits, etc.).
- Compliance-sensitive features MUST list regulatory constraints (e.g., GDPR) in the plan and spec.

## Development Workflow
- Follow the workflow: specify → plan → tasks → implement. Use branch names like feature/short-description and include a link to the spec in the PR description.
- Code review: PRs MUST include tests, a plan reference, and a short migration/rollback note when applicable. At least one maintainer review is required for non-trivial changes.
- CI gates: Unit tests, linters, and the Constitution Check (validating governance compliance) MUST pass before merge. Integration and contract tests MUST run in CI for relevant changes.
- Release process: Tag releases with semantic versions and include change notes that reference any constitution-driven migration guidance.

## Governance
- The constitution supersedes informal practices. Amendments to the constitution require a documented proposal (PR) and approval from repository maintainers.
- Versioning policy:
  - MAJOR: Backward-incompatible governance or principle removals/renames.
  - MINOR: Addition of new principle or material expansions to guidance.
  - PATCH: Clarifications, wording fixes, and non-semantic refinements.
- Amendment procedure: Propose changes via PR against .specify/memory/constitution.md. Include rationale, migration plan (if applicable), and tests or checks that will enforce the change. Maintainters must approve and merge; the constitution's Last Amended date updates to the merge date.
- Compliance: PRs that touch policy-sensitive areas MUST include a short "Constitution Compliance" section in the PR body describing how the change aligns or why a deviation is necessary.

**Version**: 1.0.0 | **Ratified**: 2026-06-14 | **Last Amended**: 2026-06-14
