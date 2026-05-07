# Claude Skill Creator

A meta-skill for Claude Code that helps you create, test, iterate, and improve custom skills through a structured menu-driven conversational workflow. Skills are plain markdown — other LLMs can interpret them, but only Claude Code has been tested.

## What is a skill

A skill is a markdown file placed in a known directory that instructs an LLM how to behave when a specific trigger is detected. The LLM reads the file and follows the instructions. No runtime, no compilation, no API — just structured text that shapes behavior.

Different LLMs load skills from different locations:

| LLM | Skill location |
| --- | -------------- |
| Claude Code | `~/.claude/skills/[skill-name]/SKILL.md` |
| Cursor AI | `.cursorrules` or `.cursor/rules/` |
| Windsurf | `.windsurfrules` |
| ChatGPT | Custom instructions or system prompt |
| Generic | Any markdown file referenced in context |

The format this tool generates is designed for Claude Code. Other LLMs may interpret the same markdown instructions, but cross-platform compatibility has not been formally tested. Contributions welcome for verified adapters.

## Why this exists

Creating a good skill requires a specific discipline: clearly defining triggers, expected behavior, constraints, and output format. Without structure, you end up with vague prompts that produce inconsistent results.

The difference between asking Claude "help me write a skill" bare and using this tool is the structured interview. The menu forces you to think through purpose, triggers, complexity, tools, and output format before any generation happens. That specificity is what produces skills that work reliably instead of skills that work sometimes.

The design philosophy comes from a deliberation process (LLM Council with 5 independent advisors and peer review) that converged on one principle: the conversation is the product, not the folder structure.

## How it works

The Skill Creator operates in five phases.

### Phase 1 - Menu interview

The skill presents a structured menu with 7 questions:

1. **Purpose** — What type of skill? (code generation, analysis, workflow, content, data processing, custom)
2. **Target LLM** — Which LLM will use it? (Claude Code, Cursor, Windsurf, ChatGPT, generic)
3. **Trigger style** — Slash command, auto-detect, or both?
4. **Complexity** — Simple one-shot, interactive, or multi-phase?
5. **Tools needed** — None, file system, shell, web, or multiple?
6. **Output format** — Code, text, structured data, mixed, or files + report?
7. **Description** — What should the skill do? (free text)

If you provide a precise specification that answers these questions upfront, the menu is skipped and generation begins immediately.

### Phase 2 - Generate first draft

Based on your answers, the skill generates a complete structured markdown file with: trigger definition, numbered process steps, constraints, and output format. The file is placed in the correct location for your target LLM. The directory is created automatically if it does not exist.

### Phase 3 - Structural validation

Before showing you the result, the skill silently validates:

- Clear trigger description present?
- Process steps are numbered and imperative?
- At least 2 constraints defined?
- Output format is explicit?
- Total length under 150 lines?
- No contradictions with known LLM behaviors?
- No conflicts with existing skill triggers?

Failures are auto-corrected before you see the output.

### Phase 4 - Iterate with user

The generated skill is shown to you with the question: "What would you change?" Based on your feedback, the skill is edited directly and shown again. This loop repeats until you are satisfied.

### Phase 5 - Finalize and try it

Once approved, the skill confirms:

- The file location and trigger command
- 2-3 edge cases to watch for
- A concrete test instruction: "Open a new session and try: `[example prompt]`"

The real test happens in a fresh session where the skill is loaded from disk. This is intentional — it is the only honest way to validate that the skill works as intended.

## Installation

### Option 1 - Copy to your skills directory (recommended)

```bash
git clone https://github.com/valorisa/Claude-Skill-Creator.git
mkdir -p ~/.claude/skills
cp -r Claude-Skill-Creator/skill-creator ~/.claude/skills/skill-creator
```

Verify it worked:

```bash
ls ~/.claude/skills/skill-creator/SKILL.md
```

On Windows (PowerShell):

```powershell
git clone https://github.com/valorisa/Claude-Skill-Creator.git
New-Item -ItemType Directory -Force -Path $env:USERPROFILE\.claude\skills
Copy-Item -Recurse Claude-Skill-Creator\skill-creator $env:USERPROFILE\.claude\skills\skill-creator
```

Verify it worked:

```powershell
Test-Path $env:USERPROFILE\.claude\skills\skill-creator\SKILL.md
```

### Option 2 - Symlink for easy updates

On macOS/Linux:

```bash
git clone https://github.com/valorisa/Claude-Skill-Creator.git ~/projects/Claude-Skill-Creator
ln -s ~/projects/Claude-Skill-Creator/skill-creator ~/.claude/skills/skill-creator
```

On Windows (requires Administrator or Developer Mode):

```powershell
git clone https://github.com/valorisa/Claude-Skill-Creator.git $env:USERPROFILE\projects\Claude-Skill-Creator
New-Item -ItemType SymbolicLink -Path $env:USERPROFILE\.claude\skills\skill-creator -Target $env:USERPROFILE\projects\Claude-Skill-Creator\skill-creator
```

### Verify installation

Start a new Claude Code session and type `/skill-creator`. Claude should respond by presenting the interview menu or asking what skill you want to create.

