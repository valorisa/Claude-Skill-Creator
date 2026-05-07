# Commit Message Writer

Generate clear, conventional commit messages from staged changes.

## Trigger

Use when the user says "write commit", "commit message", `/commit-msg`, or asks for help with a git commit message.

## Process

1. Run `git diff --staged` to read the staged changes
2. Identify the nature of the change: new feature, bug fix, refactor, docs, test, chore
3. Write a commit message following Conventional Commits format:
   - Type prefix (feat, fix, refactor, docs, test, chore)
   - Optional scope in parentheses
   - Imperative mood subject line under 72 characters
   - Blank line then body if the change needs explanation
4. Present the message and ask: "Use this, or adjust?"

## Constraints

- Never commit automatically. Only generate the message.
- Do not invent changes that are not in the diff.
- If the diff is empty, say so and stop.
- Keep subject line under 72 characters — no exceptions.

## Output

A ready-to-use commit message displayed to the user, formatted for direct paste into `git commit -m`.
