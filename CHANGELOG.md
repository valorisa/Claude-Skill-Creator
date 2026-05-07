# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

## [2.0.0] - 2026-05-07

### Added

- Structured 7-question menu interview with predefined options
- Multi-LLM output format support (Claude Code tested, others experimental)
- Structural validation phase (7 automated checks before showing output)
- "Try it in a new session" as the honest test step
- "What is a skill?" preamble for newcomers
- Safety notice in generated skill template (permission warning, no destructive ops without confirmation)
- Windows-specific installation instructions with mkdir and verification
- 5 template examples covering different skill categories
- 5 test scenarios covering different use cases
- Full GitHub standardization (CI, templates, security policy, code of conduct)

### Design decisions

- Replaced simulated smoke test with structural validation (LLM cannot invoke itself)
- Replaced arbitrary "2 screens" limit with measurable 150-line limit
- Installation recommends copy over symlink (no admin required)
- Multi-LLM claims explicitly marked as untested for non-Claude platforms
