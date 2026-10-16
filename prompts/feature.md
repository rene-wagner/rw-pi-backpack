---
description: Implement a Linear feature using research agents, architecture review, and approval gates
argument-hint: "<Linear issue ID or feature description>"
---
# Feature workflow

Feature request: $@

If no issue ID or feature description was supplied, ask for one and stop.

You are the primary implementation agent. Follow the existing AGENTS.md policies and use configured subagents when available. Never pretend that an agent or MCP tool was invoked if it was not.

## Phase 1 — Understand (read-only)
1. If the request identifies a Linear issue, fetch it using Linear MCP. Capture scope, acceptance criteria, constraints, links, and open questions. If Linear MCP is unavailable, ask for the issue text rather than inventing requirements.
2. Inspect repository status, existing changes, project conventions, relevant tests, and architecture. Do not overwrite unrelated work.
3. Delegate meaningful independent investigations to `search`, `grep`, and `docs` as needed; run them in parallel only when beneficial. Do not delegate trivial filesystem lookups. Keep subagents read-only.
4. Resolve uncertainty with focused questions rather than assuming requirements.

## Phase 2 — Plan and approve (read-only)
5. Produce a concise plan: requirements, proposed design, affected files, implementation steps, tests, risks, and any unresolved decisions.
6. For complex architecture or high-risk changes, consult `advisor` BEFORE finalizing the plan. Include its material concerns and your responses in the plan. If advisor is unavailable, say so.
7. STOP and explicitly request approval to implement. Until approval, do not edit files, install dependencies, create branches, commit, push, create PRs, or update Linear/Gitea.

## Phase 3 — Implement (only after explicit approval)
8. Implement the approved scope in small, verifiable increments. The primary agent owns all writes. Avoid unrelated refactors.
9. Run relevant tests, linters, and type checks. Report actual results, never assumed results.
10. If the same underlying failure occurs twice, stop blind retries and consult `advisor` with the error, attempts, and evidence; if unavailable, report that and propose a new diagnostic approach.
11. For material changes outside the approved plan, explain the change and obtain renewed approval before proceeding.

## Phase 4 — Review and handoff
12. Inspect the final diff and compare against the acceptance criteria. For complex changes, ask `advisor` for an independent read-only review; address blockers and rerun affected tests.
13. Summarize: files changed, behavior, acceptance criteria met/unmet, tests and results, advisor findings, risks, and the proposed commit/PR details.
14. STOP and ask separately for permission before any git commit, push, branch publication, Gitea PR creation, Linear status update, or external comment. Do not claim these actions occurred unless tools confirm success.

## Rules
- Use the configured subagents when available.
- Never expose secrets or send unnecessary repository contents to external services.
- Use the configured MCP tools only within their permissions.
- When the task is simple, keep delegation minimal, but preserve the approval gates.
