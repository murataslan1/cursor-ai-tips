# Model Context Protocol (MCP) Integration

[← Back to Main](../README.md)

> MCP is "USB-C for AI" - universal standard for connecting Cursor to databases, APIs, and tools.

---

## What is MCP?

If LLMs are the "brain" of the new development stack, MCP is the "nervous system." Introduced by Anthropic and rapidly adopted by Cursor, MCP provides a standardized interface for AI models to connect with external data and tools.

Enables Cursor Agent to:
- Query databases directly
- Manage GitHub PRs/Issues
- Control browsers for testing
- Access custom APIs
- Read production error logs (Sentry)
- Execute autonomous testing loops

---

## Transport Methods

### Stdio (Local)
```
Cursor → spawns process → stdin/stdout JSON-RPC
Best for: Local databases, dev tools
```

### SSE (Remote)
```
Cursor → HTTP → Server-Sent Events
Best for: Team-shared resources, cloud services
```

---

## Configuration: mcp.json

### File Location

As of Cursor 2.1, the recommended location is `.cursor/mcp.json` (project-specific):

```bash
mkdir -p .cursor
touch .cursor/mcp.json
```

> **Note**: Project root `mcp.json` still works for backward compatibility, but `.cursor/mcp.json` is preferred for isolation.

### Basic Structure

```json
{
  "mcpServers": {
    "server-name": {
      "command": "executable",
      "args": ["arg1", "arg2"],
      "env": { "KEY": "value" }
    }
  }
}
```

---

## Example: PostgreSQL

```json
{
  "mcpServers": {
    "postgres": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-postgres", "${env:DATABASE_URL}"],
      "env": { "DANGEROUSLY_ALLOW_WRITE_OPS": "false" }
    }
  }
}
```

**Usage**: "Check Users table schema and create TypeScript interface"

---

## Example: GitHub

```json
{
  "mcpServers": {
    "github": {
      "command": "docker",
      "args": ["run", "-i", "--rm", "-e", "GITHUB_PERSONAL_ACCESS_TOKEN", "ghcr.io/github/github-mcp-server"]
    }
  }
}
```

**Usage**: "Find open issues labeled 'bug'" or "Create PR with these changes"

---

## Example: Puppeteer (Browser)

```json
{
  "mcpServers": {
    "browser": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-puppeteer"]
    }
  }
}
```

**Usage**: "Navigate to localhost:3000 and screenshot login page"

---

## Multi-Server Setup

```json
{
  "mcpServers": {
    "postgres": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-postgres", "${env:DATABASE_URL}"]
    },
    "github": {
      "command": "docker",
      "args": ["run", "-i", "--rm", "-e", "GITHUB_PERSONAL_ACCESS_TOKEN", "ghcr.io/github/github-mcp-server"]
    },
    "browser": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-puppeteer"]
    }
  }
}
```

---

## Available Servers

| Server | Package | Purpose |
|--------|---------|---------|
| PostgreSQL | `@modelcontextprotocol/server-postgres` | DB queries |
| SQLite | `@modelcontextprotocol/server-sqlite` | Local DB |
| Filesystem | `@modelcontextprotocol/server-filesystem` | File access |
| Puppeteer | `@modelcontextprotocol/server-puppeteer` | Browser |
| GitHub | `ghcr.io/github/github-mcp-server` | PR/Issues |

---

## Security

1. **Never hardcode secrets** - Use `${env:VAR_NAME}`
2. **Read-only default** - `DANGEROUSLY_ALLOW_WRITE_OPS: false`
3. **Limit paths** - Restrict filesystem access
4. **Use official images** - For Docker-based servers

---

## Troubleshooting

- **Not working**: Restart Cursor after adding mcp.json
- **Server error**: Test manually with `npx -y @modelcontextprotocol/server-postgres $DATABASE_URL`
- **Permissions**: Check env vars are set correctly

---

---

