tests/
# Implementation Plan: Task API — Create Task endpoint

**Branch**: `[feature/task-api-create]` | **Date**: 2026-06-19 | **Spec**: specs/001-task-api/spec.md
**Input**: Feature specification from `specs/001-task-api/spec.md`

## Summary

Implement `POST /api/v1/tasks` to create Task entities (UUID id, title, optional description, status defaulting to `TODO`, createdAt/updatedAt timestamps). Validation, authentication, observability, and contract-level OpenAPI documentation will be provided. Tests include unit validation, integration with Testcontainers PostgreSQL, and contract checks against the OpenAPI snippet.

## Technical Context

**Language/Version**: Java 21 (LTS), Spring Boot 3.x
**Primary Dependencies**: Spring Boot, Spring Web, Spring Data JPA, Spring Security, SpringDoc OpenAPI, Flyway, Micrometer, Testcontainers
**Storage**: PostgreSQL 16 (production & tests via Testcontainers)
**Testing**: JUnit 5, Spring Boot Test, Mockito, Testcontainers
**Target Platform**: Linux server; containerized deployment via Maven `spring-boot:build-image`
**Project Type**: Web service / REST API
**Performance Goals**: 95% of valid create requests within 500ms (integration/load-tested baseline)
**Constraints**: Follow repository constitution — no Lombok, DTOs as records, Flyway migrations, DB UUID generation via `gen_random_uuid()`, CSRF and session-based auth enforced

## Constitution Check

*GATE: Must pass before proceeding. Re-check after design phase.*

| Principle | Status | Notes |
|-----------|--------|-------|
| All endpoints are versioned under `/api/v1/...` | PASS | Spec uses `/api/v1/tasks` as endpoint.
| Successful creation returns `201 Created` with a `Location` header | PASS | Implemented; `Location` will be relative path `/api/v1/tasks/{id}`.
| Validation errors return `400 Bad Request` with structured body | PASS | `ErrorResponse` schema defined in OpenAPI; spec updated.
| Authentication required for endpoints by default | PASS | Plan enforces Spring Security integration; integration tests will assert `401` for unauthenticated requests.
| DTOs as Java records; layered architecture (Controller→Service→Repository) | PASS | Plan will create DTO records and standard layered components.
| Migrations via Flyway; UUID DB-generated PKs | PASS | Migration stub included in data-model.md; use `gen_random_uuid()` as default.

## Project Structure

### Documentation (this feature)

```text
specs/001-task-api/
├── spec.md
├── plan.md        # this file
├── research.md
├── openapi.yaml
├── contracts/
│   └── README.md
├── data-model.md
└── quickstart.md
```

### Source Code (repository root)

```text
src/main/java/com/example/taskapi/
├── web/controller/    # REST controllers
├── application/dto/   # Java records for requests/responses
├── application/service # service interfaces + implementations
├── domain/model/      # JPA entities
└── infrastructure/
    ├── persistence/    # repositories, migrations
    └── config/         # security, web config

src/test/java/...      # unit and integration tests (JUnit 5 + Testcontainers)
```

**Structure Decision**: Follow constitution-mandated package layout; controllers remain thin, services own business logic, and repositories use Spring Data JPA.

## Execution Strategy

### TDD Requirements

- `application.service.TaskService` and its validation logic — [TDD] required to ensure validation behavior (title non-blank, maxlengths, reject client `status`).
- Controller-to-service wiring and error mapping — [TDD] to validate responses and status codes.

### Parallel Execution Opportunities

- `OpenAPI contract + spec` work can proceed in parallel with `DTO and validation` implementation.
- `DB migration` and `repository` tasks can be developed independently from controller wiring and auth configuration.

### Human Checkpoints

1. After OpenAPI + DTOs are added — review contract and examples
2. After DB migration and repository implementation — review schema and indexes
3. After integration tests pass — verify performance metrics in a smoke load test
4. Before merge — final review against specification and constitution

### Review Gates

- API contracts/interfaces: [REVIEW] before client consumers are implemented
- Security-sensitive code (auth config, session handling): [REVIEW]
- Data model changes/migrations: [REVIEW]

## Complexity Tracking

No constitution violations identified.

