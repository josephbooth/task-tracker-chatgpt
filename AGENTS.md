# Agent Instructions

These instructions apply to AI assistants and other automated agents working in this repository.

## 1. Start Here

At the beginning of a new conversation or work session:

1. Read `README.md`.
2. Read `tasks.md`.
3. Read this file and the relevant documents in `docs/`.
4. Treat `tasks.md` as the authoritative source for task and project state.
5. Ask one focused question if an important detail is unclear.

During the same conversation, retain the instructions already read. Retrieve the latest `tasks.md` before answering a task-state question or changing task data because the repository may have been modified outside the conversation.

## 2. Repository Access and Authorization

Use only the GitHub and file access available in the current client. Do not assume that read access implies write access.

Before changing repository content:

1. Retrieve the latest version of each affected file.
2. Confirm that the user explicitly requested the change.
3. Identify the specific records or files affected.
4. Preserve unrelated records, content, identifiers, and formatting.
5. Make only the requested changes.
6. Write to the appropriate branch.
7. Retrieve the updated file and verify the result.

Ideas, observations, and proposed rules are discussion, not authorization to edit repository content. If it is unclear whether an edit was requested, ask before making it.

Never claim a change succeeded unless the repository confirms the write and the resulting content has been verified.

If write access is unavailable, explain the limitation and provide the proposed changes for the user to apply. Do not create reminders or automations as substitutes for repository changes.

## 3. Task Operations

- **Show or find:** Retrieve the current ledger. By default, show all open tasks, dated tasks in due-date order, overdue tasks included, and undated tasks last. Omit completed tasks unless requested.
- **Create:** Add a task or project only when explicitly requested. Use the next available ID and increment the relevant counter.
- **Update:** Modify only the fields the user asked to change. Preserve the record ID and unrelated details.
- **Complete:** Move the full task record from Open tasks to Completed tasks, set its status to `completed`, and set its completion date. Preserve its ID and history.
- **Reopen:** Move the record back to Open tasks, set its status to `open`, and clear its completion date. Preserve its ID.
- **Delete:** Remove a record only when the user explicitly requests deletion. Never treat deletion as completion and never reuse a deleted ID.

Ask one focused question when ambiguity could affect the record, date, recurrence, or requested operation.

## 4. Task and Project Rules

- A project groups related tasks; it is not itself an actionable task.
- Give each project a stable ID.
- Put each actionable step in its own task record.
- Associate tasks with a project when appropriate.
- Due dates are optional and use `YYYY-MM-DD`.
- Do not put a time of day in the due-date field. Put times in the title or description.
- Use `none` for optional fields without a value.
- Preserve the three required ledger headings: Projects, Open tasks, and Completed tasks.
- Never decrement ID counters or reuse IDs.
- If a counter is behind an ID already in the ledger, advance it to the next unused number before creating a record.

### Repeating Tasks

- Record a clear recurrence rule and an anchor due date.
- Ask for clarification when the schedule is underspecified.
- Do not create future copies in advance.
- When the current occurrence is completed, create exactly one next occurrence.
- Calculate the next occurrence from the original recurrence pattern, not the completion date.
- Skip missed past occurrences.
- Do not create a next occurrence merely because a task is overdue, listed, or edited.

### Shopping Lists

Use a separate `grocery_visit` task for each shopping trip. Keep its items as separate lines in the description. Do not overwrite an earlier visit to represent a new trip. A recurring visit starts with an empty list unless the user asks to carry items forward.

## 5. Due Today Summary

After every successful task-ledger read or write, include a brief section titled **Due today**.

The summary must:

- Include all open tasks due on the current date in the user's local timezone.
- Include each task ID and title.
- Include the current item list for `grocery_visit` tasks.
- State explicitly when no open tasks are due today.

For writes, retrieve and verify the updated ledger before producing the summary. If an operation fails or requires clarification, do not provide a stale or guessed summary.

## 6. Data Integrity

- `tasks.md` is the only authoritative task and project ledger.
- Do not rely on previous conversations, saved memories, or cached copies for current task state.
- Preserve unrelated records and descriptions.
- Avoid duplicate records.
- Follow the existing record format.
- Verify every write by retrieving the affected file and checking the requested change and surrounding structure.
- Do not add databases, applications, task-data files, or unrelated documentation unless the user requests them.

## 7. Scope

Keep task-management work focused on this repository. Do not introduce unrelated project context unless the user explicitly asks.
