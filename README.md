# Claude Skill Creator

A meta-skill for Claude Code that helps you create, test, iterate, and improve custom skills through a structured conversational workflow.

## What is this

Claude Skill Creator is a skill for Claude Code (the CLI tool by Anthropic) that guides you through the process of building new skills from scratch. It follows a proven iterative loop: interview your intent, generate a first draft, smoke-test it immediately, and refine until you are satisfied.

A skill is just a markdown file. Nothing more. This tool embraces that simplicity.

## Why this exists

Creating a good skill requires a specific discipline: clearly defining triggers, expected behavior, constraints, and output format. Without structure, you end up with vague prompts that produce inconsistent results. This skill provides that structure as a guided conversation rather than a complex framework or infrastructure.

The design philosophy comes from a deliberation process (LLM Council) that converged on one principle: the conversation is the product, not the folder structure. Every decision in this tool optimizes for getting you from vague intent to working skill in the shortest possible feedback loop.

## How it works

The Skill Creator operates in five phases.

### Phase 1 - Interview

Claude asks you targeted questions to extract your intent:

- What should the skill do (one sentence)
- When should it trigger (slash command, auto-detect phrases, or both)
- What does good output look like (concrete example)
- What should it NOT do (boundaries and failure modes)
- Does it need specific tools (file access, bash, web, agents)

If you are vague, Claude proposes a concrete interpretation and asks for confirmation.

### Phase 2 - Generate first draft

Based on your answers, Claude writes a complete SKILL.md with proper structure: trigger definition, numbered process steps, constraints, and output format. The file lands directly in your skills directory.

### Phase 3 - Smoke test

Claude immediately proposes 2-3 realistic test prompts, simulates how the new skill would respond, and shows you the result. You evaluate whether the output matches your expectations.

### Phase 4 - Iterate

Based on your feedback, Claude edits the skill and re-runs the test. This loop repeats until you say it is done. No artificial stopping point.

### Phase 5 - Finalize

Once satisfied, Claude confirms the skill location, suggests edge cases to watch for, and reminds you that you can return to improve it anytime.

## Installation

### Option 1 - Copy to your skills directory

Clone this repository and copy the skill-creator folder to your Claude Code skills directory:

```bash
git clone https://github.com/valorisa/claude-skill-creator.git
cp -r claude-skill-creator/skill-creator ~/.claude/skills/
```

### Option 2 - Symlink for easy updates

```bash
git clone https://github.com/valorisa/claude-skill-creator.git
ln -s "$(pwd)/claude-skill-creator/skill-creator" ~/.claude/skills/skill-creator
```

### Verify installation

Start a new Claude Code session and type `/skill-creator`. Claude should respond by asking what skill you want to create.

## Usage

### Create a new skill from scratch

```text
> /skill-creator
> "I want a skill that translates error messages into plain French explanations"
```

Claude will interview you, generate the skill, test it, and iterate with you.

### Create a skill with a precise specification

```text
> Create a skill called 'sql-explainer' that takes a SQL query and explains it
  step by step in plain French. Trigger: /sql-explain. No tools needed.
```

When your request is specific enough, Claude skips most questions and generates directly.

### Improve an existing skill

```text
> My existing skill /review is too verbose. Help me tighten it.
```

Claude reads the existing skill, identifies issues, and iterates with you.

## Project structure

```text
skill-creator/
  SKILL.md              The skill definition
  templates/
    simple-skill.md     Example: single-purpose skill
    multi-step-skill.md Example: multi-phase skill
  tests/
    test-prompts.md     Validation scenarios
```

### About the templates

The templates folder contains two reference skills of different complexity levels. They serve as pattern references for Claude when generating your new skill, not as rigid scaffolding to fill in. Claude adapts the structure to match what your skill actually needs.

### About the tests

The tests folder contains three scenarios covering different use cases: vague requests, precise specifications, and improvement of existing skills. Use them to verify the skill creator behaves correctly after any modifications you make.

## Design principles

These principles guided every decision in this tool:

1. **A skill is just a markdown file.** No build steps, no dependencies, no runtime. If you need more than a markdown file, you are building something else.

2. **The conversation is the product.** The value lives in the interactive loop, not in folder structure or infrastructure. The interview extracts intent. The test validates it. The iteration refines it.

3. **The human is the evaluator.** No automated scoring, no rubrics, no eval frameworks. You look at the output and decide if it is good. That is the fastest and most reliable evaluation system at individual scale.

4. **Ship then improve.** A working skill with rough edges beats a perfect design document. Generate, test, iterate. Do not plan in the abstract.

5. **No premature infrastructure.** No registries, no pattern libraries, no composition engines. Add complexity only when you have five working skills that demand it.

## Requirements

- Claude Code CLI (any recent version)
- A Claude model with tool access (Opus, Sonnet, or Haiku)
- Works on any platform (Windows, macOS, Linux)
- No external dependencies

## Contributing

Contributions are welcome. The bar for inclusion is simple: does this change make it faster to go from intent to working skill? If yes, open a pull request. If you are adding infrastructure, explain why the current simplicity is insufficient for your use case.

Areas where contributions would be particularly valuable:

- Additional template examples covering different skill categories
- Translations of the SKILL.md for non-English users
- Bug reports from edge cases encountered during real usage

## License

MIT License. See the LICENSE file for details.

## Credits

- Design methodology informed by an LLM Council deliberation (5 independent advisors with peer review, adapted from Andrej Karpathy's LLM Council concept)
- Built for use with Claude Code by Anthropic
