# Contributing to Claude Skill Creator

Thank you for your interest in contributing. This guide explains how to participate effectively.

## The bar for inclusion

Every contribution must answer one question: **does this make it faster to go from intent to working skill?** If the answer is unclear, the contribution likely does not belong here.

## What we welcome

- New template examples covering skill categories not yet represented
- Translations of SKILL.md into other languages
- Adaptations for LLM platforms not yet documented
- Bug reports from real usage edge cases
- Improvements to the interview menu (better questions, better options)
- Documentation clarifications for newcomers

## What we do not welcome

- Evaluation frameworks, scoring rubrics, or automated testing infrastructure
- Registries, databases, or external service integrations
- Build systems, package managers, or runtime dependencies
- Features that add complexity without clear value

## How to contribute

1. Fork the repository
2. Create a branch with a descriptive name (e.g., `add-template-git-workflow`)
3. Make your changes
4. Run markdownlint to verify no violations: `npx markdownlint-cli "**/*.md"`
5. Test the skill creator with your changes (see `skill-creator/tests/test-prompts.md`)
6. Open a pull request with a clear description

## Style guidelines

- Skills are markdown files. Keep them that way.
- Use imperative mood in process steps ("Read the file", not "The file is read")
- Keep generated skills under 150 lines
- Use ATX-style headings (# not underlines)
- No emoji unless the user explicitly requests them

## Code of conduct

Be respectful, constructive, and focused on the work. We follow the [Contributor Covenant](https://www.contributor-covenant.org/version/2/1/code_of_conduct/).
