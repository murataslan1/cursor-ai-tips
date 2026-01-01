<div align="center">

# 🚀 Cursor AI Tips & Tricks

[![GitHub stars](https://img.shields.io/github/stars/murataslan1/cursor-ai-tips?style=social)](https://github.com/murataslan1/cursor-ai-tips/stargazers)
[![GitHub forks](https://img.shields.io/github/forks/murataslan1/cursor-ai-tips?style=social)](https://github.com/murataslan1/cursor-ai-tips/network/members)
[![Awesome](https://awesome.re/badge.svg)](https://awesome.re)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)](http://makeapullrequest.com)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Last Updated](https://img.shields.io/badge/Updated-January%201,%202026-brightgreen)](https://github.com/murataslan1/cursor-ai-tips)
[![Cursor Version](https://img.shields.io/badge/Cursor-2.3.9-blue)](https://cursor.com/changelog)

**The ultimate guide to mastering Cursor AI IDE**

*Keyboard shortcuts, Composer workflows, .cursorrules examples, Gemini 3 Pro benchmarks, and Reddit community wisdom*

[⚠️ Holiday Freeze](#-holiday-freeze-protocol) • [🆕 Cursor 2.3](#-whats-new-in-cursor-23) • [🧠 Gemini 3 Pro](#-gemini-3-pro-guide) • [💸 Budget Models](#-budget-models-2026) • [📊 Market Stats](#-cursor-market-statistics) • [Shortcuts](#-keyboard-shortcuts) • [Composer](#-composer-mode) • [Models](#-model-selection) • [MCP](#-mcp-integration) • [🎭 PlayWhite](#-playwhite-workflow) • [🔮 2026 Outlook](#-2026-predictions)

</div>

---

> [!CAUTION]
> **🚨 CRITICAL: Zombie Revert Bug Active in Cursor 2.2.36+**
> 
> Cursor is randomly reverting code without consent, causing **data loss**. This bug persists in 2.3.
> 
> **Do NOT update** until hotfix is released. Use defensive commits. [Read the Holiday Freeze Protocol →](tips/zombie-revert-bug.md)

---

## Why Cursor?

> 💰 **January 2026**: Anysphere (Cursor's parent company) valued at **$29.3 billion**. With **~$500M ARR**, **50% of Fortune 500** adoption, and **1M+ queries/second**, Cursor is the fastest-growing AI dev tool.

[→ Full Market Statistics](tips/cursor-market-stats.md)

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

## 🆕 What's New in Cursor 2.3 (December 22, 2025)

> 🎯 **"Stability and polish" release** - Major architectural improvements under the hood.

### Process Separation (The Big Change)

Extensions now run in an isolated process. If an extension crashes, **AI keeps working**:

```
Before: Extension crash → Everything freezes
After:  Extension crash → AI continues working ✅
```

This is critical for enterprise users with large codebases.

### Layout Customization Engine

Four preset layouts with `⌘+⌥+⇥` (Mac) / `Ctrl+Alt+Tab` (Win):

| Mode | Description | Best For |
|:-----|:------------|:---------|
| **Agent** | 50/50 Chat + Editor | Pair programming with AI |
| **Editor** | Maximized editor | Deep focus |
| **Zen** | Hidden chrome | Complex algorithms |
| **Browser** | Split with Chromium | Frontend dev |

### Enterprise Features

- **Service Accounts** - Headless CI/CD automation
- **SOC 2 Certified** - Enterprise compliance ready
- **Enforcement Hooks** - Block sensitive data in prompts
- **Linux Sandboxing** - Container-friendly deployments

[→ Full 2.3 Guide](tips/cursor-23-features.md) | [→ Enterprise Features](tips/enterprise-features.md)

---

## 🚨 Holiday Freeze Protocol

> ⚠️ **ALERT LEVEL: 🔴 DO NOT UPDATE** - Maintain v2.2.35 until Cursor 2.4

The developer community declared a "Holiday Freeze" starting December 24, 2025, after the "Zombie Revert" bug caused critical data loss in Cursor 2.3.

### Protocol Directives (Dec 24 - Jan 7)

| Status | Action | Risk/Reason |
|:------:|--------|-------------|
| ❌ **PROHIBITED** | Update to 2.3.x | "Zombie Revert" data loss |
| ❌ **PROHIBITED** | Multi-file Agent | Race conditions with disk |
| ❌ **PROHIBITED** | "Auto" Model Selection | No deterministic control |
| ⚠️ **CAUTION** | Visual Editor | Infinite "Apply/Undo" loops |
| ✅ **SAFE** | Single-file edits | Lower desync risk |
| ✅ **SAFE** | Planning Mode | Doesn't touch file I/O |
| ✅ **RECOMMENDED** | Playwright MCP | "PlayWhite" verification workflow |

### The "Split-Brain" Scenario

```
Editor (Process 1) ←?→ Composer (Process 2) ←?→ File System
                              ↓
              Desync = "Zombie Revert" (Silent Data Loss)
```

### Defensive Commit (CRITICAL)

```bash
# BEFORE every agent operation:
git add -A && git commit -m "pre-agent-$(date +%s)"
```

### Visual Editor Infinite Loop Bug (NEW in 2.3.9)

The Visual Editor can enter infinite loops:
- Agent applies CSS change
- Browser renders slowly (latency)
- Agent sees old state, tries again
- Loop burns API credits indefinitely

**Workaround**: Use Source View only until 2.4.

[→ Full Holiday Freeze Protocol](tips/holiday-freeze-protocol.md) | [→ Zombie Revert Details](tips/zombie-revert-bug.md) | [→ PlayWhite Workflow](tips/playwhite-workflow.md)

---

## 🧠 Gemini 3 Pro Guide

> 🏆 **HLE Benchmark Leader**: 37.5% - Best reasoning performance ever recorded.

### Model Comparison (December 2025)

| Model | HLE Score | Best For | Cost |
|:------|:----------|:---------|:-----|
| **Gemini 3 Pro** | 37.5% | Architecture, reasoning | Free (Beta) |
| Claude Opus 4.5 | 34.8% | Legacy refactoring | $$$ |
| GPT-5.2 | 32.1% | UI/UX, speed | $$ |
| DeepSeek V3 | 28.9% | Budget bulk work | ¢ |

### New Model Strategy

```
Architecture Planning  → Gemini 3 Pro (Free, best reasoning)
Daily Implementation   → Claude 3.5 Sonnet (Best bang-for-buck)
Legacy Refactoring     → Claude Opus 4.5 (Lowest risk)
Bulk Test Writing      → DeepSeek V3 (53x cheaper)
```

### Configure Gemini for Codebase Indexing

```
Settings → Codebase Indexing → Model → Gemini 3 Pro
```

[→ Full Gemini 3 Pro Guide](tips/gemini-3-pro-guide.md)

---

## 💸 Budget Models (2026)

> 💡 **Cost Revolution**: DeepSeek V3 and Google Flash offer Claude-level quality at 15-50x lower cost.

### The New Cost Landscape

| Model | Input/1M | Output/1M | vs Claude Opus |
|-------|----------|-----------|----------------|
| Claude Opus 4.5 | $15.00 | $75.00 | Baseline |
| GPT-5.2 | $1.75 | $14.00 | 5x cheaper |
| **Google Flash** | $0.10 | $1.67 | **45x cheaper** |
| **DeepSeek V3** | ~$0.14 | ~$0.28 | **268x cheaper** |

### DeepSeek V3 + R1

China-based open-source model gaining rapid adoption:

```
✅ 15x cheaper than Claude
✅ Strong reasoning capabilities  
✅ BYOK via OpenRouter
⚠️ Less battle-tested
⚠️ Use Claude for security-critical code
```

### Google Flash (Gemini 2.0 Flash)

```
✅ 45x cheaper than Claude Opus
✅ Near-instantaneous responses
✅ 1M token context window
⚠️ Less reliable for agentic workflows
```

### Tiered Cost Strategy (2026)

```
Tier 1 (Critical Production):
├─ Claude Opus 4.5 ($75/M output)
└─ Security-critical, high-stakes refactors

Tier 2 (Daily Development):
├─ Google Flash ($1.67/M output) ← 45x cheaper!
├─ DeepSeek V3 ($0.28/M output)
└─ Routine CRUD, boilerplate

Tier 3 (Planning/Architecture):
├─ Gemini 3 Pro (Free Beta)
└─ Design reviews, architecture planning
```

[→ DeepSeek V3 Guide](tips/deepseek-v3-guide.md) | [→ Google Flash Guide](tips/google-flash-guide.md)

---

## 📊 Cursor Market Statistics

> 🚀 Cursor is approaching feature parity with GitHub Copilot's **~$500M ARR**.

### Growth Metrics (October 2025 - Official)

| Metric | Value |
|--------|-------|
| **Load Increase** | 100x in 1 year |
| **Queries/Second** | 1M+ peak |
| **Paying Customers** | 40,000+ |
| **Fortune 500 Adoption** | ~50% |
| **ARR** | ~$500M |
| **Valuation** | $29.3B (Dec 2025) |

### Market Share (2025)

```
GitHub Copilot ████████████████████ 40% (flat)
Cursor         ████████████████░░░░ 35% (rising fast)
Windsurf       ████░░░░░░░░░░░░░░░░ 8%
Cline          ███░░░░░░░░░░░░░░░░░ 7%
Others         ██░░░░░░░░░░░░░░░░░░ 10%

Projection: Cursor overtakes Copilot in Q2 2026
```

### Notable Enterprise Customers

OpenAI | Samsung | Midjourney | Perplexity | Shopify

[→ Full Market Statistics](tips/cursor-market-stats.md)

---

## 🔍 Debug Mode (Detailed Guide)

> **Success Rate**: ~70% first-attempt fix | **ROI**: 60% debugging time reduction

Debug Mode transforms debugging from guesswork into a scientific process.

### The Debug Mode Workflow

```
┌─────────────────────────────────────────────────────────┐
│  1. HYPOTHESIS GENERATION                               │
│     Agent scans codebase, generates multiple theories   │
│                         ↓                               │
│  2. ACTIVE INSTRUMENTATION                              │
│     Agent autonomously inserts logging statements       │
│                         ↓                               │
│  3. YOU TRIGGER THE BUG                                 │
│     Reproduce the issue with instrumented code          │
│                         ↓                               │
│  4. RUNTIME ANALYSIS                                    │
│     Agent ingests logs, compares to mental model        │
│                         ↓                               │
│  5. ROOT CAUSE IDENTIFICATION                           │
│     Agent accepts/rejects hypotheses based on evidence  │
│                         ↓                               │
│  6. FIX PROPOSAL                                        │
│     Targeted fix based on empirical data                │
└─────────────────────────────────────────────────────────┘
```

### Quick Usage

```
1. Open Chat (Cmd + L)
2. Click "Debug" icon OR type "/debug"
3. Describe the bug clearly
4. Let agent instrument code
5. Reproduce the bug
6. Review and accept fix
```

[→ Full Debug Mode Guide](tips/debug-mode-guide.md)

---

## 🎭 PlayWhite Workflow

> **Status**: ✅ RECOMMENDED - The safest workflow during Cursor instability

"PlayWhite" is the Playwright MCP integration workflow that emerged as the **primary defense** against the Zombie Revert bug.

### The Core Principle

```
Instead of: Trust AI → Write Code → Hope it works
PlayWhite:  AI writes test → Runs browser → Proves it works → Commit
```

### The Self-Healing Loop

```
1. Agent writes/modifies code
2. Agent runs E2E test via Playwright MCP
3. Test fails? Agent analyzes DOM, fixes code
4. Agent verifies fix
5. Test passes → Safe to commit ✅
```

### Quick Setup

```json
// .cursor/mcp.json
{
  "mcpServers": {
    "playwright": {
      "command": "npx",
      "args": ["-y", "@playwright/mcp@latest"]
    }
  }
}
```

### The Magic Prompt

```
Run the login test. If it fails:
1. Read the error logs
2. Analyze the DOM
3. Fix the code (not just the test)
4. Run again until it passes
```

[→ Full PlayWhite Workflow Guide](tips/playwhite-workflow.md)

---

## 🔮 2026 Predictions

Strategic forecasts based on December 2025 intelligence.

### Prediction Matrix

| Prediction | Confidence | Timeline |
|------------|:----------:|----------|
| Cursor overtakes Copilot market share | 🟢 High | Q2 2026 |
| DeepSeek V3 gains 10%+ market share | 🟢 High | Q1 2026 |
| Multi-Agent Judging becomes standard | 🟢 High | Q1 2026 |
| Google Flash challenges Claude Sonnet | 🟡 Medium | Q2 2026 |
| Cursor 2.4 focuses on performance | 🟡 Medium | Q1 2026 |

### Key Dates to Watch

| Date | Expected Event |
|------|----------------|
| Jan 6-10, 2026 | Cursor 2.3 "all clear" |
| Q1 2026 | Cursor 2.4 release |
| Q2 2026 | Cursor overtakes Copilot |

### Strategic Recommendations

```
Individual Developers:
├─ Adopt tiered model strategy
├─ Master Playwright MCP
└─ Build personal .cursorrules library

Team Leads:
├─ Standardize on Cursor v2.2.35
├─ Implement model routing policy
└─ Mandate pre-agent Git commits

CTOs:
├─ Evaluate Cursor Enterprise
├─ Budget for model diversity
└─ Track cost-per-feature metrics
```

[→ Full 2026 Predictions](tips/2026-predictions.md)

---

## 🆕 What's New in Cursor 2.2


> ⚠️ **WARNING**: Cursor 2.2 has critical bugs. See [Cursor 2.2 Bugs](tips/cursor-22-bugs.md) before using.

### Debug Mode (2.2)

Agent instruments your code with logging, you trigger the bug, agent analyzes runtime data for empirical debugging.

```
1. Describe bug → 2. Agent adds logging → 3. YOU trigger bug → 4. Agent analyzes → 5. Fix proposed
```

### Visual Editor (2.2)

Bidirectional DOM ↔ Source Code editing. Select elements in browser, modify via GUI, changes write to source files.

### Multi-Agent Judging (2.2)

Multiple agents solve your prompt in parallel. "Judge" agent picks the best solution. Increases token cost but improves quality.

### ⚠️ Critical Bugs in 2.2

| Bug | Impact | Workaround |
|-----|--------|------------|
| **Revert Broken** | Data loss! | Git commit before every agent call |
| **Visual Editor Loop** | Infinite re-apply | Reload window |
| **WSL Terminal** | Agent can't run commands | Enable Legacy Terminal |

[→ Full 2.2 Features Guide](tips/cursor-22-features.md) | [→ 2.2 Bugs & Workarounds](tips/cursor-22-bugs.md)

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
| **GPT-5.2 Thinking** | Reliability, Agent Mode, debugging | **400K** | Medium | $$ |
| **Claude 4.5 Opus** | Planning, deep reasoning, backend | 200K | Medium | $$$ |
| **GPT-5.1 High Max** | Architecture, balanced reasoning | 128K | Medium | $$ |
| **Gemini 3 Pro** | Visuals, massive context, frontend | **2M** | Fast | $ |
| **Kimi k2 Thinking** | Cost-effective reasoning, open-source | 256K | Medium | ¢ |
| **Grok 4.1** | Personality, real-time data, vibe coding | 256K | Fast | $$ |

> 🆕 **GPT-5.2** (Dec 2025): 98.7% tool reliability, 128K output window, adaptive reasoning. [→ Full GPT-5.2 Guide](tips/gpt52-guide.md)

### Model Personalities

| Model | "Vibe" |
|-------|--------|
| GPT-5.2 | Reliable Workhorse |
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

### 🏆 GPT-5.2 vs Claude Opus 4.5 (The Model Wars)

After three weeks of field usage (Dec 2025), the consensus is clear:

| Benchmark | GPT-5.2 | Claude Opus 4.5 | Winner |
|-----------|---------|-----------------|:------:|
| **GDPval-AA** (Economic Value) | 🏆 Highest | - | GPT-5.2 |
| **SWE-Bench Verified** (Code) | 80.0% | **80.9%** | Claude |
| **Prompt Injection Resistance** | 21.9% success | **4.7%** success | Claude 🛡️ |
| **Rust Benchmark Runtime** | ~20 min | **~9 min** | Claude ⚡ |
| **Memory Usage** | **100GB** | 500GB | GPT-5.2 |

**Community Sentiment:**

> **Claude Opus 4.5**: "The Gold Standard" - Best for implementation
> **GPT-5.2**: "Fast but sterile" - Best for architecture/planning

**The Hybrid Workflow Winner:**

```
GPT-5.2 → Architecture & Planning (fast, cheap cached)
Claude Opus 4.5 → Implementation (accurate, secure)
```

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

## 🧬 GPT-5.1 Codex Guide

The deployment of GPT-5.1 Codex (December 2025) introduced new capabilities and quirks.

### The "Stupidity" Paradox

The new "smart" model sometimes behaves "stupidly" due to **over-reasoning** and safety alignment:

- Over-analyzes simple requests
- Hallucinated constraints (e.g., insisting VPS is required)
- Conservative refusals on legitimate security utilities

### Model Arbitrage Strategy

```
Complex Architecture Planning → GPT-5.1 Codex Max (expensive)
Implementation Details       → Gemini 3 Pro or Claude Sonnet (cheaper)
```

### Key Insight

**Use Composer over raw chat** — Cursor's agent harness improves model behavior significantly.

[→ Full GPT-5.1 Codex Guide](tips/gpt51-codex-guide.md)

---

## 🚀 GPT-5.2 Guide

Released December 11, 2025 — OpenAI's "Code Red" response to competitors.

### Key Specs

| Variant | Context | Output | Best For |
|---------|---------|--------|----------|
| GPT-5.2 Instant | 128K | 16K | Quick edits |
| GPT-5.2 Thinking | 200K | 32K | Complex reasoning |
| GPT-5.2 Pro | **400K** | **128K** | Massive refactors |

### Benchmarks

- **AIME 2025**: 100% (math reasoning)
- **SWE-Bench Pro**: 55.6% (can solve majority of mid-level tickets)
- **Tool Reliability**: 98.7%

### Pricing

| Tier | Input/1M | Output/1M |
|------|----------|-----------|
| Standard | $1.75 | $14.00 |
| **Cached** | **$0.175** | $14.00 |

**90% discount on cached inputs** — ideal for IDE usage.

[→ Full GPT-5.2 Guide](tips/gpt52-guide.md)

---

## 🎯 Confidence Scoring (Anti-Hallucination)

A powerful technique to combat AI hallucinations:

```
"Fix this only if you are 100% confident. Tell me your confidence score."
```

This prompt bypasses the "helpful assistant" persona and accesses the model's raw probability assessment, forcing it to:

1. Re-evaluate its own logic
2. Search for actual evidence
3. Admit uncertainty instead of hallucinating

[→ Full Confidence Scoring Guide](tips/confidence-scoring.md)

---

## 🐛 Known Bugs (Dec 2025)

### CRITICAL: "Plan" Disconnect Bug

UI shows diffs but files are never written to disk. **Always verify file timestamps after accepting changes.**

```bash
ls -la --time=modified <filename>
git diff
```

### Other Issues

| Bug | Severity | Status |
|-----|----------|--------|
| Plan mode not writing files | CRITICAL | Open |
| Todo List corruption | CRITICAL | Open |
| Ghost Process spawn (rg.exe) | HIGH | Fixed in v2.1.39 |
| Context Decay | MEDIUM | Workaround: Session Reset |
| PowerShell variable expansion | MEDIUM | Manual escaping required |

[→ Full Known Bugs Guide](tips/known-bugs-dec2025.md)

---

## 🌐 Google Antigravity

New competitor capturing "Early Adopter" mindshare with **parallel agents** and **free preview**.

| Feature | Cursor | Antigravity |
|---------|--------|-------------|
| Multi-Agent | Sequential | Parallel |
| Price | $20/mo | Free Preview |
| Verification | Git diffs | Artifacts |
| Best For | Precision | Rapid prototyping |

**The Hybrid Workflow**: Users exhaust Cursor credits, then switch to free Antigravity.

[→ Full Google Antigravity Guide](tips/google-antigravity.md)

---

## 📜 Advanced .cursorrules

Sophisticated patterns from production teams:

### The "Shout" Protocol
```
If existing code is altered, warn by shouting:
❗️SHOUT WITH LARGE LETTERS❗️
"WARNING: Modified existing function in file.ts"
```

### "Dumb" Component Enforcement
```
Presentation components must include "Dumb" in filename:
- UserProfileCardDumb.vue
- ProductListDumb.tsx
```

### Anti-Flake Testing
```
NEVER use page.waitForTimeout(5000)
ALWAYS use built-in auto-wait mechanisms
Target elements using data-testid attributes
```

[→ Full Advanced .cursorrules Guide](tips/advanced-cursorrules.md)

---

## 📋 Strategic Recommendations (Dec 2025)

Based on community intelligence:

1. **Enforce Confidence Scoring**: Mandate for all debugging tasks
2. **Adopt Interface Freeze**: Don't let AI define architecture
3. **Implement Rigid .cursorrules**: Copy "Dumb Component" and "Auto-Wait" patterns
4. **Monitor Plan Bug**: Verify file timestamps after every Accept
5. **Session Reset Cadence**: Clear context every 2-4 hours
6. **Commit Before Agent**: Always `git commit -m "checkpoint"`

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

## 📁 Configuration Files (Copy-Paste Ready)

Ready-to-use configuration files for optimal Cursor setup:

### MCP Configuration

```json
// .cursor/mcp.json
{
  "mcpServers": {
    "playwright": {
      "command": "npx",
      "args": ["-y", "@playwright/mcp@latest"]
    }
  }
}
```

### Defensive Settings

```json
// .cursor/settings.json
{
  "agents": {
    "auto_apply_changes": false,
    "legacy_terminal_tool": true,
    "max_turns_per_session": 40
  },
  "models": {
    "codebase_indexing": "gemini-3-pro"
  }
}
```

[→ Full MCP Config](configs/mcp.json) | [→ Full Settings](configs/settings.json) | [→ React Rules](configs/react-shipping.mdc) | [→ Test Rules](configs/testing.mdc)

---

## 🎭 Playwright Self-Healing Tests

Tests that automatically fix themselves when selectors change:

```
1. Agent runs E2E test
2. Test fails (selector changed)
3. Agent analyzes DOM
4. Agent updates selector
5. Test passes ✅
```

### Quick Setup

```bash
# 1. Create .cursor/mcp.json (see above)
# 2. Create .cursor/rules/testing.mdc
# 3. Enable auto-tools in settings
```

[→ Full Playwright MCP Guide](tips/playwright-mcp-setup.md)

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

**⭐ Star this repo if it helped you!**

Made with 💙 by [Murat Aslan](https://github.com/murataslan1)

*Last updated: January 1, 2026*

</div>

