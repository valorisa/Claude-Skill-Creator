# Security Policy

## Supported versions

| Version | Supported |
| ------- | --------- |
| latest  | Yes       |

## Reporting a vulnerability

This project is a collection of markdown files with no executable code, runtime, or dependencies. The attack surface is minimal.

However, if you discover a security concern (e.g., a generated skill template that could lead to unintended file system access or command execution), please report it responsibly:

1. Do NOT open a public issue
2. Email the maintainer or use GitHub's private vulnerability reporting feature
3. Include a description of the concern and steps to reproduce

We will acknowledge receipt within 48 hours and provide a fix or response within 7 days.

## Scope

Security concerns relevant to this project include:

- Skill templates that instruct LLMs to perform destructive actions without confirmation
- Generated skills that could expose sensitive data (credentials, tokens, private keys)
- Instructions that bypass LLM safety constraints

Out of scope:

- Vulnerabilities in Claude Code itself (report to Anthropic)
- Vulnerabilities in other LLMs (report to their respective vendors)
