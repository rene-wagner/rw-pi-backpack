# Stack plan for $ARGUMENTS

You are the primary Pi agent. Plan stacked pull requests for a Linear parent issue and its sub-issues. **Planning only: strictly read-only.** Use existing `search`, `grep`, `docs` and `advisor` agents as appropriate.

## Input and discovery
- Resolve `$ARGUMENTS` to a Linear parent issue ID/URL (or ask for one). Use Linear MCP read operations to fetch parent, all sub-issues, dependencies, acceptance criteria, and definitions of done. If data cannot be accessed, ask for the issue text; do not fabricate IDs.
- Read repository status, relevant git history and existing branches, and Gitea PRs (read-only) to detect existing work and base branches. Do not alter repository state.
- Use read-only research agents for code ownership and cross-issue dependencies; ask advisor to challenge the proposed dependency graph and sequencing.

## Planning rules
- Prefer independent PRs targeting main when dependencies do not require stacking. Build the shortest reasonable stacks; do not force all issues into one chain.
- Every proposed PR should be independently reviewable, narrowly scoped, and mapped to exactly one sub-issue unless an explicit exception is justified.
- Describe branch names, base branch, dependency prerequisites, proposed PR title, acceptance criteria, DoD, tests, and review risks for each issue.
- Identify critical path, parallel tracks, cross-stack integration points, and merge/rebase strategy (including squash-merge implications).
- Never assume a dependency just because issue numbers are consecutive.

## Output
1. Parent issue summary and inventory of discovered sub-issues.
2. Verified dependencies vs assumptions and questions.
3. Stack diagram (text) and table: order, issue, branch, base, blockers, size, and status.
4. For each issue: proposed PR scope, acceptance criteria, definition of done, test plan, and review considerations.
5. Merge order, restacking instructions, risks, and next recommended issue.
6. Explicitly state that no files, branches, PRs, or Linear records were modified.

STOP after presenting the plan. Do not create branches, issues, PRs, comments, commits, or code. Ask for explicit approval of the stack plan before any implementation workflow.
