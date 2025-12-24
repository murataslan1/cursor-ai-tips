# 🎭 Playwright MCP Setup - Self-Healing Tests

> **Game Changer**: Playwright MCP integration enables tests that automatically fix themselves when selectors change.

## What Is Self-Healing Testing?

Traditional E2E tests break when UI changes. With Playwright MCP, Cursor agents can:

1. Run tests automatically
2. Detect selector failures
3. Analyze the DOM
4. Update selectors without human intervention
5. Verify the fix works

---

## Quick Setup (Copy-Paste Ready)

### Step 1: Create `.cursor/mcp.json`

```json
{
  "mcpServers": {
    "playwright": {
      "command": "npx",
      "args": ["-y", "@playwright/mcp@latest"],
      "env": {
        "PLAYWRIGHT_BROWSERS_PATH": "0"
      }
    }
  }
}
```

### Step 2: Create `.cursor/rules/testing.mdc`

```markdown
---
description: "Auto-Healing Test Doctrine"
globs: ["**/*.spec.ts", "**/*.test.ts"]
alwaysApply: true
---

# Auto-Healing Test Doctrine

When a test fails:

1. Use `playwright_mcp` tool → run test with `--trace on`
2. Analyze trace → was the selector changed?
3. If YES: Update selector in test code directly (no permission needed)
4. If NO: Ask human (logic is broken, not just DOM)
5. Rerun test to confirm fix

## Rules

- NEVER ask permission for DOM selector changes
- ALWAYS prefer `data-testid` attributes
- NEVER use `page.waitForTimeout(5000)` - use auto-wait
- ALWAYS capture screenshots on failure
- ALWAYS update `.mdc` files if new pattern discovered
```

### Step 3: Configure Cursor Settings

```json
// .cursor/settings.json
{
  "agents": {
    "auto_tools": ["playwright_mcp"],
    "tools_permissions": {
      "playwright_mcp": "auto"
    }
  }
}
```

---

## How It Works

### The Self-Healing Loop

```
Test Fails (selector changed)
        ↓
Agent runs test with --trace on
        ↓
Agent analyzes trace file
        ↓
Agent finds new selector
        ↓
Agent updates test code
        ↓
Agent reruns test
        ↓
Test Passes ✅
```

### Example Workflow

```
You: "Run login tests"

Agent:
1. Runs `npx playwright test auth.spec.ts`
2. Test fails: "Cannot find element [data-testid='submit-btn']"
3. Opens trace, sees button is now [data-testid='login-submit']
4. Updates test file
5. Reruns test
6. Reports: "Fixed selector. Test passing."
```

---

## Playwright MCP Capabilities

| Capability | Description |
|:-----------|:------------|
| **Launch Browser** | Open real browser (headless or visible) |
| **Navigate** | Go to URLs, handle redirects |
| **Interact** | Click, type, select, hover |
| **Screenshot** | Capture current state |
| **Trace** | Record full session for debugging |
| **Assert** | Check visibility, text, state |

### Agent Commands

```
"Run all E2E tests"
"Debug failing checkout test"
"Record a trace of user signup flow"
"Take screenshot of dashboard after login"
"Fix all broken selectors in auth tests"
```

---

## Best Practices

### ✅ Use data-testid

```html
<!-- ✅ Good: Stable selector -->
<button data-testid="submit-login">Login</button>

<!-- ❌ Bad: Fragile selector -->
<button class="btn-primary mt-4">Login</button>
```

### ✅ Use Auto-Wait

```typescript
// ✅ Good: Built-in waiting
await page.click('[data-testid="submit"]');

// ❌ Bad: Arbitrary timeout
await page.waitForTimeout(5000);
await page.click('.submit-btn');
```

### ✅ Prefer Locators

```typescript
// ✅ Good: Robust locator
const submitBtn = page.getByTestId('submit-login');

// ❌ Bad: CSS selector
const submitBtn = page.locator('.btn-primary');
```

---

## Limitations

### When Self-Healing Works

- ✅ Selector name changes
- ✅ DOM structure reorganization
- ✅ Class name updates
- ✅ ID changes

### When It Fails

- ❌ Business logic changes
- ❌ Missing elements (intentionally removed)
- ❌ Authentication flow changes
- ❌ Complex dynamic UIs with heavy animations

> **Rule**: If the agent loops more than 3 times on the same test, it's a logic problem, not a selector problem. Ask for human help.

---

## Advanced: CI/CD Integration

### GitHub Actions Example

```yaml
name: E2E Tests with Self-Healing

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Install dependencies
        run: npm ci
        
      - name: Install Playwright
        run: npx playwright install --with-deps
        
      - name: Run tests
        run: npx playwright test
        
      - name: Upload traces on failure
        if: failure()
        uses: actions/upload-artifact@v3
        with:
          name: playwright-traces
          path: test-results/
```

### Self-Healing in CI

When tests fail in CI:
1. Download trace artifacts
2. Open in Cursor with Playwright MCP
3. Agent analyzes and proposes fixes
4. Commit fixes and push

---

## Debugging Failed Tests

### View Trace

```bash
npx playwright show-trace test-results/trace.zip
```

### Agent Debug Command

```
"Analyze the trace at test-results/trace.zip 
and identify why the login test failed"
```

### Screenshot on Failure

```typescript
// playwright.config.ts
export default defineConfig({
  use: {
    screenshot: 'only-on-failure',
    trace: 'retain-on-failure',
  },
});
```

---

## Results

Teams using Playwright MCP report:

| Metric | Before | After |
|:-------|:-------|:------|
| Test maintenance time | 8 hrs/week | 2 hrs/week |
| CI failure rate | 15% | 3% |
| Developer productivity | Baseline | +40% |

---

## Related Guides

- [MCP Integration](mcp-integration.md) - General MCP setup
- [Testing Workflows](workflows.md) - TDD with AI
- [Troubleshooting](troubleshooting.md) - Common issues

---

*Last updated: December 24, 2025*