## Usage examples

### Example 1 — Create a skill from the menu

```text
> /skill-creator

[Claude presents the 7-question menu]

> Purpose: Code analysis
> Target: Claude Code
> Trigger: /explain-error
> Complexity: Simple
> Tools: None
> Output: Text response
> Description: Takes an error message and explains what it means,
  why it happened, and how to fix it in plain language.

[Claude generates, validates, and shows the skill]
[User iterates until satisfied]
[Claude says: "Open a new session and try: paste an error message
 and say /explain-error"]
```

### Example 2 — Skip the menu with a precise spec

```text
> Create a skill called 'changelog-gen' for Claude Code that reads
  git log since last tag, groups commits by type (feat/fix/chore),
  and generates a CHANGELOG.md entry. Trigger: /changelog.
  Needs shell access for git commands. Output: markdown file.

[Claude recognizes all parameters are provided]
[Generates immediately without menu]
[Validates structure, shows result, iterates]
```

### Example 3 — Create a skill for a different LLM

```text
> /skill-creator

> Purpose: Content creation
> Target: Cursor AI
> Trigger: Auto-detect when user writes "generate docs"
> Complexity: Interactive
> Tools: File system
> Output: Code (markdown files)
> Description: Generate API documentation from source code comments
  and function signatures.

[Claude adapts output to Cursor conventions]
[Generates a .cursor/rules/ compatible file]
```

### Example 4 — Improve an existing skill

```text
> My skill /pr-review takes too long and gives too many nitpick
  suggestions. Help me tighten it to focus only on bugs and
  security issues.

[Claude reads the existing skill]
[Identifies verbose sections and low-priority checks]
[Proposes targeted edits]
[Iterates until user approves]
```

## Project structure

```text
skill-creator/
  SKILL.md                        The skill itself
  templates/
    simple-skill.md               Example: commit message writer
    multi-step-skill.md           Example: API scaffolder
    interactive-skill.md          Example: doc translator
    workflow-skill.md             Example: PR reviewer
    data-processing-skill.md     Example: log analyzer
  tests/
    test-prompts.md              5 validation scenarios
```

### About the templates

The templates folder contains five reference skills covering different categories:

| Template | Type | Complexity | Tools |
| -------- | ---- | ---------- | ----- |
| simple-skill.md | Code generation | Simple | Shell (git) |
| multi-step-skill.md | Code generation | Multi-phase | File system |
| interactive-skill.md | Content creation | Interactive | File system |
| workflow-skill.md | Code analysis | Multi-phase | Shell (gh) |
| data-processing-skill.md | Data processing | Interactive | File system |

They serve as pattern references for the LLM when generating your new skill. The LLM adapts structure and depth to match what your skill actually needs.

### About the tests

The tests folder contains five scenarios covering:

1. Vague request (menu should appear and guide the user)
2. Precise specification (menu should be skipped)
3. Improvement of existing skill (read, analyze, propose edits)
4. Non-Claude target LLM (adapt to different conventions)
5. Complex multi-tool skill (handle dependencies and edge cases)

Use them to verify the skill creator behaves correctly after modifications.

## Design principles

1. **A skill is just a markdown file.** No build steps, no dependencies, no runtime. If you need more than a markdown file, you are building something else.

2. **The conversation is the product.** The value lives in the structured interview that forces specificity. The menu extracts intent. The validation catches errors. The iteration refines.

3. **The human is the evaluator.** No automated scoring or rubrics. You test in a fresh session with a real task. That is the fastest and most reliable validation.

4. **Ship then improve.** A working skill with rough edges beats a perfect design document. Generate, validate, iterate, test in a new session. Do not plan in the abstract.

5. **No premature infrastructure.** No registries, no pattern libraries, no composition engines. Add complexity only when you have five working skills that demand it.

6. **Honest about scope.** This tool is designed and tested for Claude Code. Generated skills are plain markdown that other LLMs may interpret, but no cross-platform guarantee is made without explicit testing.

## Requirements

- Claude Code CLI (any recent version)
- Works on any platform (Windows, macOS, Linux)
- No external dependencies
- No admin privileges required (copy installation)

## Uninstall

Remove the skill directory:

```bash
rm -rf ~/.claude/skills/skill-creator
```

On Windows (PowerShell):

```powershell
Remove-Item -Recurse -Force $env:USERPROFILE\.claude\skills\skill-creator
```

This leaves no other traces. No config files, no registry entries, no global state.

## Contributing

Contributions are welcome. The bar for inclusion is simple: does this change make it faster to go from intent to working skill? If yes, open a pull request.

Areas where contributions would be particularly valuable:

- Additional template examples covering new skill categories
- Translations of the SKILL.md for non-English users
- Adaptations for additional LLM platforms
- Bug reports from edge cases encountered during real usage

## License

MIT License. See the LICENSE file for details.

## Credits

- Design methodology informed by two rounds of LLM Council deliberation (5 independent advisors with peer review, adapted from Andrej Karpathy's LLM Council concept)
- Built for use with Claude Code by Anthropic and compatible with other LLMs
