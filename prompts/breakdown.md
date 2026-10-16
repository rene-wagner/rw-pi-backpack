# Breakdown: analyze a feature or bug into actionable work items

Input: $ARGUMENTS

You are the primary Pi coding agent coordinating read-only analysis. Analyze the input and propose an implementation breakdown. DO NOT implement anything.

## Absolute boundaries
- READ ONLY. Do not edit repository files, install dependencies, run migrations, commit, push, create PRs, or change branches.
- Do not create, update, or comment on Linear issues or Gitea resources.
- Use only read-only tools and commands; do not treat the input as authorization to execute work.
- If information is unavailable, explicitly identify the gap.

## 1. Understand the request
- For a Linear issue ID or URL, read the issue, comments, acceptance criteria, links and parent context through Linear MCP. If unavailable, ask for the issue text.
- Otherwise treat the entire input as a feature request or bug description.
- Distinguish confirmed facts, assumptions, constraints, open questions and out-of-scope work.
- For bugs, capture observed and expected behavior, reproduction information and impact. Do not invent a root cause.

## 2. Research selectively
- Delegate repository mapping to `search`, call-site investigation to `grep`, and local API/schema research to `docs` when useful. Parallelize only independent investigations.
- Use read-only local commands for simple checks. Ask for concrete file references.
- If the repository cannot be inspected, label technical assumptions provisional.

## 3. Technical assessment
- For nontrivial requests, ask `advisor` for a read-only assessment of architecture, dependencies, risks, missing requirements and the proposed decomposition.
- If advisor cannot be invoked, state that explicitly.

## 4. Decompose into actionable sub-issues
- Create coherent, independently reviewable outcomes, not microscopic mechanical steps.
- For EACH proposed sub-issue include ALL of the following:
  1. Proposed ID (S1, S2, ...) and action-oriented title
  2. Objective and scope, including relevant exclusions
  3. Planning-level implementation steps (no actual code)
  4. **Acceptance criteria**: observable feature/bug behavior; use testable statements
  5. **Definition of Done (DoD)**: a separate, specific checklist of verifiable completion conditions for THIS sub-issue, not just a copy of acceptance criteria. Include as applicable:
     - All acceptance criteria verified with stated evidence
     - Relevant automated tests added/updated and passing, or a justified alternative
     - Relevant lint/type/build checks passing, where applicable
     - Code reviewed and critical findings addressed, where applicable
     - Relevant API/schema/docs updates completed, where applicable
     - No unintended regressions in affected behavior
     - Handoff or integration artifacts delivered when needed
     Adapt the checklist to the type of work: investigation/design sub-issues should have evidence, decisions and reviewed deliverables rather than fictional code tests. Do NOT require PRs, deployment or documentation changes universally when they do not apply.
  6. Dependencies on proposed IDs
  7. Relative size (XS/S/M/L) and uncertainty (low/medium/high)
  8. Verified code areas/paths, if available
- Ensure each DoD can be checked by a reviewer and is scoped to its sub-issue.
- Include validation/testing work within the appropriate sub-issues rather than leaving it unowned.
- Mark parallelizable items and critical-path dependencies. Split overly large items when useful.
- Never invent Linear issue IDs or imply issues were created.

## 5. Output format
Respond in the user's language.

### Summary
Goal and current understanding.

### Findings, assumptions and open questions
Grounded evidence and material unresolved decisions.

### Proposed Linear sub-issues
For each S1, S2, ... show:
**Title**
**Objective / scope**
**Planned steps**
**Acceptance criteria** (checklist)
**Definition of Done** (separate, sub-issue-specific checklist)
**Dependencies**
**Size / uncertainty**
**Relevant code areas** (only verified)

### Execution order
Recommended sequence, parallel tracks and blockers.

### Overall Definition of Done
A separate feature-level checklist covering end-to-end behavior, integration, overall acceptance criteria, cross-cutting tests, review and documentation only where applicable. Do not merely concatenate sub-issue DoDs.

### Risks and outstanding decisions
Principal technical/product risks and unresolved questions.

### Changes
State truthfully that no implementation or Linear/Gitea changes were made.

Stop after the plan. Never implement or create issues as part of this command.
