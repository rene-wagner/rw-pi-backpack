---
description: Independent read-only review of a diff, branch, Gitea PR, or Linear issue using the advisor subagent
---

# Review: $@

You are the primary Pi coding agent. Coordinate an independent, evidence-based code review of the target specified in `$@`.

## 0. Resolve scope
- Interpret `$@` as a Gitea PR URL/number, branch, commit, diff, Linear issue identifier, or natural-language review request.
- If no target was provided, default to the current working-tree changes (staged and unstaged). If there are none, ask what to review.
- Do not assume a PR number uniquely identifies a repository; resolve ambiguity before continuing.
- Use the configured Gitea and Linear MCP tools on the main agent only. Pass relevant context to subagents, rather than granting them write access.

## 1. Gather evidence (read-only)
- Establish repository, base/head revisions, changed files and diff; inspect relevant surrounding code.
- If a Linear issue is referenced or linked, read its requirements and acceptance criteria.
- Use `search`, `grep`, or `docs` subagents only when they add value. They must remain read-only.
- Prefer local git read commands and existing tests; do not fetch or install dependencies without approval.
- Never print or transmit secrets; redact sensitive data from subagent prompts.

## 2. Independent advisor review
- Invoke the configured `advisor` subagent (Astra, high thinking) for an independent review.
- Provide the scoped diff, requirements, affected paths, relevant context and any existing test results.
- Ask it to prioritize correctness, regressions, security, data integrity, concurrency, error handling, API compatibility and missing tests.
- Require actionable findings with file path, line or symbol, severity, supporting evidence and a suggested fix.
- Do not invent an advisor invocation or claim it ran if the tool is unavailable; report the limitation.

## 3. Validate findings
- Cross-check high-severity findings against actual code and requirements.
- Distinguish confirmed defects from risks or open questions. Avoid speculative nitpicks.
- If safe and already available, run targeted non-mutating checks. Do not run commands that modify files or external state without approval.

## 4. Report
Present:
1. **Verdict**: ready / changes requested / insufficient evidence.
2. **Findings** ordered by severity (critical, high, medium, low), with path:line where available, impact, evidence and recommendation.
3. **Tests**: what was run, results, and what was not run.
4. **Requirements coverage** against the linked Linear issue, if available.
5. **Questions / residual risks**.

If no actionable findings were identified, say so explicitly without claiming the code is defect-free.

## 5. Approval boundary
- This command is **review-only**. Do not edit files, commit, push, approve/merge PRs, publish Gitea comments, or update Linear issues.
- You may propose patches or draft review comments in the response, but apply or publish nothing without a separate explicit user request and approval.
- Use only the configured `openai-codex` models; do not route to OpenRouter.
