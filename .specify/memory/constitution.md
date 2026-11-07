<!--
SYNC IMPACT REPORT (scratch — remove before committing)
Version change: [unversioned template] → 1.0.0 (initial adoption; no prior project values)
Modified principles: none (template placeholders replaced)
Added sections: Technology Constraints, Development Workflow
Removed sections: none (template sections renamed to match the project)
Deferred TODOs:
  - TODO(RATIFICATION_DATE): original adoption date unknown; confirm and replace.
-->

# Getting Started Todo App Constitution

## Core Principles

### I. Documented Scope
The application MUST implement only the features listed in `requirements-KR.txt`: list, add,
toggle completion, delete, greeting banner, and persistence selection. New features, user accounts,
or authentication MUST NOT be added without first amending the requirements file.
Rationale: the project is a teaching reference app; a fixed scope keeps it easy to verify.

### II. Stable REST Contract
The client and backend MUST communicate only through the documented `/api` endpoints
(`GET /api/greeting`, `GET|POST /api/items`, `PUT|DELETE /api/items/:id`). Any change to a request
or response shape MUST update `requirements-KR.txt` and both `backend/` and `client/` in the same
change. The backend MUST generate item IDs as UUID v4 and MUST store new items with
`completed: false`.

### III. Pluggable Persistence
All database access MUST go through `backend/src/persistence/`. Route handlers MUST NOT import
database drivers (`mysql2`, `sqlite3`) directly. MySQL MUST be used when `MYSQL_HOST` is set;
SQLite MUST be the default otherwise. Both backends MUST expose the same interface.

### IV. Verified Changes
Every change MUST pass the checks that exist for the touched package before it is merged:
backend unit tests (`npm test` in `backend/`, Jest), client lint (`npm run lint` in `client/`),
and Prettier format checks (`npm run format-check`). Bug fixes MUST include a regression test
in `backend/spec/` when the bug is in backend logic.

### V. Declared Dependencies and Secrets
Every package imported by source code MUST be declared in the `package.json` of its package.
Lock files MUST be committed. Credentials MUST NOT be committed; database credentials MUST come
from environment variables or `*_FILE` secret files, as supported by `backend/`.

## Technology Constraints

- Runtime: Node.js 22 (`Dockerfile` uses `node:22`).
- Backend: Express 5, `mysql2` for MySQL, `sqlite3` for SQLite, `uuid`, `wait-port`.
- Client: React 19 with Vite 6; Bootstrap 5 and react-bootstrap for UI; FontAwesome for icons.
- Infrastructure: Docker and Docker Compose are the reference environment. MySQL 9.3 and Traefik v3.6
  are used in `compose.yaml`. phpMyAdmin is for development only and MUST NOT be part of production
  configuration.

## Development Workflow

- Work on a feature or fix branch; commits go to `master` only through reviewed pull requests.
- Commit messages MUST describe the change in one line (e.g., `fix: ...`, `feat: ...`).
- Prettier formatting is applied before commit; formatting-only changes MUST NOT be mixed with
  behavior changes in the same commit.
- Before requesting review, run the Core Principle IV checks and note the results in the PR.

## Governance

This constitution supersedes other project practices. Amendments MUST be made by a pull request
that updates this file, the Sync Impact Report, and any dependent documentation
(`requirements-KR.txt`) in the same change.

Versioning follows semantic versioning:
- MAJOR: a principle is removed or redefined in a backward-incompatible way.
- MINOR: a principle or section is added, or existing guidance is materially expanded.
- PATCH: wording, clarification, or typo fixes with no change in meaning.

Compliance: every PR review MUST confirm that the change respects Principles I–V. Any deviation
MUST be justified in the PR description.

**Version**: 1.0.0 | **Ratified**: TODO(RATIFICATION_DATE) | **Last Amended**: 2026-11-07
