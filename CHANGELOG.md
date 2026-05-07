# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

## [2.0.0] - 2026-05-07

### Added

- Structured 7-question menu interview with predefined options
- Multi-LLM support (Claude Code, Cursor, Windsurf, ChatGPT, generic)
- Structural validation phase (7 automated checks before showing output)
- "Try it in a new session" as the real test step
- "What is a skill?" preamble for newcomers
- Windows-specific installation instructions (PowerShell)
- 5 template examples covering different skill categories
- 5 test scenarios covering different use cases
- CONTRIBUTING.md, CODE_OF_CONDUCT.md, SECURITY.md
- GitHub issue templates and PR template

### Changed

- Replaced fake smoke test with honest structural validation
- Replaced arbitrary "2 screens" limit with measurable 150-line limit
- Installation now recommends copy over symlink on Windows
- SKILL.md rewritten with menu-driven flow

### Removed

- Simulated smoke test phase (replaced by structural validation)

## [1.0.0] - 2026-05-07

### Added

- Initial release
- SKILL.md with 5-phase interview process
- 2 template examples (code reviewer, API scaffolder)
- 3 test scenarios
- README with installation and usage documentation
- MIT License
