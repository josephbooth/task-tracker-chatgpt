# ChatGPT Task Tracker

A lightweight, file-based task tracker designed for use by an individual with ChatGPT, Codex, or another AI assistant that can access the repository through an authorized integration.

The repository uses a Markdown file as its authoritative task and project ledger. No database or external task-management service is required.

## 1. Getting Started

1. Fork this repository or create your own copy.
2. Review `AGENTS.md` and customize any workflow rules you want to change.
3. Open `tasks.md` and add your first task or project.
4. Connect an AI client to your GitHub account and grant it the repository permissions you want it to have.
5. Ask the AI client to read `README.md`, `AGENTS.md`, and `tasks.md` before managing the ledger.

A useful starting prompt is:

> Read the repository's README.md, AGENTS.md, and tasks.md. Follow the repository instructions and help me manage my tasks.

The AI client must retrieve the current repository files. It must not rely on previous conversations, remembered task state, or old local copies.

## 2. Repository Structure

- `README.md`: project overview and getting-started guidance.
- `AGENTS.md`: operating instructions for AI assistants and contributors.
- `tasks.md`: authoritative task and project ledger.
- `docs/task_ledger_format.md`: ledger structure and record definitions.
- `docs/assistant_workflow.md`: read, create, update, delete, and verification workflow.

## 3. How It Works

The `tasks.md` file contains three sections:

1. Projects
2. Open tasks
3. Completed tasks

Each task has a stable ID. Projects also have stable IDs. Counters at the top of the ledger identify the next available IDs.

The ledger is intentionally simple: it is readable and editable in GitHub, a text editor, or an AI client with repository access.

## 4. Task Management

An AI assistant can help with common operations when the current client has the necessary repository permissions:

- Show open tasks, overdue tasks, or tasks due on a specified date.
- Add tasks and projects.
- Update task details or due dates.
- Complete tasks while preserving their history.
- Reopen completed tasks.
- Delete tasks when explicitly requested.
- Maintain optional shopping or errand lists using the `grocery_visit` task type.

The assistant must read the latest ledger before answering task-state questions or making changes. After a write, it must retrieve the updated file and verify the result.

## 5. GitHub Permissions

Repository capabilities vary by AI client, account connection, session, and repository permissions. A client may be able to read files without being able to write them.

Do not assume that an AI assistant can perform an operation. It should use the capabilities available in the current session, report limitations accurately, and never claim a change succeeded unless it has verified the resulting repository content.

For safer experimentation, users may choose to work through a dedicated branch and review changes before merging them.

## 6. Privacy and Public Repositories

A task ledger can contain personal information. Consider the repository's visibility and the AI services that will process its contents before adding sensitive details.

If the repository is public, its contents may be visible to anyone. Do not store passwords, authentication tokens, financial account details, government identifiers, or other sensitive information in the ledger.

When an AI client reads task data, relevant content is processed by that AI provider. Review the provider's privacy terms and your organization's policies before connecting an account or repository.

## 7. Documentation

See the `docs/` directory for the ledger format and the assistant workflow.

This project provides a simple repository-based workflow. It does not provide a hosted application, automatic reminders, background scheduling, or guaranteed synchronization with other task systems.
