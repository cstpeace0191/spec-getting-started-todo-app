# Contract: Items API

**Feature**: [spec.md](../spec.md) | **Data model**: [data-model.md](../data-model.md)

Base path: `/api`. All bodies are JSON. This contract matches the endpoint summary in
`requirements-KR.txt`.

## GET /api/greeting

Returns the banner text (User Story 5, FR-012).

- **200 OK**: `{ "greeting": "Hello world!" }`

## GET /api/items

Returns every task on the shared list (User Story 1).

- **200 OK**: `[ { "id": "<uuid>", "name": "<string>", "completed": <boolean> }, ... ]`
- Empty list: `[]`. The client shows "No items yet! Add one above!".
- **Failure** (any non-2xx or network error): the client shows the error state with "Try again"
  (FR-014). The client must not treat failure as an empty list.

## POST /api/items

Creates a task (User Story 1, FR-004 to FR-007).

- **Request**: `{ "name": "<string, non-empty after trim>" }`
- **201 Created** (or 200 OK): `{ "id": "<uuid>", "name": "<string>", "completed": false }`
- **400 Bad Request**: `name` is missing or empty after trim (FR-005).

## PUT /api/items/:id

Updates a task (User Story 2). The UI sends only the toggled `completed` value.

- **Request**: `{ "name": "<string>", "completed": <boolean> }`
- **200 OK**: the updated task, same shape as the list entry.
- **404 Not Found**: no task with that `id`.

## DELETE /api/items/:id

Deletes a task (User Story 3).

- **200 OK**: no body required.
- **404 Not Found**: no task with that `id`.

## Compatibility

No changes from `requirements-KR.txt`. Any change to these shapes must update that file in the same
change (constitution Principle II).
