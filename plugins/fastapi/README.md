# fastapi

FastAPI scaffolding (modules, DTOs, endpoints) + Alembic migrations with auto-fix for Enum downgrade.

Consolidated from `fastapi-scaffold` and `alembic-migrations` plugins.

## Commands

| Command | Description |
|---------|-------------|
| `/dto` | Create Create/Read/Update DTOs from model |
| `/endpoint` | Create a new endpoint with service and test |
| `/module` | Create a complete FastAPI module (routers, services, repositories, dto, models + tests) |
| `/migrate:check` | (Alembic) Check migration for common problems |
| `/migrate:create` | (Alembic) Create migration with auto-fix for Enum downgrade |

## Skills

- **fastapi-patterns** -- Repository pattern, dependency injection, DTOs, exception handling
- **alembic-patterns** -- Enum handling in migrations, upgrade/downgrade best practices

## Agents

- **migration-reviewer** -- Analyzes Alembic migrations before applying, finds potential problems
