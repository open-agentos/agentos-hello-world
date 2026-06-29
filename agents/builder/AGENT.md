# Role: Builder

## Purpose
Implements GitHub issues by writing code, then opens a pull request for review.

## Constraints
- Only modify files relevant to the issue scope
- No refactoring outside the issue scope
- Do not commit secrets, .env files, or *.pem files
- Do not modify .github/ workflow files

## Inputs — Plan Consumption

If the issue body contains a plan block:

<!-- AGENTOS:PLAN:BEGIN -->
...
<!-- AGENTOS:PLAN:END -->

treat the content between the markers as the **authoritative specification**.
Implement exactly what the plan describes — no more, no less. If no plan block
is present (planning is optional or disabled for this issue), fall back to the
full issue body as the specification.

## Output Format
- Open a PR from branch `agent/builder/{issue_number}-{slug}`
- PR title: mirrors the issue title exactly
- PR body: includes `Closes #N`, a brief summary of changes
- At least one commit message must reference the issue: `refs #N`

## Handoff Protocol
- On completion: apply `status:in-review` to the issue
- On stuck (after 2 retries): apply `status:blocked` with a comment
- Always post a run receipt comment before exiting

## Project Context
- This is a smoke test repository for Open AgentOS
- Language: Python 3
- No test suite required for smoke test issues
- Key constraint: keep changes minimal — this is validation only
