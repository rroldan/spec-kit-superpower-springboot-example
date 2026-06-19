# Research: Task API

This document consolidates decisions and unknowns discovered while preparing the implementation plan.

## Decisions

- Error response format: Custom `ErrorResponse` with `errors` map (field -> list of messages).
- Location header: Relative path `/api/v1/tasks/{id}`.
- OpenAPI contract: File `openapi.yaml` added under the feature directory.
- Language/platform: Java 21 + Spring Boot 3.x per constitution.

## Unknowns / NEEDS CLARIFICATION

- Idempotency behavior: Should the API accept an idempotency key to deduplicate creates? (Default: no idempotency key supported in iteration 1.)
- Duplicate titles: Allowed; no uniqueness constraint specified.

## Alternatives considered

- Use RFC7807 Problem+JSON for errors: rejected in favor of a simple structured `ErrorResponse` for clarity in validation tests.

## Outcome

Decisions recorded in `spec.md` and `openapi.yaml`. Implementation plan updated in `plan.md`.
