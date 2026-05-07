# Skill Creator

Create new skills for Claude Code or any other LLM. A skill is just a markdown file — nothing more.

## What is a skill

A skill is a markdown file placed in a known directory (e.g., `~/.claude/skills/` for Claude Code) that instructs an LLM how to behave when a specific trigger is detected. The LLM reads the file and follows the instructions. No runtime, no compilation, no API — just structured text that shapes behavior.

Different LLMs load skills from different locations:

- Claude Code: `~/.claude/skills/[skill-name]/SKILL.md`
- Cursor: `.cursorrules` or `.cursor/rules/`
- Windsurf: `.windsurfrules`
- Generic: any markdown file referenced in a system prompt

This tool is designed and tested for Claude Code. Other LLMs may interpret the generated markdown, but cross-platform compatibility is not guaranteed.

## Trigger

Use when the user says "create a skill", "new skill", "skill for X", "make me a skill", `/skill-creator`, or asks to build, edit, or improve a custom command or behavior.

## Process

### Phase 1 — Menu interview (extract intent)

Present this menu to the user and collect answers. Use the AskUserQuestion tool to present choices when available, or ask conversationally if the tool is not available.

#### Question 1 — Purpose

> What type of skill do you want to create?

- Code generation (generate boilerplate, scaffolding, templates)
- Code analysis (review, audit, explain, document)
- Workflow automation (multi-step process, pipeline)
- Content creation (writing, translation, formatting)
- Data processing (parse, transform, extract, summarize)
- Custom (describe freely)

#### Question 2 — Trigger style

> How should this skill activate?

- Slash command only (e.g., /my-skill)
- Auto-detect from natural language (trigger phrases)
- Both (slash command + auto-detect)

#### Question 3 — Tools needed

> Does this skill need to use external tools?

- None (pure text reasoning)
- File system (read/write files)
- Shell / terminal commands
- Web access (fetch URLs, search)
- Multiple tools (specify which)

#### Question 4 — Output format

> What should the skill produce?

- Code (files written to disk)
- Text response (displayed to user)
- Structured data (JSON, YAML, table)
- Mixed (code + explanation)
- Files + summary report

#### Question 5 — Boundaries

> What should this skill explicitly REFUSE to do?

(Examples: never delete files without confirmation, never modify code outside the target directory, never auto-commit, never send data externally)

This is critical — boundaries prevent the skill from doing harm or overstepping scope.

#### Question 6 — Autonomy level

> Should this skill ask before acting, or act then confirm?

- Ask first (always confirm before making changes)
- Act then confirm (do the work, then show the result for approval)
- Full autonomy (execute without asking — only for safe, read-only operations)

#### Question 7 — Description and example

> Describe in one or two sentences what the skill should do. Then give one concrete example: what input would you provide, and what output do you expect?

(Free text — the description gives intent, the example anchors the generation)

If the user provides a precise specification upfront that answers most of these questions, skip the menu and move directly to Phase 2. Adapt — do not rigidly follow the menu when the intent is already clear.

If the user mentions a non-Claude target LLM (Cursor, Windsurf, etc.), adapt the output format to that platform's conventions. Otherwise, default to Claude Code.

### Phase 2 — Generate first draft

Based on the interview answers, write a complete skill file. Structure it as:

```markdown
# [Skill Name]

[One-line description of what this skill does]

> **Safety:** This skill runs with your full user permissions.
> Review all instructions before first use. Never perform
> destructive operations (file deletion, force push, reset)
> without explicit user confirmation.

## Trigger

[When this skill activates — command name and/or trigger phrases]
[Adapt to target LLM conventions]

## Process

[Numbered steps — what the LLM does when this skill runs]
[Be specific and imperative: "Read the file", "Ask the user", "Generate"]

## Constraints

- Never perform destructive operations without explicit user confirmation
- [What NOT to do — boundaries, failure modes, edge cases]
- [Include at least 2-3 additional constraints]

## Output

[What the user gets at the end — format, location, style]
[Be explicit about deliverable format]
```

Place it in the appropriate location for the target LLM. For Claude Code: `~/.claude/skills/[skill-name]/SKILL.md`. Create the directory if it does not exist.

### Phase 3 — Council validation

After generating, submit the draft skill to the LLM Council for peer review. Invoke the `llm-council` skill (or apply the council methodology manually with 5 sub-agents if the skill is available) with this framing:

> "Review this generated skill for quality, safety, and effectiveness. The skill is: [paste the full generated SKILL.md]. Evaluate: (1) Will the trigger fire correctly and not conflict with existing commands? (2) Are the process steps specific enough to produce consistent results? (3) Are the constraints sufficient to prevent harmful or unexpected behavior? (4) Is the output format clear and deliverable? (5) Would a user be confused by any instruction? (6) Are there edge cases not covered?"

Present the Council verdict to the user with:

- What the advisors agreed works well
- What they flagged as problems
- Specific fixes recommended

Apply the recommended fixes before moving to Phase 4. If the Council finds no issues, proceed directly.

This is real validation — 5 independent perspectives evaluating the skill, not self-grading. If the `llm-council` skill is not available, fall back to a structural checklist:

1. Has a clear, unambiguous trigger description?
2. Process steps are numbered and imperative (not vague)?
3. At least 2 constraints defined (including no destructive ops without confirmation)?
4. Output format is explicit?
5. Total length under 150 lines or 5000 characters?
6. No instructions that contradict known LLM behaviors?
7. Trigger phrases do not conflict with existing skills?

If any check fails, fix it before showing to the user.

### Phase 4 — Iterate with user

Show the generated skill to the user and ask:

> "Here is the first draft. What would you change?"

Based on feedback:

1. Identify what to change
2. Edit the skill file directly
3. Show the updated version
4. Ask: "Better? What is still off?"

Repeat until the user is satisfied. Do not stop after one round.

### Phase 5 — Finalize and try it

Once the user approves:

1. Confirm the skill location and trigger command
2. Suggest 2-3 edge cases to watch for
3. Instruct the user: "Open a new session and test it with a real task. For example, try: `[concrete example prompt that would trigger the skill]`"

This is the real test. A new session ensures the skill is loaded fresh and behaves as intended.

## Constraints

- Never over-engineer. A skill is a markdown file, not a codebase.
- Keep generated skills under 150 lines. If longer, split into multiple skills.
- Do not add folders, scripts, or infrastructure unless the user explicitly asks.
- Do not generate evaluation frameworks or rubrics. The user is the evaluator.
- Respect the user's existing skill naming conventions if they have any.
- Always create the target directory before writing the file.
- On Windows, use copy operations — never suggest symlinks without warning about admin privileges.
- Generate skills that are LLM-agnostic in their instructions unless the user specifies a target.

## Optional extensions

### LLM Council (recommended for full validation)

Phase 3 uses the `llm-council` skill for multi-perspective peer review. Without it, a structural checklist is used as fallback.

To install the Council skill:

```bash
git clone https://github.com/tenfoldmarc/llm-council-skill.git
mkdir -p ~/.claude/skills/llm-council
cp llm-council-skill/SKILL.md ~/.claude/skills/llm-council/SKILL.md
```

Source: [tenfoldmarc/llm-council-skill](https://github.com/tenfoldmarc/llm-council-skill) — MIT License, adapted from Andrej Karpathy's LLM Council methodology.

The Council skill is not bundled in this repo to avoid version drift. It evolves independently and should be updated from its upstream source.

## Output

A working skill file, validated and iterated to the user's satisfaction, placed in the correct location for their target LLM.
