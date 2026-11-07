# Quickstart: Todo App Baseline

**Feature**: [spec.md](spec.md) | **Contract**: [contracts/items-api.md](contracts/items-api.md)

Validation guide. It proves each user story works end-to-end. Implementation details belong in
`tasks.md`.

## Prerequisites

- Node.js 22 and npm
- Docker and Docker Compose (for the MySQL path)

## Setup

```bash
cd backend && npm install
cd ../client && npm install
```

## Scenario 1: Empty list and first add (User Story 1)

1. Start the app with no `MYSQL_HOST` set (SQLite default).
2. Open the page. Expect: the loading indication, then "No items yet! Add one above!".
3. Confirm the Add button is disabled while the input is empty.
4. Type a name, select Add Item. Expect: the task appears, unchecked, and the input clears.

## Scenario 2: Toggle and delete (User Stories 2 and 3)

1. Select the checkbox on the task. Expect: it shows as completed.
2. Select the checkbox again. Expect: it shows as not completed.
3. Select the delete icon. Expect: the task is removed.

## Scenario 3: Persistence (User Story 4)

1. Add a task and mark it completed.
2. Stop and restart the backend.
3. Reload the page. Expect: the same task, same completed state.

## Scenario 4: Load failure (FR-014)

1. Stop the backend while the page is open.
2. Reload the page. Expect: an error message with "Try again", not the empty-list message.
3. Start the backend, select "Try again". Expect: the list loads.

## Scenario 5: Greeting (User Story 5)

1. Open the page. Expect: the heading "Hello world!".

## Scenario 6: Long name (FR-015)

1. Enter a name of 1,000 characters and add it. Expect: stored and shown in full.

## Automated checks

```bash
cd backend && npm test
cd ../client && npm run lint
npm run format-check    # run in both backend and client
```

## MySQL path

```bash
docker compose up --build
```

Repeat Scenario 3 against MySQL to confirm persistence through the `MYSQL_HOST` path.
