# Implement next stacked issue for $ARGUMENTS

You are the primary Pi agent implementing **at most one** approved Linear sub-issue per invocation. Use existing `search`, `grep`, `docs`, `advisor` agents as needed.

## Phase 1 — Resolve and verify (read-only)
- Resolve `$ARGUMENTS` to a parent Linear issue ID/URL. Fetch its sub-issues and dependencies via Linear MCP, and inspect local git state and relevant Gitea PRs using read-only operations.
- Retrieve the approved stack plan from the current conversation or a user-supplied plan. If unavailable or not clearly approved, propose/verify the plan and STOP for approval. Do not assume earlier chat history is persisted.
- Reconcile actual branches, PR bases, merged status and issue states with the approved plan. Do not trust stale plans. Never overwrite uncommitted changes.
- Select one eligible sub-issue based on dependencies, approval and existing work. If multiple eligible items exist, propose a choice and ask the user.
- Determine its exact base branch and whether the dependency PR exists and is ready. If the base is missing, ambiguous, or stale, STOP and ask.

## Phase 2 — Plan and approve (read-only)
- Read the chosen issue's acceptance criteria and definition of done. Research only relevant code, using subagents selectively.
- Propose a focused implementation plan: chosen issue, parent/base branch, target branch, files, tests, risks, review strategy, and planned Gitea/Linear actions.
- Ask `advisor` to review complex architecture before finalizing the plan.
- **STOP for explicit user approval before any checkout, branch creation, file edit, dependency installation, commit, push, PR creation or Linear write.**

## Phase 3 — Implement approved single issue
- Verify the repository has no conflicting uncommitted work; do not discard user changes. Create the approved branch from the approved base only after permission.
- Implement only this issue, with targeted tests and checks. Keep the change small and reviewable.
- After two failed attempts on the same underlying error, consult `advisor` with exact diagnostics before trying a different fix. Seek renewed approval for scope changes.
- Review the final diff and check all acceptance criteria and DoD items. For complex changes, request an independent `advisor` review; address blockers and rerun tests.
- Present the actual diff summary, test outcomes, review findings, remaining risks and proposed PR metadata.

## Phase 4 — Publish only after separate approval
- **STOP for a second explicit approval** before commit, push, opening/updating a Gitea PR, or updating/commenting on Linear. Never merge automatically.
- After approval, publish only the approved changes, target the specified parent branch (not necessarily main), and link the Linear issue. Verify the actual PR base and report its URL.
- End the workflow after this single issue. Never automatically start the next sub-issue.

## Stack safety
- Prefer merging from the bottom of a stack upward. Before restacking, re-evaluate PR bases and merge strategy; never force-push, rebase published branches, delete branches or retarget PRs without explicit approval.
- Never claim a tool action, review or test occurred unless confirmed.
- These are prompt-level gates, not a technical sandbox. Respect tool-level permissions.
