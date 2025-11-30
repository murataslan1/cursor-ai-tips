<div align="center">

# Cursor AI Tips & Tricks

[![Awesome](https://awesome.re/badge-flat2.svg)](https://awesome.re)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg?style=flat-square)](http://makeapullrequest.com)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

**The ultimate guide to mastering Cursor AI IDE**

*Keyboard shortcuts, Composer workflows, .cursorrules examples, and Reddit community wisdom*

[Shortcuts](#-keyboard-shortcuts) • [Composer](#-composer-mode) • [Context](#-context-management) • [Rules](#-cursorrules) • [Models](#-model-selection) • [Reddit Tips](#-reddit-community-wisdom)

</div>

---

## Why Cursor?

Cursor is not just VS Code with AI - it's a **fork** that integrates LLMs directly into the rendering pipeline. This enables:

- **Cursor Tab**: Multi-line predictions (not just single line like Copilot)
- **Composer**: Autonomous multi-file editing agent
- **Shadow Workspace**: Background indexing for semantic search
- **Native Diffs**: Inline green/red diff visualization

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

[→ Full Shortcuts Guide](tips/keyboard-shortcuts.md)

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

[→ Full Context Guide](tips/context-management.md)

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

[→ Full .cursorrules Guide](tips/cursorrules-guide.md)

---

## 🧠 Model Selection

### Available Models

| Model | Best For | Speed | Cost |
|-------|----------|-------|------|
| **Claude 3.5 Sonnet** | Complex reasoning, architecture | Medium | $$ |
| **GPT-4o** | Simple logic, text manipulation | Fast | $$ |
| **o1-preview** | Novel algorithms, hard bugs | Slow | $$$ |
| **Cursor-small** | Inline edits (Cmd+K) | Very Fast | $ |

### Cost Optimization Strategy

```
Pro Plan ($20/mo):
├── 500 fast requests (premium models)
└── Unlimited slow requests (deprioritized)

Recommendation:
├── Daily work → Pro Plan
├── Heavy refactoring → BYOK API key
└── Hard bugs → o1-preview (BYOK)
```

> **Warning**: Don't switch models mid-conversation. It breaks the "train of thought."

[→ Full Model Guide](tips/model-selection.md)

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

## ⚔️ Cursor vs Competitors

| Feature | Cursor | Windsurf | GitHub Copilot |
|---------|--------|----------|----------------|
| **Architecture** | Fork (Native AI) | Fork (Native AI) | Extension |
| **Multi-File Edits** | Composer ⭐ | Cascade | Limited |
| **Model Choice** | Claude/GPT/All | Proprietary | OpenAI only |
| **Context** | Manual @ + Index | Auto-context | Open files only |
| **Price** | $20/mo | $15/mo | $10/mo |

**Verdict**: Cursor wins on Composer and model flexibility. Windsurf wins on auto-context. Copilot is cheapest but limited.

[→ Full Comparison](tips/vs-competitors.md)

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

*Last updated: November 2025*

</div>
