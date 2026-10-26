# AI-Assisted Coding Workshops — Project Template

Starting point for the hands-on project in the **AI Augmented Delivery Training**. Your team builds a **Planning Poker** app (private voting, reveal together, agree an estimate) with the help of GitHub Copilot.

You define the scope. Tasks and backlog are created by your team during Backlog Preparation, and every change produced by the AI agent is reviewed by you.

## Prerequisites

- Access to the `KainosSoftwareLtd` GitHub organisation and the [GitHub CLI](https://cli.github.com/)
- Latest [VS Code](https://code.visualstudio.com/)
- GitHub Copilot licence and the GitHub Copilot Chat extension installed in VS Code
- Any modern web browser

Preflight check:

```shell
gh auth status
gh repo list KainosSoftwareLtd --limit 1
```

Both commands must succeed, and Copilot Chat must reply to `Hello`.

## Getting started

1. Click **Use this template** → **Create a new repository** and create it under your own GitHub account.
2. Clone your new repository (not the template):

   ```shell
   git clone https://github.com/<your-username>/<your-repo-name>
   cd <your-repo-name>
   ```

3. Open the folder in VS Code and start Copilot Chat.

## API contract

The [backend API contract](docs/api-contract.md) defines the Planning Poker WebSocket protocol, room and voting behavior, and the `/health` route. Use it as the shared interface when planning and implementing the frontend and backend.

## How we work

- Slice work into small, testable, vertical slices delivered end to end.
- Work on a branch per task and review every AI-generated change before merging.
- The final scope and tech stack are decided by your team during planning.
