# 🎭 "PlayWhite": The Playwright MCP Workflow

**Trend:** Viral "Killer App" of Jan 2026
**Role:** Test-Driven Development on Autopilot

"PlayWhite" is the community nickname for connecting **Playwright** (via Model Context Protocol) to Cursor. It transforms the IDE into a self-healing testing lab.

## 🚀 The Workflow

1.  **Agent Runs Test:** You ask Cursor to "Verify the login flow".
2.  **Browser Launches:** Cursor uses `@playwright/mcp` to open a headless Chrome instance.
3.  **Test Fails:** The selector `.submit-btn` is missing.
4.  **Agent Analyzes:** The Agent inspects the DOM, sees the button is now `.btn-primary`.
5.  **Agent Fixes Code:** It updates `login.tsx` to match the test requirements.
6.  **Agent Re-Runs:** Verification passes.

All of this happens without you writing a single line of test code.

---

## 🛠️ Setup

1.  **Install the MCP Server:**

```bash
npm install -D @playwright/mcp
```

2.  **Configure `mcp.json`:**

Add this to your project's `mcp.json` (or global settings):

```json
{
  "mcpServers": {
    "playwright": {
      "command": "npx",
      "args": ["@playwright/mcp@latest"]
    }
  }
}
```

3.  **Usage Prompt:**

> "Using the Playwright MCP, navigate to http://localhost:3000. Try to log in with user 'admin' and password 'admin'. If it fails, take a screenshot and analyze the DOM to tell me why."

---

## ⚠️ Limitations

*   **Context usage:** Playwright logs are verbose. Use `Gemini 3 Pro` (2M context) if you are analyzing long test runs.
*   **Flakiness:** Agentic tests can be flaky. Always manually review critical test paths.
