# Phase 1 — Repository and Toolchain Baseline

## Repository baseline

- Repository: `AshfanMEW/pos-platform`
- Default branch: `main`
- Bootstrap branch: `bootstrap/phase-1`
- Initial repository contents: `README.md` only
- Visibility: public; do not commit secrets, credentials, personal data, or production configuration

## Planned solution layout

```text
src/
  BuildingBlocks/
  Modules/
  Host/
tests/
  UnitTests/
  IntegrationTests/
  ArchitectureTests/
frontend/
  pos-web/
docs/
deploy/
.github/workflows/
```

The backend will be a modular monolith. Modules will own their domain and application behavior while sharing only deliberately designed building blocks. The Angular client will use standalone components, strict TypeScript, lazy-loaded feature areas, and an offline-ready synchronization foundation.

## Verification workflow

`.github/workflows/toolchain-check.yml` runs on pushes to `main` and `bootstrap/**`, and pull requests targeting `main`. It records the GitHub-hosted runner versions for .NET, Node.js, npm, Docker, and Git, and verifies that Angular 22 packages are available from npm.

The workflow is an inspection gate only. It does not install application dependencies, run migrations, or claim that the solution builds until the solution exists and the relevant build/test jobs are added.

## Constraints and defaults

- PostgreSQL is the development and production database target.
- Persisted instants use UTC; tenant and branch time zones are explicit configuration.
- Tenant identity is derived and validated server-side; client-supplied tenant headers are never trusted by themselves.
- Payment providers, fiscalization, hardware, and storage are adapter interfaces with safe development implementations.
- Country-specific legal compliance is not claimed without a configured provider and documented assumptions.
- Database changes will use EF Core migrations; production startup will not perform destructive schema changes.

## Phase 1 exit criteria

- Backend solution and projects compile with the runner SDK.
- Angular workspace installs and compiles with strict TypeScript.
- PostgreSQL local-development services are defined in Docker Compose.
- Formatting, linting, unit tests, and baseline integration-test commands are documented and runnable.
- No secrets are committed.
