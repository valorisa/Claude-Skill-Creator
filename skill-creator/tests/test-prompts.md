# Test Prompts for Skill Creator

Use these to verify the skill works correctly. Open a new Claude Code session for each test.

## Test 1 — Vague request (menu should appear)

> "I want a skill that helps me write better commit messages"

Expected behavior:

- The skill presents the menu questions (purpose, target LLM, trigger style, etc.)
- After collecting answers, it generates a structured SKILL.md
- It validates the output before showing it
- It asks for feedback and iterates

## Test 2 — Precise request (menu should be skipped)

> "Create a skill called 'sql-explainer' that takes a SQL query and explains it step by step in plain French. Trigger: /sql-explain. Target: Claude Code. No tools needed. Output: text explanation displayed to user."

Expected behavior:

- The skill recognizes that all questions are answered
- It generates immediately without asking the menu
- It runs structural validation
- It shows the result and asks for feedback

## Test 3 — Improvement of existing skill

> "My existing skill /translate-doc is too slow because it processes the entire file even for small changes. Can you help me optimize it?"

Expected behavior:

- The skill reads the existing skill file
- It identifies the performance issue in the process steps
- It proposes targeted edits (not a full rewrite)
- It iterates based on user feedback

## Test 4 — Non-Claude target LLM

> "Create a skill for Cursor AI that auto-generates JSDoc comments for TypeScript functions. Trigger: auto-detect when user selects a function. Simple one-shot behavior."

Expected behavior:

- The skill adapts output format to Cursor conventions (.cursorrules or .cursor/rules/)
- It does not reference Claude-specific paths or features
- It produces a valid Cursor rule file

## Test 5 — Multi-tool complex skill

> "I need a skill that monitors a GitHub repo for new issues, classifies them by priority, and drafts a response. It needs web access and file system tools."

Expected behavior:

- The skill asks clarifying questions about the classification criteria
- It generates a multi-phase skill with explicit tool declarations
- It validates that tool usage is coherent with the process steps
- It warns about potential edge cases (rate limits, auth requirements)
