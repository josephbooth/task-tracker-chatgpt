# Solution Requirements

## 1. Purpose

This document defines what the ChatGPT Task Tracker is expected to do and the minimum capabilities required of an AI agent operating it.

The solution is a lightweight, file-based personal task tracker. GitHub stores the repository, Markdown stores the task data and operating instructions, and an AI agent provides the conversational interface.

The solution deliberately avoids a dedicated application, database, server, or programming language requirement for everyday task management.

## 2. Scope

### In Scope

The solution must support an individual who wants to manage personal tasks through natural-language requests to an AI agent.

The agent must be able to:

- Read the repository instructions and current task ledger.
- Display open tasks, including overdue, dated, and undated tasks.
- Create tasks and projects using the established ledger format.
- Update requested task details without changing unrelated information.
- Complete and reopen tasks while preserving task identity and history.
- Delete tasks only when explicitly requested.
- Maintain shopping visits and their item lists.
- Interpret due dates using the user's local date and timezone.
- Verify repository changes after writing.

### Out of Scope

The solution does not provide:

- A standalone or hosted task-management application.
- A database or separate task-management service.
- Automatic reminders, notifications, or background scheduling.
- Guaranteed synchronization with calendars or other task systems.
- Multi-user assignment, team workflows, or role-based task management.
- A guarantee that every AI client or account can access or modify GitHub repositories.

These capabilities may be explored separately, but they are not requirements of the current solution.

## 3. Solution Components

The solution has three primary components:

| Component | Responsibility |
|---|---|
| GitHub repository | Stores the files and provides version history. |
| Markdown files | Store the task ledger, data format, and operating instructions. |
| AI agent | Interprets user requests and performs authorized operations against repository files. |

The `tasks.md` file is the sole authoritative source for task and project state. Conversation history, memory, and cached copies are not authoritative.

## 4. Preconditions and Agent Capabilities

Before managing tasks, the agent must:

1. Identify the intended repository and its default or designated working branch.
2. Read `README.md`, `AGENTS.md`, and `tasks.md`.
3. Read relevant supporting documentation, particularly the ledger format and assistant workflow.
4. Confirm that it can retrieve the current ledger.
5. Determine whether repository write operations are available before attempting a change.

The AI client must provide, directly or through an authorized integration:

- Repository file retrieval.
- Repository file creation and update when write operations are expected.
- A way to identify the current version of a file or otherwise detect conflicting changes.
- The ability to retrieve a file again after a write to verify the result.

Read access and write access are separate capabilities. The agent must not infer one from the other.

If a capability is unavailable, the agent must explain the limitation and offer a suitable manual alternative. It must not claim to have performed an operation that it could not verify.

## 5. Functional Requirements

### FR-01: Read Current Task State

The agent must retrieve the latest `tasks.md` before answering questions about task state.

It must not use remembered or previously retrieved task data as a substitute for the current ledger.

### FR-02: Display Tasks

When asked for a general task overview, the agent must:

- Show open tasks.
- Include overdue tasks.
- Sort dated tasks by due date.
- Show undated tasks after dated tasks.
- Exclude completed tasks unless requested.

The agent must be able to filter tasks by date or status when asked.

### FR-03: Create Tasks

When explicitly asked to create a task, the agent must:

- Use the next available task ID.
- Populate all required fields in the documented format.
- Use `none` for optional fields without a value.
- Place the record under Open tasks.
- Increment the next task ID.
- Preserve all existing records.

If an essential detail is ambiguous and cannot be safely inferred, the agent must ask one focused question before writing.

### FR-04: Create Projects

When explicitly asked to create a project, the agent must:

- Use the next available project ID.
- Populate the documented project fields.
- Place the record under Projects.
- Increment the next project ID.
- Preserve all existing records.

A project groups related tasks; it is not itself an actionable task.

### FR-05: Update Tasks

The agent must modify only the fields the user requested.

It must preserve the task ID and all unrelated task details. It must not silently change status, due date, project association, recurrence, or description.

### FR-06: Complete Tasks

When explicitly asked to complete a task, the agent must:

- Move the complete record from Open tasks to Completed tasks.
- Set its status to `completed`.
- Set its completion date.
- Preserve the task ID and relevant task details.

Completion must not be treated as deletion.

### FR-07: Reopen Tasks

When explicitly asked to reopen a completed task, the agent must:

- Move the complete record to Open tasks.
- Set its status to `open`.
- Clear its completion date by setting it to `none`.
- Preserve the task ID and relevant details.

