# Data Model: Task

## Table: tasks

Columns:
- id UUID PRIMARY KEY DEFAULT gen_random_uuid()
- title VARCHAR(255) NOT NULL
- description TEXT NULL
- status VARCHAR(20) NOT NULL DEFAULT 'TODO'
- created_at TIMESTAMP WITH TIME ZONE NOT NULL DEFAULT now()
- updated_at TIMESTAMP WITH TIME ZONE NOT NULL DEFAULT now()

Indexes:
- ix_tasks_created_at (created_at) — useful for sorting/paging

Flyway migration: place SQL under `src/main/resources/db/migration/V2__create_tasks_table.sql`.

Notes:
- Use `@EnableJpaAuditing` with `@CreatedDate` and `@LastModifiedDate` for timestamps.
- No ownerId column in this iteration (global/shared tasks).
