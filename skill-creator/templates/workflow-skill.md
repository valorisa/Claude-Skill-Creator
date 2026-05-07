# PR Reviewer

Perform a structured pull request review with security, performance, and maintainability analysis.

## Trigger

Use when the user says "review this PR", "check this pull request", `/pr-review`, or provides a PR URL or diff to review.

## Process

1. Read the PR diff (from URL via `gh`, from local branch via `git diff`, or from pasted content)
2. Analyze in four passes:
   - **Security**: injection risks, exposed secrets, auth bypasses, OWASP top 10
   - **Performance**: N+1 queries, unnecessary allocations, missing indexes, blocking calls
   - **Correctness**: logic errors, edge cases, race conditions, missing null checks
   - **Maintainability**: naming clarity, function length, duplication, test coverage
3. For each finding, state:
   - Severity (critical / warning / suggestion)
   - File and line reference
   - The problem in one sentence
   - A concrete fix or alternative
4. Generate a summary with counts: "X critical, Y warnings, Z suggestions"
5. Ask: "Want me to post these as inline PR comments, or keep as a report?"

## Constraints

- Do not comment on pure style preferences (tabs vs spaces, trailing commas)
- Do not suggest changes unrelated to the PR's intent
- If the PR is clean, say so clearly — do not invent issues to appear thorough
- Never auto-post comments without user confirmation
- Limit to 15 findings maximum — prioritize by severity

## Output

A structured review report sorted by severity (critical first), with optional GitHub PR comment posting via `gh` CLI.
