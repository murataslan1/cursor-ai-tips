# 🎭 PlayWhite Workflow (Playwright MCP Self-Healing)

> **Status**: ✅ **RECOMMENDED** - The safest workflow during Cursor instability

"PlayWhite" is the colloquial name for the Playwright MCP integration workflow that emerged as the **primary defense** against the Zombie Revert bug. It forces the agent to prove code works via real browser verification.

---

## 🎯 Why PlayWhite?

With Cursor 2.3 stability issues, developers shifted trust away from "Composer" blind edits and toward **verification-first** workflows via MCP.

### The Core Principle

```
Instead of: Trust AI → Write Code → Hope it works
PlayWhite:  AI writes test → Runs real browser → Proves it works → THEN commit
```

---

## 🔄 The Self-Healing Loop

```
┌─────────────────────────────────────────────┐
│  1. Agent writes/modifies code              │
│                    ↓                        │
│  2. Agent runs E2E test via Playwright MCP  │
│                    ↓                        │
│  3. Test fails? (selector changed, etc.)    │
│        ↓ YES              ↓ NO              │
│  4a. Agent reads DOM      4b. Code is safe! │
│  5a. Agent fixes test         Commit ✅     │
│  6a. Agent verifies fix                     │
│        └──────────→ Loop back to 2          │
└─────────────────────────────────────────────┘
```

This isolates the agent's action. Instead of blindly trusting the agent to write code, Playwright forces the agent to **prove** the code works.

---

## 🛠️ Quick Setup

### Step 1: Create MCP Configuration

Create `.cursor/mcp.json` in your project root:

```json
{
  "mcpServers": {
    "playwright": {
      "command": "npx",
      "args": ["-y", "@playwright/mcp@latest"]
    }
  }
}
```

### Step 2: Restart Cursor

```
Cmd + Shift + P → "Developer: Reload Window"
```

### Step 3: Verify Connection

In Chat, type:
```
@playwright Can you list available tools?
```

You should see tools like `navigate`, `click`, `screenshot`, etc.

---

## 💬 The Magic Prompt

The most effective PlayWhite prompt pattern:

```
Run the login test. If it fails:
1. Read the error logs
2. Analyze the DOM to understand what changed
3. Fix the code (not just the test)
4. Run the test again
5. Repeat until it passes
```

### Real-World Example

```
@playwright
Navigate to localhost:3000/login
Fill username with "admin"
Fill password with "test123"
Click the Login button
Verify that the dashboard heading appears

If any step fails, analyze the page structure and fix the issue.
```

---

## 🎯 Use Cases

### 1. End-to-End Testing

```
"Write an E2E test for the checkout flow. 
Run it with Playwright. Fix any failures."
```

### 2. Visual Regression

```
"Take a screenshot of the homepage.
Compare it to the baseline.
If different, identify what changed."
```

### 3. Self-Healing Selectors

```
"The #submit-btn selector is broken.
Analyze the current DOM structure.
Update the selector to match the new HTML."
```

### 4. Debug with Evidence

```
"The form submission isn't working.
Navigate to the form, fill it out, submit.
Capture the network request and response.
Tell me what's wrong."
```

---

## 🛡️ Why It's Safe During "Holiday Freeze"

| Risk | PlayWhite Mitigation |
|------|---------------------|
| **Zombie Revert** | Tests verify state BEFORE commit |
| **Silent overwrites** | Browser provides ground truth |
| **Desynced Composer** | Playwright operates independently |
| **Infinite loops** | Test failure breaks the loop naturally |

---

## ⚙️ Advanced Configuration

### Add More MCP Tools

```json
{
  "mcpServers": {
    "playwright": {
      "command": "npx",
      "args": ["-y", "@playwright/mcp@latest"]
    },
    "postgres": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-postgres", "${env:DATABASE_URL}"]
    },
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"]
    }
  }
}
```

### Configure for CI/CD

```json
{
  "mcpServers": {
    "playwright": {
      "command": "npx",
      "args": ["-y", "@playwright/mcp@latest", "--headless"]
    }
  }
}
```

---

## 📋 .cursorrules for PlayWhite

Add to your `.cursorrules`:

```
## Testing Protocol

When writing or modifying frontend code:
1. ALWAYS create or update a Playwright test
2. Run the test via @playwright MCP
3. If test fails, fix code and rerun
4. Only consider task complete when test passes

NEVER use page.waitForTimeout()
ALWAYS use built-in auto-wait mechanisms
Target elements using data-testid attributes
```

---

## 🔗 Related Resources

- [Playwright MCP Setup](playwright-mcp-setup.md)
- [MCP Integration Guide](mcp-integration.md)
- [Holiday Freeze Protocol](holiday-freeze-protocol.md)
- [Self-Healing Tests](playwright-qa.md)

---

## Sources

- [Playwright MCP GitHub](https://github.com/microsoft/playwright-mcp)
- [Cursor MCP Docs](https://cursor.com/docs/cli/mcp)
- [Medium: Supercharge Testing with Playwright MCP](https://medium.com/@jagdalebr/supercharge-testing-with-playwright-mcp-server-and-cursor-ai-0e66f2430d11)
