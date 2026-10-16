# rw-pi-backpack

A Pi package with research agents, development prompts, and a dark theme.

## Prerequisites

- Install Pi with support for packages and prompt templates. Load this package in your Pi configuration.
- Configure model credentials. The main agent and the models set in `agents/*.md` must be available. Adjust the agent model settings if needed.
- Install and load `pi-subagents` for delegation:

  ```bash
  pi install npm:pi-subagents
  ```

- Add project instructions in `AGENTS.md` to define delegation, tests, and approval rules. This is recommended for the suggested workflow.
- For Linear tasks, configure an authenticated Model Context Protocol (MCP) server named `linear`. Gitea pull request tasks also need authenticated Gitea MCP tools. Neither integration is needed for local tasks or the theme.
- For code changes, provide the project's required tools and test dependencies. Git is needed for diff and commit workflows.

Keep tool permissions and confirmation controls enabled. Grant write access to external services only when needed.

## Agents

The agents in `agents/` inspect information without changing files.

Work with Pi as usual. The main agent uses your project's `AGENTS.md` instructions to select subagents when they help. You do not need to choose a model for each task. Small changes usually need no subagent. Independent research tasks can run in parallel.

You can also ask Pi to use an agent by name:

| Agent | Purpose | Example request |
| --- | --- | --- |
| `search` | Map the repository and find relevant modules and tests. | Use search to find the login flow and its tests. |
| `grep` | Find symbol definitions, references, and call sites. | Use grep to find all callers of `createSession`. |
| `docs` | Read local documentation, API definitions, and schemas. | Use docs to check the session API contract. |
| `advisor` | Review architecture, investigate repeated failures, and check changes. | Ask advisor to review the proposed cache design. |
| `linear-issue-reader` | Read a Linear issue and summarize it in German. | Use linear-issue-reader to summarize ENG-142. |

### Suggested workflow

Use this sequence: **Ticket → Research → Plan → Implement → Review → Pull request**.

1. Open your project and start `pi`. The main agent reads the project instructions and coordinates the work.
2. Describe the task or provide a Linear issue ID. Ask for research and a plan before any edits.
3. The main agent reads the issue through MCP and uses `search`, `grep`, or `docs` where useful. Ask `advisor` to check complex architecture decisions.
4. Check and approve the plan. The main agent then writes the code and runs tests.
5. If the same underlying error occurs twice, ask `advisor` to help find the cause. Use it to review complex changes before completion.
6. Check the diff and test results. Approve commits, pushes, and external updates separately. You can then ask Pi to create a Gitea pull request and link it in Linear.

Delegation requires the loaded `pi-subagents` extension and access to its tools. `AGENTS.md` alone does not start subagents. Use delegation only when it adds value.

For a feature or bug, use the prompt commands below. For an architecture question, ask directly:

```text
Analyze the session storage design. Use search and docs for context.
Ask advisor for an independent review. Show risks, alternatives, and
a recommendation. Do not change files.
```

## Prompt commands

The templates in `prompts/` guide Pi through common development tasks.

### `/breakdown`

Analyze a feature, bug, or Linear issue without changing files or external resources. Propose sub-issues with acceptance criteria, a separate Definition of Done for each item, dependencies, size, and risks. Include an execution order and an overall Definition of Done. Do not implement changes or create issues.

```text
/breakdown ENG-142
/breakdown Add team invitations with email verification
```

### `/stack-plan`

Plan pull requests for a Linear parent issue and its sub-issues without changing files or external resources. Identify dependencies, branch names, pull request bases, tests, and merge order. Use stacked pull requests only where dependencies require them. Stop for plan approval.

```text
/stack-plan ENG-142
```

### `/stack-next`

Use an approved stack plan to select one eligible sub-issue. Verify current branches and pull requests, then propose an implementation plan. Wait for approval before edits and separate approval before publication. Process only one issue per invocation. Never merge automatically.

```text
/stack-next ENG-142
```

Run `/stack-plan` first, approve its plan, then repeat `/stack-next` for each sub-issue. If you change Pi sessions, provide the approved plan again. Both commands need Linear and Gitea MCP read access.

### `/feature`

Read a feature request or Linear issue, inspect the code, and propose a plan. Ask for approval before implementation. Then implement, test, and review the change.

```text
/feature ENG-142
/feature Add keyboard navigation to the settings dialog
```

### `/bugfix`

Investigate a bug, reproduce it where possible, and propose a minimal fix. Ask for approval before editing. Then add a regression test where possible and verify the fix.

```text
/bugfix ENG-142
/bugfix Login returns 500 after password reset
```

### `/review`

Request an independent review from `advisor`. Report findings, risks, and missing tests without changing files. With no argument, review staged and unstaged changes.

```text
/review
/review current branch against main
/review https://gitea.example.org/team/project/pulls/42
```

Linear and Gitea operations need the corresponding MCP tools. The templates request separate approval before commits, pushes, or external updates. These are prompt instructions, not enforced permissions. Keep tool permissions and confirmation controls enabled.

## Cyberpunk theme

`themes/cyberpunk.json` uses neon pink, cyan, and yellow on dark panels. It colors the interface, Markdown, syntax, and HTML exports. It does not change the terminal background.

Select `cyberpunk` in `/settings`, or start Pi with:

```bash
pi --use-theme cyberpunk
```

Use a dark terminal background. Run `/reload` after changing package resources.

## Extensions and skills

`extensions/` and `skills/` are empty placeholders for future modules.