## Example: Playwright (E2E Testing) - "PlayWhite"

The most transformative MCP application - enables "Self-Healing Tests."

> 💡 **"PlayWhite"** is the community nickname for Playwright MCP. As one user put it: *"UTILIZEM PLAYWHITE, É RAPIDO, FACIL E BOM DEMAIS"* (Use PlayWhite, it's fast, easy and amazing!)

### Configuration (.cursor/mcp.json)

```json
{
  "mcpServers": {
    "playwright": {
      "command": "npx",
      "args": [
        "-y",
        "@playwright/mcp@latest"
      ],
      "env": {
        "PLAYWRIGHT_BROWSERS_PATH": "0"
      }
    }
  }
}
```

> **Important**: `PLAYWRIGHT_BROWSERS_PATH: "0"` ensures browsers are downloaded to the default location, avoiding path issues.

### The Autofix Workflow (Shadow Engineering)

```
1. Context Injection: User provides URL (localhost) or test file
2. Tool Execution: Agent launches browser (headed or headless)
3. Perception: Agent captures DOM, console errors, screenshots
4. Remediation: If test fails, Agent analyzes and fixes code
5. Verification: Agent re-runs test to confirm fix
```

### Why "Shadow Engineering"?

This workflow allows a single developer to maintain a test suite that would typically require a dedicated QA engineer:

```
Before PlayWhite:
- Test breaks → Developer manually debugs → Hours lost
- Selector changed → Manual inspection → Context switching

After PlayWhite:
- Test breaks → Agent identifies #submit-btn renamed to #login-btn
- Agent edits login.spec.ts → Re-runs test → Minutes, not hours
```

### Usage Examples

```
"Run the login E2E test and fix any failures"
"Navigate to localhost:3000/dashboard and verify all elements load"
"Take a screenshot of the checkout page and identify CSS issues"
```

### Why This Matters

E2E tests often break with minor UI changes. With MCP, the Agent can "heal" tests automatically:

```
Before MCP:
- Test breaks → Developer manually fixes → Hours lost

After MCP:
- Test breaks → Agent identifies issue → Agent fixes → Minutes
```

---

## Example: Sentry (Production Debugging)

Connect to production error logs for faster debugging.

```json
{
  "mcpServers": {
    "sentry": {
      "command": "npx",
      "args": ["-y", "@sentry/mcp-server"],
      "env": {
        "SENTRY_AUTH_TOKEN": "${env:SENTRY_AUTH_TOKEN}",
        "SENTRY_ORG": "your-org"
      }
    }
  }
}
```

### Production-to-Development Loop

```
Instead of:
- User reports bug
- Developer searches Sentry
- Copy-pastes stack trace
- Debugs manually

With MCP:
- "Fix the error from Sentry issue #12345"
- Agent queries Sentry directly
- Gets full stack trace, correlation ID, variable state
- Proposes fix in local code
```

---

## Headed vs Headless Browser

### Headed Mode (Visible)

```
Pros:
- Watch Agent "click around"
- Trust verification
- Debug visual issues

Cons:
- Slower
- DISPLAY issues in WSL2/Linux
- More resource intensive
```

### Headless Mode (Default)

```
Pros:
- Faster
- Works in CI/CD
- No GUI dependencies

Cons:
- Can't watch execution
- May miss visual regressions
```

### Forcing Headed Mode

```json
{
  "mcpServers": {
    "playwright": {
      "command": "npx",
      "args": ["@playwright/mcp@latest", "--headed"]
    }
  }
}
```

---

## References

- [Cursor MCP Docs](https://cursor.com/docs/cookbook/building-mcp-server)
- [GitHub MCP Server](https://github.com/github/github-mcp-server)
- [Playwright MCP](https://github.com/microsoft/playwright-mcp)
- [Sentry MCP Guide](https://skywork.ai/skypage/en/sentry-mcp-server-ai-debugging/)

---

[← Back to Main](../README.md)