### FR-08: Delete Tasks

The agent may delete a task only when the user explicitly requests deletion.

It must not reuse a deleted ID or decrement the next-ID counter. If the user says to finish, close, or complete a task, the agent must not interpret that as a deletion request.

### FR-09: Shopping Visits

The agent must support the documented `grocery_visit` task type.

- Each shopping trip must have its own task record.
- Items must be represented as separate lines in the description.
- Creating a new visit must not overwrite an earlier visit.
- A recurring visit starts with an empty list unless the user asks to carry items forward.

### FR-10: Dates and Timezones

The ledger must store due dates in `YYYY-MM-DD` format. Times of day belong in the title or description, not the due-date field.

The agent must use the user's local date and timezone when interpreting relative dates and determining which tasks are due today.

If the user's local date or timezone cannot be established reliably and it affects the requested operation, the agent must ask for clarification rather than guess.

### FR-11: Recurring Tasks

Recurring tasks must have a clear recurrence rule and an anchor due date.

The agent must follow the recurrence rules in `AGENTS.md`:

- Do not create future occurrences in advance.
- Create exactly one next occurrence when the current occurrence is completed.
- Calculate the next occurrence from the original recurrence pattern, not the completion date.
- Skip missed past occurrences.
- Do not create a next occurrence merely because a task is overdue, listed, or edited.
- Ask for clarification when the recurrence schedule is underspecified.

If the recurrence pattern cannot be interpreted unambiguously, the agent must ask before creating or advancing an occurrence.

### FR-12: Due-Today Summary

After every successful task-ledger read or write, the agent must provide a concise Due today summary.

The summary must include all open tasks due on the current local date, with each task ID and title. For shopping visits, it must also include the current item list. If no open tasks are due today, the agent must say so.

The agent must not provide a stale or guessed summary after a failed or unverified operation.

## 6. Data Integrity and Error Handling

### DR-01: Preserve the Ledger

The agent must preserve the required ledger headings, format version, ID counters, record identifiers, and unrelated content.

### DR-02: Validate Before Writing

Before changing the ledger, the agent must check that:

- The required sections are present.
- The target record exists when an update, completion, reopening, or deletion is requested.
- The requested record is not ambiguous.
- The next-ID counter is safe to use when creating a record.
- Existing IDs are not duplicated.

If the ledger is malformed, contains conflicting or duplicate IDs, or cannot be interpreted safely, the agent must stop the affected operation and explain the problem. It must not silently repair unrelated data.

### DR-03: Detect Concurrent Changes

The agent must use the current file version or equivalent conflict detection when writing.

If the file changes after it was read and before the write is accepted, the agent must retrieve the latest version, reassess the requested change, and avoid overwriting intervening edits.

### DR-04: Verify Every Write

After a successful write, the agent must retrieve the affected file again and verify:

- The requested change is present.
- The file remains structurally valid.
- Unrelated records and content remain intact.
- ID counters are correct when records were created.

The agent must report failure or uncertainty accurately. A successful API response alone is not sufficient evidence that the intended content is correct.

### DR-05: Explicit Authorization

Discussion, suggestions, and hypothetical examples are not authorization to modify repository content.

The agent must make changes only when the user explicitly requests them. Destructive operations, including task deletion, require an explicit request.

## 7. Quality Attributes

The solution should remain:

- **Simple:** Avoid unnecessary files, services, dependencies, and infrastructure.
- **Portable:** Keep task data in ordinary Markdown that can be read and edited without the AI agent.
- **Transparent:** Make changes inspectable in GitHub and its file history.
- **Recoverable:** Preserve version history and avoid destructive changes where possible.
- **Understandable:** Keep instructions clear enough for a new human or AI agent to follow.
- **Honest about capability:** Describe limitations based on the connected AI client's actual access and tools.

## 8. Acceptance

A configured instance is considered operational when an AI agent can successfully complete the applicable scenarios in `docs/acceptance_tests.md`, including reading the ledger, performing authorized task operations, and verifying repository changes.

A read-only connection may pass read-related scenarios, but it does not satisfy the requirements for full task-management operation.

## 9. Known Limitations

The solution depends on the capabilities and availability of the connected AI client and GitHub integration. User interfaces, authorization flows, and permission controls may vary.

The repository defines expected behavior, but it cannot guarantee that an AI client will follow those instructions perfectly. Users should review important changes in GitHub, especially during initial setup and experimentation.
