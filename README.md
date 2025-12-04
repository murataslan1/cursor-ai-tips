<div align="center">

# Cursor AI Tips & Tricks

[![Awesome](https://awesome.re/badge.svg)](https://awesome.re)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)](http://makeapullrequest.com)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

**The ultimate guide to mastering Cursor AI IDE**

*Keyboard shortcuts, Composer workflows, .cursorrules examples, and Reddit community wisdom*

[Shortcuts](#-keyboard-shortcuts) • [Composer](#-composer-mode) • [Context](#-context-management) • [Rules](#-cursorrules) • [Models](#-model-selection) • [MCP](#-mcp-integration) • [Security](#-security-concerns) • [Troubleshooting](#-troubleshooting) • [Reddit Tips](#-reddit-community-wisdom)

</div>

---

## Why Cursor?

> 💰 **December 2025**: Anysphere (Cursor's parent company) valued at **$29.3 billion** after Series D funding, reflecting the explosive adoption of AI-assisted development.

Cursor is not just VS Code with AI - it's a **fork** that integrates LLMs directly into the rendering pipeline. This enables:

- **Cursor Tab**: Multi-line predictions (not just single line like Copilot)
- **Composer**: Autonomous multi-file editing agent
- **Shadow Workspace**: Background indexing for semantic search
- **Native Diffs**: Inline green/red diff visualization

---

## 🆕 What's New in Cursor 2.0/2.1

### Plan Mode (2.1)
Strategic thinking before coding. AI analyzes, plans, asks clarifying questions - then executes.

```
"Plan Mode forces 'measure twice, cut once' philosophy"
```

### Instant Grep (2.1)
Millisecond codebase search. No more context hallucinations - agent verifies every reference instantly.

### Multi-Agent Interface (2.0)
Run parallel agents using git worktrees:
- Agent 1: Refactoring component
- Agent 2: Writing tests
- No file lock conflicts

### Composer Improvements
- 4x faster than general LLMs for diff-edit loops
- Codebase-wide semantic search
- Implicit checkpoints for instant rollback

[→ Full 2.0/2.1 Guide](tips/cursor-2x-features.md)

### ⚠️ Deprecated Features

Some features were removed in 2.0/2.1:

| Removed | Replacement |
|---------|-------------|
| Interpreter Mode | Agent + Terminal |
| @Web, @Definitions | Auto-context |
| Reapply Button | Checkpoints |
| .cursorrules | .mdc files |
| Fast Request Packs | Usage-based pricing |

[→ Full Deprecated Features Guide](tips/deprecated-features.md)

### Lovable + Cursor Workflow

Popular "vibe coding" pattern for rapid prototyping:

```
1. Lovable → Design UI visually, connect to GitHub
2. Clone → Pull repo to local
3. Cursor → Add backend logic, APIs, complex features
4. Push → Sync back to Lovable
```

Users report building full SaaS products in 4 days using this hybrid approach.

[→ Full Lovable + Cursor Guide](tips/lovable-cursor-workflow.md)

---

## ⌨️ Keyboard Shortcuts

### The Command Hierarchy

| Command | Mac | Windows/Linux | Scope | When to Use |
|---------|-----|---------------|-------|-------------|
| **Inline Edit** | `Cmd + K` | `Ctrl + K` | Single file | Quick fixes, rename, split function |
| **Chat** | `Cmd + L` | `Ctrl + L` | Conversational | Explain code, debug, explore |
| **Composer** | `Cmd + I` | `Ctrl + I` | Multi-file | Refactoring, new features |
| **Composer Full** | `Cmd + Shift + I` | `Ctrl + Shift + I` | Multi-file | Large refactors, review diffs |
| **Add to Context** | `Cmd + Shift + L` | `Ctrl + Shift + L` | Selection | Add selected code to chat |
| **Terminal AI** | `Cmd + K` (in terminal) | `Ctrl + K` | Shell | Generate shell commands |

### Quick Reference

```
Cmd + K  → "Fix this type error"
Cmd + L  → "Explain how auth works"  
Cmd + I  → "Refactor to use Axios instead of Fetch"
```

> **Pro Tip**: Use `Cmd + K` for local scope, `Cmd + I` for global scope. Don't use `Cmd + K` for multi-file tasks.

[→ Full Shortcuts Guide](tips/keyboard-shortcuts.md) | [→ Development Workflows](tips/workflows.md)

---

## 🤖 Composer Mode

Composer is Cursor's **killer feature** - an autonomous agent that can plan and execute multi-file edits.

### Normal vs Agent Mode

| Mode | Description | Risk Level |
|------|-------------|------------|
| **Normal** | Proposes edits, you click "Accept" | Safe |
| **Agent** | Creates/deletes files, runs terminal | ⚠️ Use with caution |

### Checkpoints (Time Travel)

Composer creates snapshots at each step. If the AI breaks something:

1. Click previous Checkpoint
2. Workspace reverts instantly
3. Try different approach

### Effective Prompting

❌ **Bad**: "Add a login page"

✅ **Good**:
```
Implement a login route:
- @user_model.ts @auth_service.ts @routes.json
- Use Zod for validation
- Match error format in @errors.ts
- Create unit test in tests/auth/
```

[→ Full Composer Guide](tips/composer-mode.md)

---

## 📎 Context Management

The AI is only as good as the context you provide.

### @ Symbol Reference

| Symbol | What it does | Best for |
|--------|--------------|----------|
| `@Files` | Full file content | Active editing |
| `@Folders` | File tree + summaries | Architecture questions |
| `@Codebase` | Semantic RAG search | "Where is X used?" |
| `@Docs` | External documentation | Third-party APIs |
| `@Git` | Git history/diff | Commit messages, history |
| `@Web` | Web search | Current info |

### Pro Tips

```
@Codebase is probabilistic - if you call it "Login" 
but code says "SessionCreation", RAG may fail.

Use explicit @Files for critical tasks.
```

### Notepads (Persistent Context)

Create a `current_task_spec` Notepad with:
- PRD / Requirements
- Design constraints
- Architecture decisions

Reference with `@current_task_spec` in every new chat.

[→ Full Context Guide](tips/context-management.md) | [→ Security Best Practices](tips/security-concerns.md)

---

## 📋 .cursorrules

System prompts that customize AI behavior per project.

### Basic Setup

Create `.cursorrules` in project root:

```
You are an expert TypeScript engineer.
Use functional components with Hooks.
Use Tailwind CSS for styling.
Never use CSS modules or styled-components.
Every function needs a unit test.
```

### Advanced: .mdc Files

New system uses `.cursor/rules/*.mdc` with glob patterns:

```yaml
---
description: "React Component Rules"
globs: ["src/**/*.tsx"]
alwaysApply: false
---

Use shadcn/ui for primitives.
Components in src/ui must be presentational only.
Business logic goes in src/services.
```

### Essential Rules

| Category | Example Rule |
|----------|--------------|
| **Tech Stack** | "Use Tailwind. Never styled-components." |
| **Architecture** | "Services in src/services, UI in src/ui" |
| **Anti-Lazy** | "Output FULL file. No placeholders. No `//...existing code`" |
| **Testing** | "Every function needs unit test in tests/" |

### The Anti-Lazy Prompt (Reddit Gold)

```
You are an expert engineer.
You DO NOT use placeholders.
You output the FULL content of the file every time.
You do not be lazy.
```

[→ Full .cursorrules Guide](tips/cursorrules-guide.md) | [→ .mdc Examples](tips/mdc-examples.md)

---

## 🧠 Model Selection (2025 Update)

### Latest Models Comparison

| Model | Best For | Context | Speed | Cost |
|-------|----------|---------|-------|------|
| **Claude 4.5 Opus** | Planning, reliability, backend | 200K | Medium | $$$ |
| **GPT-5.1 High Max** | Architecture, balanced reasoning | 128K | Medium | $$ |
| **Gemini 3 Pro** | Visuals, massive context, frontend | **2M** | Fast | $ |
| **Kimi k2 Thinking** | Cost-effective reasoning, open-source | 256K | Medium | ¢ |
| **Grok 4.1** | Personality, real-time data, vibe coding | 256K | Fast | $$ |

### Model Personalities

| Model | "Vibe" |
|-------|--------|
| Claude 4.5 | Strict Senior Developer |
| GPT-5.1 | Pragmatic Architect |
| Gemini 3 | Creative Designer |
| Kimi k2 | Efficient Researcher |
| Grok 4.1 | Witty Collaborator |

### The Plan-Act Pattern

```
1. PLAN (Claude 4.5 / GPT-5.1 High):
   "Analyze request. Don't code. Create plan.md"
   
2. CRITIQUE (Gemini 3 Pro - optional):
   "Review plan.md for efficiency gaps"
   
3. EXECUTE (Composer / GPT-5.1):
   "Implement Step 1 of plan.md. Run tests."
```

### Thinking Models (Kimi k2, Grok 4.1)

Don't over-prompt! Give high-level goals:

```
❌ "Write function that does X, Y, Z step by step"
✅ "Goal: Implement resilient payment processor. 
    Reason about Stripe failure modes. Then output code."
```

### Cost Optimization

```
Pro Plan ($20/mo):
├── Credit pool system (fast vs slow)
├── "Auto" mode switches models by task complexity
└── Set per-user spending limits (Enterprise)

Strategy:
├── Daily work → Pro Plan (Auto mode)
├── Heavy refactoring → BYOK Claude 4.5
├── Budget tasks → Kimi k2 via OpenRouter
└── Hard bugs → GPT-5.1 High Max
```

> **Warning**: Don't switch models mid-conversation. It breaks the "train of thought."

[→ Full Model Guide](tips/model-selection.md)

---

## 🔌 MCP Integration

Model Context Protocol lets Cursor connect to databases, GitHub, and browsers.

### Quick Setup

Create `mcp.json` in project root:

```json
{
  "mcpServers": {
    "postgres": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-postgres", "${env:DATABASE_URL}"]
    }
  }
}
```

### Popular Servers

| Server | Use Case |
|--------|----------|
| `server-postgres` | Query database schema |
| `github-mcp-server` | Manage PRs/Issues |
| `server-puppeteer` | Browser automation |
| `@playwright/mcp` | E2E testing, self-healing tests |
| `@sentry/mcp-server` | Production error debugging |

### Playwright Integration (Self-Healing Tests)

The most transformative MCP application:

```
1. Agent runs E2E test
2. Test fails (selector changed)
3. Agent analyzes DOM
4. Agent fixes test automatically
5. Agent verifies fix
```

[→ Full MCP Guide](tips/mcp-integration.md)

---

## 🔒 Security Concerns

> ⚠️ As AI agents gain autonomy, security risks increase exponentially.

### The Agentic Attack Surface

The ability of an Agent to execute shell commands and connect to the internet (via MCP) introduces new risks:

| Risk | Description |
|------|-------------|
| **Prompt Injection** | Malicious READMEs or docs that trick the AI |
| **Credential Exfiltration** | Agent accessing environment variables |
| **MCP Exploits** | Malicious MCP servers hijacking machines |
| **YOLO Mode Dangers** | Auto-execution of destructive commands |

### Security Best Practices

```
✅ Review all terminal commands before approval
✅ Use read-only MCP configurations
✅ Audit .mdc files in cloned repositories
✅ Never approve `env` or `printenv` commands
✅ Set strict YOLO mode restrictions
❌ Don't trust arbitrary @Docs sources
❌ Don't enable unrestricted shell access
```

### The "Black Box" Risk

```
⚠️ "Vibe Coding" can create unmaintainable code

If you don't understand the AI-generated code:
- You can't debug it when AI fails
- It becomes "Legacy Code" immediately
- Security vulnerabilities go unnoticed
```

[→ Full Security Guide](tips/security-concerns.md)

---

## 🔧 Troubleshooting

### Quick Fixes

| Problem | Solution |
|---------|----------|
| "Connection Failed" | New chat (Cmd+L), disable HTTP/2 |
| "Stuck Generating" | New Composer (Cmd+N) |
| Files deleted by Agent | Use checkpoint to restore |
| Rules ignored | Restart Cursor |
| High token usage | Set API spending limits |

### The "Single Purpose Composer" Rule

Don't reuse Composer windows. One task = one Composer. Prevents context pollution.

### Always Commit Before Agent

```bash
git add -A && git commit -m "checkpoint"
```

[→ Full Troubleshooting Guide](tips/troubleshooting.md)

---

## 🔥 Reddit Community Wisdom

Tips from r/cursor power users:

### The "Fresh Chat" Rule

> If debugging exceeds ~20 messages, context is polluted. Start NEW chat with summary.

### The "Delete Bug"

> In Agent mode, Cursor sometimes deletes and recreates files instead of editing. **Always commit before Agent sessions.**

### Screenshot Debugging

> Paste UI bug screenshots directly into chat. Vision models diagnose CSS issues better than text descriptions.

### Cost Control

> Set hard limits in OpenAI/Anthropic dashboard. Runaway Agent loops can drain your credit card.

### Model Switching

> Don't switch from GPT-4o to Claude mid-task. Stick to one model per conversation.

[→ Full Reddit Tips](tips/common-mistakes.md)

---

## 🔄 Workflows

### Research-First Protocol

```
1. Discovery: "Map usage of User component" (no code yet)
2. Plan: "Propose refactor plan, list affected files"
3. Critique: Review plan, challenge assumptions
4. Execute: Open Composer, paste approved plan
5. Audit: Review git diff line-by-line
```

### TDD with AI

```
1. "Write Vitest test for compound interest calculator"
2. Review test logic
3. "Now write function to pass this test"
```

### Debug with AI

```
1. Command fails in terminal
2. Click "Debug with AI" button
3. AI gets full error context automatically
4. Don't manually copy-paste errors
```

[→ Full Workflows Guide](tips/workflows.md)

---

## ⚔️ Cursor vs Competitors (2025)

| Feature | Cursor 2.1 | Google Antigravity | Windsurf | GitHub Copilot |
|---------|------------|-------------------|----------|----------------|
| **Philosophy** | Augmentation | Full Autonomy | Augmentation | Assistance |
| **Architecture** | Fork (Native) | New IDE | Fork (Native) | Extension |
| **Multi-File** | Composer ⭐ | Agents | Cascade | Limited |
| **Model Choice** | All models | Gemini only | Proprietary | OpenAI only |
| **Special** | Plan Mode, Instant Grep | Mission Control, Artifacts | Auto-context | Enterprise SSO |
| **Price** | $20/mo (500 fast/mo) | Free Preview | $15/mo | $10/mo |

### Cursor vs Google Antigravity

| Aspect | Cursor | Antigravity |
|--------|--------|-------------|
| **Control** | Developer is pilot | Developer is mission controller |
| **Verification** | Git diffs, code review | Artifacts (screenshots, logs) |
| **Best For** | Precision, existing codebases | Greenfield, rapid prototyping |
| **Lock-in** | Model agnostic | Google ecosystem |

### The "Initiative Gap"

A critical differentiator between Cursor and Windsurf:

| Aspect | Cursor | Windsurf |
|--------|--------|----------|
| **Autonomy** | Asks permission by default | Acts proactively |
| **Shell Commands** | Requires approval | Often auto-executes |
| **User Feeling** | Controlled, predictable | "Magical" but scary |

> "Windsurf feels magical to new users, but terrifying to senior engineers who want to review every shell command."

**Verdict**: Cursor for **control and precision**. Antigravity for **full automation experiments**. Windsurf for **auto-context** (but watch the autonomy). Copilot for **enterprise compliance**.

[→ Full Comparison](tips/vs-competitors.md) | [→ Cursor vs Windsurf Deep Dive](tips/cursor-vs-windsurf.md)

---

## 💡 Vibe Coding

The new development paradigm where domain experts focus on intent while AI implements.

### The Cultural Divide: "Shadow Engineers" vs "Vibe Coders"

| Type | Description | Strength | Risk |
|------|-------------|----------|------|
| **Shadow Engineer** | Manages AI agents, writes PRDs, verifies output | Full control, maintainable code | Slower iteration |
| **Vibe Coder** | Relies entirely on natural language | Rapid MVPs | "Black box" code, debugging walls |

> ⚠️ **Warning**: If you don't understand the AI-generated code, you can't debug it when AI fails. It becomes "Legacy Code" immediately.

### What is Vibe Coding?

```
Traditional: Developer writes code → Tests → Iterates
Vibe Coding: Developer describes intent → AI implements → Developer reviews
```

### Success Stories

- **Tradofire**: Solo developer shipped complex crypto trading app
- **Enterprise ERP**: Full-scale systems built in weeks using TaskMaster workflow

### The TaskMaster Workflow

```
1. Generate detailed PRD (Product Requirement Document)
2. Feed PRD to task management system
3. Agent parses PRD into individual tickets
4. Execute tickets one by one
5. Human review at each milestone
```

[→ Full Vibe Coding Guide](tips/vibe-coding-guide.md)

---

## 🚀 Quick Start Path

### Beginner (Week 1)
1. Learn `Cmd + K` for inline edits
2. Learn `Cmd + L` for chat
3. Use `@Files` to add context

### Intermediate (Week 2-3)
1. Master `Cmd + I` Composer
2. Create `.cursorrules`
3. Use `@Codebase` for exploration

### Advanced (Month 2+)
1. Agent Mode with checkpoints
2. Custom `.mdc` rules per file type
3. Research-First protocol
4. BYOK API for heavy tasks

---

## 📚 Resources

- [Official Docs](https://docs.cursor.com)
- [r/cursor](https://reddit.com/r/cursor)
- [Cursor Forum](https://forum.cursor.com)
- [awesome-cursorrules](https://github.com/PatrickJS/awesome-cursorrules)

---

## 🤝 Contributing

Found a tip? Share it!

1. Fork this repo
2. Add your tip to relevant file
3. Include source (Reddit, Twitter, etc.)
4. Open PR

---

<div align="center">

**Star this repo if it helped you! ⭐**

Made with 💙 by [Murat Aslan](https://github.com/murataslan1)

*Last updated: December 2025*

</div>
