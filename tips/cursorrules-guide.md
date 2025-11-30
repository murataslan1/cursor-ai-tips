# .cursorrules Complete Guide

[← Back to Main](../README.md)

## What is .cursorrules?

A system prompt file that customizes AI behavior for your specific project. It's like giving the AI a "personality" and "rules" to follow.

---

## Basic Setup

Create `.cursorrules` in your project root:

```
You are an expert senior developer.
You write clean, maintainable code.
You follow the existing patterns in this codebase.
```

---

## Essential Rule Categories

### 1. Tech Stack Enforcement

Prevents the AI from hallucinating libraries you don't use:

```
Tech Stack:
- Framework: Next.js 14 with App Router
- Styling: Tailwind CSS only
- UI Components: shadcn/ui
- State: Zustand
- Forms: React Hook Form + Zod
- Database: Prisma with PostgreSQL

NEVER use:
- CSS modules
- styled-components
- Redux
- Formik
```

### 2. Architecture Rules

```
Architecture:
- Components in src/components must be presentational only
- Business logic goes in src/services
- API routes in src/app/api follow REST conventions
- Database queries only in src/lib/db
```

### 3. Code Style

```
Code Style:
- Use TypeScript strict mode
- Prefer const over let, never var
- Use arrow functions for components
- Destructure props
- Use early returns
- Maximum function length: 50 lines
```

### 4. The Anti-Lazy Rule (Critical!)

This is the **most important rule** from Reddit community:

```
IMPORTANT OUTPUT RULES:
- You MUST output the FULL content of files
- NEVER use placeholders like "// ... existing code"
- NEVER use "// ... rest of the code"
- NEVER truncate or summarize code
- If a file is too long, split into multiple responses
```

Without this rule, AI often outputs partial files that break your code when applied.

### 5. Testing Requirements

```
Testing:
- Every new function must have a unit test
- Tests go in __tests__ folder adjacent to source
- Use Vitest for unit tests
- Use Playwright for E2E tests
- Minimum 80% code coverage for new code
```

### 6. Documentation

```
Documentation:
- Add JSDoc comments to exported functions
- Update README.md when adding new features
- Keep CHANGELOG.md updated
```

---

## Advanced: .mdc Files (Project Rules)

New system with more granular control.

### Directory Structure

```
.cursor/
└── rules/
    ├── react-components.mdc
    ├── api-routes.mdc
    ├── database.mdc
    └── testing.mdc
```

### .mdc File Format

```yaml
---
description: "Rules for React Components"
globs: ["src/components/**/*.tsx", "src/app/**/*.tsx"]
alwaysApply: false
---

You are writing React components.

Rules:
1. Use functional components only
2. Props interface above component
3. Destructure props in function signature
4. Use Tailwind for styling
5. Keep components under 100 lines
```

### Glob Patterns

| Pattern | Matches |
|---------|---------|
| `*.ts` | All .ts files in root |
| `**/*.ts` | All .ts files anywhere |
| `src/**/*.tsx` | TSX files in src |
| `!**/*.test.ts` | Exclude test files |

---

## Complete Example: Next.js Project

```
# Project: E-commerce Platform

## Tech Stack
- Next.js 14 (App Router)
- TypeScript 5.3+
- Tailwind CSS
- Prisma + PostgreSQL
- NextAuth.js for auth
- Stripe for payments
- shadcn/ui components

## Architecture
src/
├── app/           # Next.js App Router pages
├── components/    # Reusable UI components (presentational)
├── lib/          # Utilities and configurations
├── services/     # Business logic
├── hooks/        # Custom React hooks
├── types/        # TypeScript type definitions
└── __tests__/    # Test files

## Code Standards
- Functional components with TypeScript
- Props interfaces defined above component
- Use "use client" only when necessary
- Server Components by default
- Validate all inputs with Zod

## Naming Conventions
- Components: PascalCase (UserCard.tsx)
- Hooks: camelCase with use prefix (useAuth.ts)
- Utils: camelCase (formatDate.ts)
- Types: PascalCase with T prefix (TUser)

## Error Handling
- Use try/catch in server actions
- Return typed errors: { error: string } | { data: T }
- Log errors with context

## Security
- Validate all user inputs
- Use parameterized queries (Prisma handles this)
- Sanitize HTML output
- Check authorization in every API route

## OUTPUT RULES (CRITICAL)
- Output FULL files, never use placeholders
- Never use "// ... existing code"
- Never truncate responses
- If file is large, ask before proceeding
```

---

## Language-Specific Rules

### TypeScript/React

```
TypeScript Rules:
- strict mode enabled
- No any types (use unknown if needed)
- Explicit return types on functions
- Use interface over type for objects
- Use enums sparingly, prefer union types

React Rules:
- Functional components only
- Custom hooks for reusable logic
- useMemo/useCallback only when needed
- Avoid prop drilling (use context or Zustand)
```

### Python

```
Python Rules:
- Python 3.11+ features allowed
- Type hints on all functions
- Use dataclasses or Pydantic models
- Black for formatting
- Pytest for testing
- No global variables
```

### Go

```
Go Rules:
- Go 1.21+ features allowed
- Handle all errors explicitly
- Use context for cancellation
- Prefer composition over inheritance
- Table-driven tests
```

---

## .cursorignore

Exclude files from Cursor's index:

```
# .cursorignore

# Dependencies
node_modules/
vendor/
.venv/

# Build outputs
dist/
build/
.next/

# Large files
*.csv
*.json  # if large data files
*.sql   # if database dumps

# Generated
package-lock.json
yarn.lock
pnpm-lock.yaml

# Secrets (shouldn't be in repo anyway)
.env*
```

---

## Tips from Reddit

### Dealing with Lazy AI

If AI keeps using placeholders despite rules:

1. Add anti-lazy rule (see above)
2. Start fresh chat
3. Be explicit: "Output the COMPLETE file"
4. Use smaller files (under 200 lines)

### Rules Not Working?

1. Check file is named exactly `.cursorrules`
2. Check it's in project root
3. Restart Cursor
4. Try explicit `@.cursorrules` in chat

### Too Many Rules?

If AI seems confused:

1. Keep rules concise
2. Prioritize critical rules at top
3. Use .mdc files for specific file types
4. Remove contradictory rules

---

## Community Resources

- [awesome-cursorrules](https://github.com/PatrickJS/awesome-cursorrules) - Community rules collection
- [cursor.directory](https://cursor.directory) - Shareable rules
- [r/cursor](https://reddit.com/r/cursor) - Community discussions

---

[← Back to Main](../README.md)
