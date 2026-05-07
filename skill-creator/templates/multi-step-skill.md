# API Scaffolder

Generate a complete REST API endpoint from a one-line description.

## Trigger

Use when the user says "create endpoint for X", "new API route", `/scaffold-api`, or describes a CRUD operation to implement.

## Process

1. Ask: "What resource? What operations (GET/POST/PUT/DELETE)? What framework?"
2. If the user already specified everything, skip questions
3. Read existing route files in the project to detect patterns and conventions
4. Generate the route handler with:
   - Input validation
   - Error handling with the project's existing error format
   - Response structure
   - TypeScript types if the project uses TypeScript
5. Show the generated code and ask: "Want me to add tests or adjust anything?"
6. If yes, generate a test file alongside using the project's test framework

## Constraints

- Match the project's existing patterns (check existing routes first)
- Do not add authentication middleware unless asked
- Do not create database migrations — only the route layer
- Use the project's existing error format and response structure
- If no existing patterns are found, ask the user which conventions to follow

## Output

One file per endpoint in the project's routes directory, following existing conventions. Optionally a test file if requested.
