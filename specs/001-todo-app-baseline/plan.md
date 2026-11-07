# Implementation Plan: Todo App Baseline

**Branch**: `001-todo-app-baseline` | **Date**: 2026-11-07 | **Spec**: [spec.md](spec.md)

**Input**: Feature specification from `/specs/001-todo-app-baseline/spec.md`

**Status**: Design complete (Phase 0 and Phase 1). Ready for `/speckit-tasks`.

## Summary

A single shared todo list, with no sign-in. Users can view, add, toggle completion, and delete tasks.
Tasks persist between visits. A failed list load shows an error with a "Try again" button. The page
shows a "Hello world!" greeting banner.

The spec was reverted from a per-user, sign-in design to keep the documented scope in
`requirements-KR.txt` and constitution Principle I.

**Repository note:** This repository currently contains only the Spec Kit scaffold, `README.md`, and
`requirements-KR.txt`. The application code described in `requirements-KR.txt` (`backend/`, `client/`)
is not present. The source layout below is the target structure that tasks must create.

## Technical Context

**Language/Version**: JavaScript (Node.js 22 runtime, per `requirements-KR.txt` Dockerfile reference); React JSX on the client

**Primary Dependencies**: Backend: Express 5.1, mysql2 3.15, sqlite3 5.1.7, uuid 11.1, wait-port 1.1.
Client: React 19.2, react-dom 19.2, Vite 6.4, Bootstrap 5.3 / react-bootstrap 2.10, FontAwesome 6.
Versions are the ones recorded in `requirements-KR.txt`.

**Storage**: MySQL 9.3 when `MYSQL_HOST` is set; SQLite file (default `/etc/todos/todo.db`) otherwise.

**Testing**: Jest 29.7 for backend unit tests (`backend/spec`); ESLint 8 for client lint; Prettier 3
`format-check` for both packages.

**Target Platform**: Docker containers via Docker Compose; Traefik v3.6 routes `/api` to the backend in development.

**Project Type**: Web application (Express backend + React/Vite client)

**Performance Goals**: The task list appears within 2 seconds of opening the app (SC-004).

**Constraints**: No user accounts or sign-in (FR-013). Failed list loads show an error with retry
(FR-014). No maximum task name length (FR-015).

**Scale/Scope**: Not specified. Assumed to be a small teaching application running as a single instance.
No concurrency target is defined in the spec.

## Constitution Check

*GATE: Must pass before Phase 0 research. Re-check after Phase 1 design.*

Checked against `.specify/memory/constitution.md` v1.0.0.

| Principle | Status | Note |
|-----------|--------|------|
| I. Documented Scope | PASS | No accounts or sign-in. Scope matches `requirements-KR.txt`. |
| II. Stable REST Contract | PASS | Endpoints are the ones documented in `requirements-KR.txt`. FR-014 is client-side behavior, so no contract change. See `contracts/items-api.md`. |
| III. Pluggable Persistence | PASS | Both MySQL and SQLite are reached only through `backend/src/persistence/`. |
| IV. Verified Changes | PASS (planned) | Backend unit tests with Jest; client lint and format checks. |
| V. Declared Dependencies and Secrets | PASS (planned) | DB credentials come from environment variables or `*_FILE` secrets. No new dependencies are needed. |

**Re-check after Phase 1:** All principles still PASS. No complexity violations.

## Phase 0: Research

See [research.md](research.md). No `NEEDS CLARIFICATION` markers remain.

## Phase 1: Design

- Data model: [data-model.md](data-model.md)
- Interface contract: [contracts/items-api.md](contracts/items-api.md)
- Validation guide: [quickstart.md](quickstart.md)

## Project Structure

### Documentation (this feature)

```text
specs/001-todo-app-baseline/
├── plan.md              # This file
├── research.md          # Phase 0 output
├── data-model.md        # Phase 1 output
├── quickstart.md        # Phase 1 output
├── contracts/
│   └── items-api.md     # Phase 1 output
├── checklists/
│   └── requirements.md
├── spec.md
└── tasks.md             # Phase 2 — created by /speckit-tasks, not by /speckit-plan
```

### Source Code (repository root) — target layout, not yet present

```text
backend/
├── src/
│   ├── index.js                 # Server start, graceful shutdown
│   ├── routes/                  # getGreeting, getItems, addItem, updateItem, deleteItem
│   └── persistence/             # index.js (selector), mysql.js, sqlite.js
└── spec/                        # Jest unit tests

client/
└── src/
    └── components/              # Greeting, TodoListCard, AddNewItemForm, ItemDisplay
```

**Structure Decision**: Two-package web application as described in `requirements-KR.txt`.
Backend persistence is isolated behind one interface with MySQL and SQLite implementations. Tasks
must create these directories; the plan does not assume they exist.

## Complexity Tracking

No violations. The table is intentionally empty.
