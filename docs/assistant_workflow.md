# AI Assistant Workflow

This document describes the expected workflow for an AI assistant managing the repository.

## 1. Read Before Answering

Before answering a question about task state:

1. Retrieve the current `tasks.md` from the repository.
2. Interpret the current records, not information from previous conversations.
3. Apply the display rules in `AGENTS.md`.

For a default task overview, include all open tasks. Sort dated tasks by due date, include overdue tasks, and place undated tasks last. Exclude completed tasks unless the user asks for them.

## 2. Read Before Writing

Before changing a task or project:

1. Retrieve the latest `tasks.md`.
2. Confirm the user explicitly requested the operation.
3. Identify the exact record and fields involved.
4. Check the relevant ID counter if creating a record.
5. Preserve all unrelated records and formatting.

Do not make a broader change than the user requested.

## 3. Create

For a new task:

1. Use the next available task ID.
2. Add the record under Open tasks.
3. Set the required fields.
4. Increment the next task ID.

For a new project:

1. Use the next available project ID.
2. Add the record under Projects.
3. Set its status and description.
4. Increment the next project ID.

Ask for clarification if required information cannot be safely inferred.

## 4. Update

Preserve the task or project ID. Change only the requested fields. Do not silently change a task's status, due date, project, recurrence, or description.

## 5. Complete, Reopen, and Delete

These operations are distinct:

- Complete a task by moving it to Completed tasks, setting its status to completed, and recording the completion date.
- Reopen a task by moving it to Open tasks, setting its status to open, and clearing the completion date.
- Delete a task only when deletion is explicitly requested.

Never reuse an ID after deletion.

## 6. Write and Verify

When repository write access is available:

1. Write the updated file to the appropriate branch.
2. Retrieve the file again from GitHub.
3. Verify the requested change.
4. Verify that unrelated records remain intact.
5. Report the result accurately.

If the write fails or verification is not possible, say so. Do not claim success.

When write access is unavailable, explain the limitation and provide the exact proposed edit for the user to apply.

## 7. Due Today

After every successful task-ledger read or write, provide a concise **Due today** summary based on the current ledger and the user's local timezone.

Include each open task's ID and title. For `grocery_visit` tasks, include the current item list. Explicitly state when no open tasks are due today.

Do not provide a stale summary after a failed or unverified write.
