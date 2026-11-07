# Feature Specification: Todo App Baseline

**Feature Branch**: `master`
**Created**: 2026-11-07
**Status**: Draft
**Input**: User description: "@requirements-KR.txt" (existing requirements specification for the Getting Started Todo App)

## Clarifications

### Session 2026-11-07

- Q: Should the task list be one shared list for all users, or a separate list for each person? → A: B - Each person has their own list, and the user must be identified (signed in) to see it. (Superseded by the decision below.)
- Q: When the saved task list cannot be loaded, what should the user see? → A: An error message with a "Try again" button.
- Q: What is the maximum length of a task name? → A: C - No length limit.
- Q: How quickly should the task list appear after the app is opened? → A: A - Within 2 seconds (SC-004 unchanged).
- Q: How should users sign in? → A: A - Email address and password. (Superseded by the decision below.)
- Q: Plan gate: the sign-in and per-user list conflict with the project's documented scope (constitution Principle I, `requirements-KR.txt`). How should the spec proceed? → A: B - Revert to one shared task list with no sign-in. FR-013 is restored, FR-016 and the User entity are removed, and the Task owner attribute is removed.

## User Scenarios & Testing *(mandatory)*

### User Story 1 - View and Add Tasks (Priority: P1)

A user opens the app and sees their list of tasks. They type a new task into the entry box and add it to the list.

**Why this priority**: Viewing and adding tasks is the core value of a to-do app. Without it, nothing else is useful.

**Independent Test**: Open the app with an empty list, add one task, and confirm it appears in the list.

**Acceptance Scenarios**:

1. **Given** the user has no saved tasks, **When** they open the app, **Then** they see the message "No items yet! Add one above!"
2. **Given** the user has saved tasks, **When** they open the app, **Then** all saved tasks are listed, and a loading indication is shown while the list is being retrieved.
3. **Given** the entry box is empty, **When** the user looks at the add button, **Then** the button is unavailable.
4. **Given** the user has typed a task name, **When** they press the add button, **Then** the task appears in the list as not completed, and the entry box is cleared.

---

### User Story 2 - Mark Tasks as Completed (Priority: P2)

A user marks a task as done, and can change it back to not done.

**Why this priority**: Tracking progress is the next most important action after capturing tasks.

**Independent Test**: Add a task, mark it complete, confirm it looks completed, then mark it not complete again.

**Acceptance Scenarios**:

1. **Given** a task that is not completed, **When** the user selects its checkbox, **Then** the task is shown as completed.
2. **Given** a completed task, **When** the user selects its checkbox again, **Then** the task is shown as not completed.

---

### User Story 3 - Delete Tasks (Priority: P3)

A user removes a task they no longer need.

**Why this priority**: Removing tasks keeps the list accurate, but users can still function without it.

**Independent Test**: Add a task, delete it, and confirm it no longer appears in the list.

**Acceptance Scenarios**:

1. **Given** a task in the list, **When** the user selects its delete icon, **Then** the task is removed from the list.
2. **Given** a task was deleted, **When** the user reopens the app, **Then** the deleted task does not reappear.

---

### User Story 4 - Tasks Persist Between Visits (Priority: P2)

A user's tasks are still there when they come back later.

**Why this priority**: A task list that forgets its contents is not useful, so saving is required for the core features to matter.

**Independent Test**: Add a task, close the app, reopen it, and confirm the task is still listed with its completion state.

**Acceptance Scenarios**:

1. **Given** a user added a task and marked it completed, **When** they close and reopen the app, **Then** the task is listed with the same name and completed state.

---

### User Story 5 - See a Greeting Banner (Priority: P3)

A user sees a heading at the top of the page. This confirms the app has loaded and is connected.

**Why this priority**: It is a demonstration feature that helps a developer confirm the app works. It adds little direct value to end users.

**Independent Test**: Open the app and confirm the heading "Hello world!" is shown at the top.

**Acceptance Scenarios**:

1. **Given** the app is running and connected, **When** the user opens it, **Then** the heading "Hello world!" is shown at the top of the page.

---

### Edge Cases

- What happens when the user submits a blank or whitespace-only task name? The add button is unavailable, so no task is created.
- What happens when the saved task list cannot be retrieved? The user sees an error message with a "Try again" button. The empty-list message is NOT shown, because it would look like "no tasks".
- What happens when the user adds a task while a previous add is still in progress? The button shows "Adding..." and is unavailable until the add finishes.
- What happens when a task is deleted or changed by another browser session? The app shows the latest saved state the next time the list is loaded. No live syncing is required.
- What happens when the user tries to rename a task? Renaming is not offered in the interface, so this is out of scope.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: The system MUST display all saved tasks when the app opens.
- **FR-002**: The system MUST show "No items yet! Add one above!" when there are no saved tasks.
- **FR-003**: The system MUST show a loading indication while saved tasks are being retrieved.
- **FR-014**: When saved tasks cannot be retrieved, the system MUST show an error message with a "Try again" button. Selecting it MUST retry the retrieval. The system MUST NOT show the empty-list message in this case.
- **FR-015**: The system MUST accept task names of any length. No maximum length is enforced.- **FR-004**: The system MUST let the user add a task by entering a name and submitting it.
- **FR-005**: The system MUST NOT allow submission of a task with an empty name.
- **FR-006**: The system MUST store each new task as not completed.
- **FR-007**: The system MUST give each new task a unique identifier that does not repeat for any other task.
- **FR-008**: The system MUST let the user toggle a task between completed and not completed.
- **FR-009**: The system MUST visually distinguish completed tasks from tasks that are not completed.
- **FR-010**: The system MUST let the user delete a task.
- **FR-011**: The system MUST save every change to tasks (add, toggle, delete) so that it is still present after the app is closed and reopened.
- **FR-012**: The system MUST display the heading "Hello world!" at the top of the page.
- **FR-013**: The system MUST NOT require a user account, login, or any identification to use the task list.

### Key Entities

- **Task**: A single item the user wants to track. Attributes: a unique identifier, a name entered by the user, and a completed or not-completed status. A new task always starts as not completed.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: A first-time user can add a task and see it listed in under 30 seconds.
- **SC-002**: 100% of tasks added, completed, or deleted are still in the correct state after the app is closed and reopened.
- **SC-003**: A user can mark a task complete or delete it with one action each.
- **SC-004**: The list shows the correct tasks within 2 seconds of opening the app under normal conditions.
- **SC-005**: No task is ever created with an empty name.

## Assumptions

- The task list is shared by everyone who uses the app. There are no per-user lists and no sign-in, because accounts and login are out of scope (FR-013).
- Task names are plain text entered by the user. No maximum length applies (FR-015).
- Renaming an existing task is out of scope for the interface, even though it may exist internally.
- The greeting banner (User Story 5) is kept as part of the baseline because the requirements list it, but it is a demonstration feature rather than a user need.
- "Normal conditions" in SC-004 means the app is running and reachable. Performance under heavy load is not specified.
- The specific storage technology is not part of this specification. Persistence is defined only by the user-visible requirement in FR-011.
