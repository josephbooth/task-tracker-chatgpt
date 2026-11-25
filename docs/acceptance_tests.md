# Acceptance Tests

## 1. Purpose

These tests provide a practical, human-led way to check whether a newly configured AI agent can operate the ChatGPT Task Tracker as intended.

They are manual acceptance tests, not automated software tests. Run them against a personal test repository or a dedicated test branch. Do not use real personal tasks as test data.

The tests assume the agent has read `README.md`, `AGENTS.md`, `tasks.md`, and the relevant supporting documentation.

## 2. Test Preparation

Before testing:

1. Confirm that the agent is connected to the intended repository.
2. Confirm that the repository contains the expected project files.
3. Use a private test repository or a dedicated branch where practical.
4. Record the starting contents of `tasks.md`, including the next task and project IDs.
5. Confirm the agent knows the user's local date and timezone, or provide them for the test.
6. Do not proceed with write tests if the agent cannot identify the intended repository and branch.

For repeatable testing, begin with an empty ledger using the documented format.

## 3. Acceptance Scenarios

### AT-01: Read Repository Instructions

**Request**

> Read README.md, AGENTS.md, and tasks.md in this repository. Summarize the purpose of the project and identify the authoritative task ledger. Do not make changes.

**Expected result**

- The agent identifies the correct repository.
- It accurately summarizes the repository's purpose.
- It identifies `tasks.md` as the authoritative task and project ledger.
- It makes no repository changes.

### AT-02: Read an Empty Ledger

**Request**

> Show me my open tasks.

**Expected result**

- The agent retrieves the current `tasks.md`.
- It reports that there are no open tasks.
- It does not invent tasks or rely on previous conversation content.
- It provides the required Due today summary and states that no tasks are due today.

### AT-03: Create a Task

**Request**

> Add a task called "Test the task tracker" due tomorrow.

**Expected result**

- The agent interprets tomorrow using the user's local date.
- It creates one task under Open tasks.
- It uses the next available task ID.
- It populates all required fields.
- It increments the next task ID.
- It retrieves the updated ledger and verifies the task.
- It provides the required Due today summary.

### AT-04: Display the Task

**Request**

> Show me my open tasks.

**Expected result**

- The agent retrieves the latest ledger.
- It displays the new task with its ID and due date.
- It does not display the task as completed.
- It provides the required Due today summary.

### AT-05: Update a Task

**Request**

> Change the due date for "Test the task tracker" to next Friday.

**Expected result**

- The agent identifies the correct task.
- It interprets next Friday using the user's local date.
- It changes only the due-date field.
- The task ID, title, status, description, and other fields remain unchanged.
- It retrieves and verifies the updated ledger.
- It provides the required Due today summary.

### AT-06: Complete a Task

**Request**

> Mark "Test the task tracker" as completed.

**Expected result**

- The agent moves the complete record from Open tasks to Completed tasks.
- It sets the status to `completed`.
- It records the completion date using the user's local date.
- It preserves the task ID and other relevant details.
- It retrieves and verifies the updated ledger.
- It provides the required Due today summary.

### AT-07: Reopen a Task

**Request**

> Reopen "Test the task tracker".

**Expected result**

- The agent moves the record from Completed tasks to Open tasks.
- It sets the status to `open`.
- It sets the completion date to `none`.
- It preserves the task ID and other relevant details.
- It retrieves and verifies the updated ledger.
- It provides the required Due today summary.

### AT-08: Create a Shopping Visit

**Request**

> Add a grocery store visit with eggs, bread, and milk.

**Expected result**

- The agent creates a task with type `grocery_visit`.
- It places each item on a separate line in the description.
- It uses the next available task ID and increments the counter.
- It preserves existing records.
- It retrieves and verifies the updated ledger.
- It provides the required Due today summary, including the shopping items if the visit is due today.

### AT-09: Delete a Task

**Request**

> Delete the grocery store visit test task.

**Expected result**

- The agent identifies the intended task.
- It removes the record only after the explicit deletion request.
- It does not move the record to Completed tasks.
- It does not decrement or reuse the task ID counter.
- It retrieves and verifies the updated ledger.
- It provides the required Due today summary.

### AT-10: Create a Project

**Request**

> Create a project called "Home organization".

**Expected result**

- The agent creates one project under Projects.
- It uses the next available project ID.
- It sets the project status to `active`.
- It increments the next project ID.
- It preserves existing tasks and projects.
- It retrieves and verifies the updated ledger.

### AT-11: Handle Ambiguous Requests

**Request**

> Move that task to Friday.

Use this request when more than one task could be meant, or when the intended Friday is unclear.

**Expected result**

- The agent asks one focused clarification question.
- It does not guess which task or date is intended.
- It makes no change until the ambiguity is resolved.

### AT-12: Handle Read-Only Access

**Preparation**

Use a connection that can read the repository but cannot write to it, if such a test connection is available.

**Request**

> Add a task called "Read-only permission test".

**Expected result**

- The agent identifies that it cannot write to the repository.
- It does not claim that the task was created.
- It does not report a successful change.
- It explains the limitation and may provide the proposed record for manual entry.
- The ledger remains unchanged.

### AT-13: Detect a Concurrent Edit

**Preparation**

1. Ask the agent to retrieve `tasks.md` for a proposed change.
2. Before the agent writes, make a separate edit to the same file in GitHub.
3. Continue with the original request.

**Expected result**

- The agent detects that the file changed since its initial read, using the available version or conflict mechanism.
- It retrieves the latest ledger.
- It preserves the intervening edit.
- It reassesses and applies the requested change to the latest version, or stops and explains why it cannot safely proceed.
- It does not overwrite the intervening edit.

### AT-14: Handle a Malformed Ledger

**Preparation**

In a disposable test branch, alter the ledger so that a required heading is missing or a task ID is duplicated.

**Request**

> Add a task called "Malformed ledger test".

**Expected result**

- The agent detects that the ledger cannot be safely interpreted.
- It explains the specific structural problem.
- It does not write a new task or silently repair unrelated content.
- The test ledger remains unchanged by the agent.

## 4. Results Record

Record the outcome of each applicable test.

| Test ID | Result | Notes |
|---|---|---|
| AT-01 | Not run | |
| AT-02 | Not run | |
| AT-03 | Not run | |
| AT-04 | Not run | |
| AT-05 | Not run | |
| AT-06 | Not run | |
| AT-07 | Not run | |
| AT-08 | Not run | |
| AT-09 | Not run | |
| AT-10 | Not run | |
| AT-11 | Not run | |
| AT-12 | Not run | |
| AT-13 | Not run | |
| AT-14 | Not run | |

Use these result values:

- **Pass:** The observed behavior matches the expected result.
- **Fail:** The agent did not meet the expected result.
- **Blocked:** The test could not be completed because a required capability or test condition was unavailable.
- **Not run:** The test has not been attempted.

## 5. Completion Criteria

A test instance is ready for ordinary use when:

- AT-01 through AT-08 pass.
- AT-10 and AT-11 pass.
- The user understands whether the connection has read-only or read/write access.
- Any failed or blocked test relevant to the intended use is understood and accepted.

AT-09, AT-12, AT-13, and AT-14 exercise deletion, permissions, concurrency, and error handling. They should be run when the user can safely provide the required test conditions.

Do not use a test instance for real task data if basic reading, writing, or write verification is failing.
