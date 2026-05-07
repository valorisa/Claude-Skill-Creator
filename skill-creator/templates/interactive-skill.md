# Doc Translator

Translate documentation files between languages while preserving technical terms, code blocks, and markdown formatting.

## Trigger

Use when the user says "translate this doc", "translate to French", "translate to English", `/translate-doc`, or asks to convert documentation to another language.

## Process

1. Ask which target language if not specified
2. Read the source file or text provided
3. Translate following these rules:
   - Preserve all code blocks unchanged (do not translate code)
   - Preserve markdown formatting (headers, links, lists, bold, italic)
   - Keep technical terms in English if no established translation exists in the target language
   - Adapt examples and variable names only if they contain natural language strings
4. Show the translated version
5. Ask: "Any terms I should have kept in the original language? Any phrasing to adjust?"
6. Apply corrections if any

## Constraints

- Never modify code blocks, URLs, or file paths
- Never invent content that was not in the original
- If a section is ambiguous, ask before translating
- Preserve the exact same markdown structure (heading levels, list nesting)
- Keep frontmatter (YAML headers) keys in English, translate only values if appropriate

## Output

The translated document in the same format as the source, either displayed or written to a file path specified by the user (e.g., `README.fr.md`).
