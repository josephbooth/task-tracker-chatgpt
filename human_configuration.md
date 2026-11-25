# Human Configuration Guide

This guide walks you through setting up the ChatGPT Task Tracker for the first time.

## 1. Understand the Two Parts

The project has two distinct stages:

- **Initial configuration:** Use a desktop browser to set up the GitHub repository and connect it to ChatGPT.
- **Everyday use:** Use the ChatGPT mobile app to ask the assistant to manage your tasks in `tasks.md`.

The mobile app is the intended day-to-day interface. You should not need to edit files directly in GitHub for routine task management.

A desktop browser is recommended for setup because GitHub repository settings, app authorization, and permission controls are easier to locate and review on a larger screen. Some steps may be possible on a mobile browser, but the exact interface can vary.

## 2. Create Your Repository

1. Sign in to [GitHub](https://github.com/) using a desktop browser.
2. Open the ChatGPT Task Tracker repository.
3. Select **Fork** to create your own copy under your GitHub account. Alternatively, create a new repository and copy the project files into it.
4. If the repository will contain personal tasks, set its visibility to **Private**.
5. Confirm that your repository contains at least:
   - `README.md`
   - `AGENTS.md`
   - `tasks.md`

Keep your personal task ledger in your own repository, not in the original project repository.

## 3. Connect GitHub to ChatGPT

1. Open [ChatGPT](https://chatgpt.com/) in a desktop browser and sign in to the account you intend to use.
2. Open **Settings** and find **Apps** (the label or location may vary by ChatGPT version and account).
3. Find and select the GitHub app/integration.
4. Choose the option to connect it. Follow the prompts to sign in to GitHub and authorize the connection.
5. If GitHub asks which repositories the integration may access, grant access to the repository you created for your task tracker. Prefer limiting access to that repository rather than granting access to all repositories, if that choice is available.
6. Complete the authorization flow and return to ChatGPT.

The GitHub authorization screen and available permission choices are controlled by GitHub. Review the requested access before approving it. Do not grant broader access than you are comfortable with.

## 4. Verify Repository Access

A successful connection does not necessarily mean the assistant can both read and modify repository files. Permissions can vary by account, app configuration, repository, and current ChatGPT session.

Start a ChatGPT conversation and ask the assistant to:

> Read the README.md, AGENTS.md, and tasks.md files in my task-tracker repository. Report whether you can read them. Do not make any changes.

Confirm that the assistant identifies the correct repository and accurately reports the current ledger structure.

Next, test write access with a harmless, temporary change only if you are comfortable doing so. For example, ask the assistant to add a clearly labeled temporary test task, verify that it appears in `tasks.md`, and then ask it to remove that test task. Confirm that each change is reflected in GitHub. You can also inspect the file's commit history in GitHub.

If the assistant can read but cannot write, revisit the GitHub app's repository access and permissions. Confirm that the integration has the required contents/write permission for the selected repository, if GitHub exposes that permission setting. Reconnect or reauthorize the integration if necessary. Do not assume write access until a change has been verified.

## 5. Use the ChatGPT Mobile App

Once setup and verification are complete:

1. Open the ChatGPT app on your iPhone or other supported mobile device.
2. Sign in to the same ChatGPT account used to connect GitHub.
3. Start a conversation and identify your task-tracker repository.
4. Ask the assistant to read the repository instructions and current task ledger before managing tasks.

Example starting prompt:

> Use my GitHub task-tracker repository. Read README.md, AGENTS.md, and tasks.md. Follow the repository instructions. Show me my open tasks.

You can then use ordinary language to manage the ledger. For example:

- "Add a task to renew my vehicle registration next Friday."
- "What tasks are due today?"
- "Mark the vehicle registration task as completed."
- "Show me my grocery list."

The assistant should retrieve the latest `tasks.md` before answering questions about task state or changing tasks. After a change, it should verify the updated file. Repository instructions in `AGENTS.md` define the expected workflow.

## 6. Important Usage and Privacy Notes

- Use the ChatGPT mobile app as the normal interface for interacting with the assistant and your task ledger.
- GitHub remains the storage location and authoritative source for task data.
- The assistant's ability to read or change files depends on the connected integration and the permissions available in that session.
- Changes made directly in GitHub are reflected in the repository. The assistant should fetch the latest file rather than rely on an old conversation or cached copy.
- A private repository limits public visibility, but the task content is still processed by the AI provider when the assistant accesses it. Review the privacy terms and policies that apply to your account.
- Never put passwords, access tokens, financial account credentials, government identifiers, or other highly sensitive information in `tasks.md`.

## 7. Troubleshooting

**The GitHub app is not listed in ChatGPT Settings**

Check the available apps for your account and plan, and confirm that the integration is supported in your region. Product labels and availability can change.

**ChatGPT can see the repository but cannot find the files**

Confirm that the correct GitHub account and repository were connected, and that the files exist in the repository's default branch.

**ChatGPT can read files but cannot update them**

Reading and writing are different capabilities. Review the GitHub app's repository selection and requested permissions, then reconnect or reauthorize if needed. Verify any permission change with a small test rather than assuming it worked.

**The mobile app cannot access the repository**

Confirm that the mobile app is signed in to the same ChatGPT account where GitHub was connected. Check the available app/integration settings and try a new conversation. If necessary, complete or repair the connection in a desktop browser.

**The assistant reports that it cannot perform a requested operation**

Ask it to identify the specific access limitation. It should not claim a task was changed unless it can verify the updated content in GitHub.
