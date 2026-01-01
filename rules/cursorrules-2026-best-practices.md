# 📏 2026 Best Practices for `.cursorrules`

**Updated: January 1, 2026**

With the release of Cursor 2.3 and new models like Claude Opus 4.5, your `.cursorrules` need an update.

## 🧠 Meta-Rules (The "Vibe" Shift)

Old rules focused on "don't be lazy". New models aren't lazy, but they can be **over-verbose** or **insecure**.

### 1. The "Concise Architect" Rule
Prevents Opus 4.5 from explaining every single import.

```markdown
# General Behavior
- Be concise. Do not explain standard code patterns unless asked.
- Focus on the "Unique Value" of the change.
- If a solution is effectively identical to the existing code, say "No changes needed".
```

### 2. The "Security First" Rule (Critical for Agents)
Agents now run recursively. You must bound them.

```markdown
# Agent Boundaries
- NEVER commit code without user review strings.
- NEVER delete config files (.env, package.json) without explicit confirmation.
- If you find a security vulnerability, STOP and report it immediately.
```

---

## 🛠️ Fixes for Common 2025 Bugs

### The "Zombie Revert" Fix
Cursor 2.2/2.3 sometimes "hangs" on apply. Add this context to help the model avoid complex diffs.

```markdown
# Diffs & Applies
- When editing large files (>500 lines), prefer `SEARCH/REPLACE` blocks over full file rewrites.
- If you are unsure where a block belongs, ask for the surrounding context first.
```

### Windows/WSL Terminal Fix
If you are on Windows, force the legacy terminal tool to avoid PID -1 errors.

```markdown
# Environment
- Use "Legacy Terminal Tool" behavior: simple command execution, no complex TTY manipulation.
```

---

## ⚡ Framework-Specific Snippets (2026 Editions)

### Next.js 15+ (App Router Strict Mode)

```markdown
# Next.js
- Default to `server components` unless `useState` or `useEffect` is strictly required.
- Use `await params` in dynamic routes (Next.js 15 change).
- Prefer `typed-routes` for all navigation.
```

### Python / FastAPI (Async First)

```markdown
# Python
- All IO-bound operations MUST be `async def`.
- Use Pydantic V2 `model_validate` patterns.
- Type hints are mandatory (Python 3.12+ syntax).
```
