# Task Ledger Format

The `tasks.md` file is the authoritative source for task and project state.

## 1. Required Structure

The ledger begins with its format version and next available identifiers:

```markdown
Task Ledger

Format version: 1
Next task ID: T-0001
Next project ID: P-0001
```

It must contain these headings in this order:

1. `Projects`
2. `Open tasks`
3. `Completed tasks`

Keep the headings even when a section has no records.

## 2. Identifiers

- Task IDs use the prefix `T-` followed by a zero-padded number.
- Project IDs use the prefix `P-` followed by a zero-padded number.
- IDs are stable and unique within their record type.
- Increment the corresponding next-ID counter after creating a record.
- Never decrement a counter or reuse an ID, including after deletion.

## 3. Project Records

A project record contains a unique ID, title, status, and optional description.

Example:

```markdown
P-0001 — Example project

* Status: active

Description:
A short description of the intended outcome.
```

Supported project statuses are `active` and `completed`.

A project groups related tasks. It is not a substitute for individual actionable task records.

## 4. Task Records

A task record contains:

- Unique task ID and title.
- Type: `task` or `grocery_visit`.
- Status: `open` or `completed`.
- Due date or `none`.
- Project reference or `none`.
- Repeat rule or `none`.
- Completion date or `none`.
- Optional multiline description.

Example:

```markdown
T-0001 — Review the project plan

* Type: task
* Status: open
* Due: 2026-12-01
* Project: P-0001 — Example project
* Repeat: none
* Completed: none

Description:
Review the current plan and record any required changes.
```

For a task without a due date, use `Due: none`.

For a task that is not associated with a project, use `Project: none`.

For a task that does not repeat, use `Repeat: none`.

For a task that is not complete, use `Completed: none`.

## 5. Shopping Visit Records

Use `Type: grocery_visit` for a shopping trip with a list of items.

Example:

```markdown
T-0002 — Grocery store visit

* Type: grocery_visit
* Status: open
* Due: 2026-12-02
* Project: none
* Repeat: none
* Completed: none

Description:

* Item one
* Item two
* Item three
```

Create a separate record for each shopping trip. Do not replace the items from an earlier visit when creating a new one.

## 6. Completing and Reopening Tasks

When completing a task:

1. Move the complete record from Open tasks to Completed tasks.
2. Change `Status: open` to `Status: completed`.
3. Set `Completed: YYYY-MM-DD`.
4. Preserve the task ID and relevant details.

When reopening a task:

1. Move the record from Completed tasks to Open tasks.
2. Change `Status: completed` to `Status: open`.
3. Set `Completed: none`.
4. Preserve the task ID and relevant details.

Completion and deletion are different operations. Deleting a record removes it from the ledger; completing a record preserves its history.

## 7. Dates and Times

- Store due dates as calendar dates in `YYYY-MM-DD` format.
- Do not store a time of day in the due-date field.
- Put a time of day in the task title or description.
- Use the user's local timezone when determining which tasks are due today.
- Ask for clarification when a natural-language date has more than one reasonable interpretation.
