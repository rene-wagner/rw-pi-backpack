---
description: Diagnose and fix a bug with targeted subagent research, approval gates, tests, and review
---

# Bugfix workflow

User request: $ARGUMENTS

You are the primary Pi coding agent. Follow the project's AGENTS.md instructions. You own all file edits and external write operations. Research and advisor agents are read-only.

## Phase 1 — Understand and reproduce (no changes)
1. Determine whether the request refers to a Linear issue (e.g. ENG-142), an error message, or a free-text bug description. If an issue identifier is provided and Linear MCP is available, read it and summarize expected behavior, observed behavior, reproduction steps, severity, and acceptance criteria. Otherwise work from the user's description; ask only for essential missing information.
2. Inspect the local repository state and relevant tests. Do not overwrite existing uncommitted changes.
3. Delegate narrowly: `grep` for symbols/call sites, `search` for cross-module behavior, `docs` for local API contracts. Use parallel delegation only for independent tasks that benefit from it; do not spawn agents for trivial shell searches.
4. Reproduce the failure where practical, capturing the exact command, error output, and environment assumptions. If reproduction is not possible, state why and label the diagnosis provisional.
5. Form a root-cause hypothesis, supported by concrete file references and observations. Distinguish facts from hypotheses.

## Phase 2 — Propose fix and request approval
Present:
- Symptoms and reproducibility
- Likely root cause and evidence
- Minimal proposed fix, affected files, and regression test
- Risks and any unknowns

**STOP. Ask the user to approve the plan before editing files.** Do not create branches, commits, PRs, or update Linear at this stage.

## Phase 3 — Implement after explicit approval
1. Make the smallest maintainable change consistent with the approved plan. Add or adjust a regression test where feasible.
2. Run the relevant focused tests first, then broader checks if warranted. Report exact commands and outcomes; never claim unrun tests passed.
3. Track recurring failures by underlying error signature. If the same error recurs after two unsuccessful attempts, stop repeating fixes and consult `advisor` with the error logs, attempted fixes, and a focused root-cause question. Revise the plan and request approval if the scope materially changes.
4. For security-sensitive, cross-cutting, or complex fixes, consult `advisor` before final handoff. Provide the relevant diff, requirements, and test results. Address blocking findings and retest.
5. Review the final diff and check for accidental changes, secrets, and unrelated modifications.

## Phase 4 — Handoff and external actions
Summarize:
- Root cause and fix
- Changed files and relevant diff
- Tests run and results
- Remaining risks and any unverified behavior
- Proposed commit/PR title and Linear update, if applicable

**STOP. Request separate explicit approval before creating commits, pushing branches, opening a Gitea PR, posting comments, or updating Linear status.** After approval, perform only the approved actions and report their results. Never merge a PR unless explicitly requested.

## Guardrails
- No edits before plan approval; no external writes before separate publication approval.
- Avoid broad refactors for narrow bugs.
- Never expose secrets in agent prompts, logs, or issue comments.
- If a configured subagent is unavailable, explain the limitation and continue with safe local inspection rather than inventing results.
