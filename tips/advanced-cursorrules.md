# Advanced .cursorrules Patterns

As users grapple with model inconsistency, sophisticated `.cursorrules` files have emerged to constrain AI behavior. These patterns come from production teams dealing with real-world AI quirks.

---

## The "Shout" Protocol

Force the AI to loudly announce when it modifies existing code.

```
If existing code is altered in any file during implementation,
the developer must warn by shouting with large letters:

❗️SHOUT WITH LARGE LETTERS❗️
"WARNING: Modified existing function calculateTotal() in utils.ts"

This ensures no silent refactoring of working code.
```

### Why This Works

The AI has a tendency to silently "improve" code that doesn't need improvement. The Shout Protocol:
- Makes changes impossible to miss
- Forces the AI to acknowledge modifications
- Gives you a clear audit trail

---

## "Dumb" Component Enforcement

Force the AI to recognize component responsibility boundaries.

```
Presentation components must include the word "Dumb" in their filename:
- UserProfileCardDumb.vue
- ProductListDumb.tsx
- NavigationDumb.svelte

"Dumb" components:
- Receive data ONLY through props
- Emit events for user actions
- NEVER contain business logic
- NEVER make API calls
- NEVER access global state
```

### Why This Works

The naming convention creates a **cognitive barrier** that prevents the AI from injecting complex logic where it doesn't belong.

---

## Architectural Segregation

Prevent the AI's bias toward brevity from creating maintenance nightmares.

```
NEVER handle different request methods for an API endpoint in the same file.

CORRECT:
  /api/users/get.ts    → GET handler
  /api/users/post.ts   → POST handler
  /api/users/put.ts    → PUT handler
  /api/users/delete.ts → DELETE handler

INCORRECT:
  /api/users/index.ts  → All methods in one file
```

### Why This Works

LLMs naturally gravitate toward putting everything in one file (it's "cleaner"). This rule enforces a strict file-per-method structure for maintainability.

---

## Anti-Flake Testing Rules (Playwright)

Stop the AI from writing brittle tests.

```
For Playwright tests:

1. NEVER use manual delays:
   ❌ await page.waitForTimeout(5000)
   ✅ await page.waitForSelector('[data-testid="submit"]')

2. ALWAYS use built-in auto-wait mechanisms:
   ❌ await sleep(1000); await page.click('.btn')
   ✅ await page.click('.btn')  // Auto-waits by default

3. Target elements using stable selectors:
   ❌ div > span:nth-child(2)
   ❌ .css-1a2b3c
   ✅ [data-testid="user-name"]
   ✅ [aria-label="Submit form"]

4. If data-testid is missing, ADD IT to source code first:
   "Before writing the test, add data-testid='submit-button' 
    to the Submit button in LoginForm.tsx"
```

### Why This Works

- `sleep(5000)` is amateur and creates flaky tests
- CSS selectors break when designers change styles
- `data-testid` attributes are stable and semantic

---

## CodeRabbit Integration

Use a second AI to review the first AI's code.

```
Before finalizing any commit, run external review:

1. Stage changes: git add -A
2. Run CodeRabbit: coderabbit --prompt-only > review.md
3. Address any HIGH severity issues
4. Only then proceed with commit

This "Adversarial AI" approach catches errors one AI misses.
```

### Integration Command

```bash
# Add to your workflow
alias commit-safe='git add -A && coderabbit --prompt-only && git commit'
```

---

## Full Stack Example (.cursorrules for Nuxt 3)

```
# Nuxt 3 Full-Stack Constitution

You are an expert Nuxt 3 engineer.

## Architecture Rules

1. Pages go in /pages, components in /components, composables in /composables
2. Server routes go in /server/api with one file per HTTP method
3. Use Zod for ALL input validation on server routes
4. Use useFetch() for data fetching, NEVER raw fetch()

## Component Rules

1. Presentation components must have "Dumb" suffix
2. Container components handle data fetching and state
3. NEVER put business logic in "Dumb" components

## Modification Warning

❗️SHOUT WITH LARGE LETTERS❗️ if you modify any existing code.
Format: "WARNING: Modified [function] in [file]"

## Code Style

1. Use TypeScript strict mode
2. Prefer composition API over options API
3. Use <script setup> syntax
4. Props must be typed with defineProps<{}>()

## Testing

1. Every composable needs a test in /tests/composables/
2. Use Vitest for unit tests
3. Use Playwright for E2E with data-testid selectors
4. NEVER use waitForTimeout() - use proper waits
```

---

## Playwright Automation Rules (.cursorrules)

```
# Playwright Test Automation Standards

## Selector Priority (highest to lowest)
1. data-testid attributes
2. aria-label attributes  
3. role selectors
4. text content (last resort)

## Waiting Strategy
- Use Playwright's built-in auto-wait
- NEVER use page.waitForTimeout()
- Use waitForSelector() only when auto-wait insufficient
- Use waitForLoadState('networkidle') for SPA navigation

## Test Structure
- One test file per feature
- Use test.describe() for grouping
- Use beforeEach for common setup
- Clean up test data in afterEach

## Assertions
- Use expect() with proper matchers
- Prefer toBeVisible() over toHaveCount(1)
- Use soft assertions for non-critical checks

## If selector doesn't exist
1. First, add data-testid to source component
2. Then write the test
3. NEVER use fragile CSS selectors
```

---

## Anti-Lazy Full Output Rule

The nuclear option for when AI gets lazy.

```
You are an expert engineer.
You DO NOT use placeholders.
You DO NOT use "// ... existing code" or similar.
You output the FULL content of every file, every time.
You do not be lazy.

If a file is too long, say so and ask to proceed.
NEVER truncate or abbreviate code.
```

---

## .mdc File Pattern (New System)

Place in `.cursor/rules/` with glob patterns:

```yaml
# .cursor/rules/react-components.mdc
---
description: "React Component Rules"
globs: ["src/**/*.tsx", "src/**/*.jsx"]
alwaysApply: false
---

Use shadcn/ui for primitive components.
Components in src/ui must be presentational only.
Business logic goes in src/services.
All props must be typed with TypeScript interfaces.
```

```yaml
# .cursor/rules/api-routes.mdc
---
description: "API Route Rules"
globs: ["src/api/**/*.ts", "pages/api/**/*.ts"]
alwaysApply: false
---

Every route must validate input with Zod.
Return proper HTTP status codes.
Log errors to structured logging service.
NEVER expose internal error details to clients.
```

---

## References

- [danielkellyio's Nuxt cursorrules](https://gist.github.com/danielkellyio/6dec151d3b6a8cd21270f96ae893b480)
- [Playwright cursorrules](https://gist.github.com/imshaiknasir/e72181d1ba5a97e55ecbd7b1ad741d50)
- [Reddit: 6 AI tools to 2](https://www.reddit.com/r/cursor/comments/1pbdttp/went_from_6_ai_coding_tools_150mo_to_2_44mo_heres/)
