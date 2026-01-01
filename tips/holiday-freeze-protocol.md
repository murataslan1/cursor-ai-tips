# 🚨 Holiday Freeze Protocol (Dec 24 - Jan 7, 2026)

> **Status**: 🔴 **ACTIVE** - Do NOT update past v2.2.35

The developer community declared a "Holiday Freeze" starting December 24, 2025, after reports that Cursor 2.3's Process Separation architecture caused critical data loss issues.

---

## 📊 Protocol Directives

| Status | Action/Feature | Risk/Reason |
|:------:|----------------|-------------|
| ❌ **PROHIBITED** | Update to 2.3.x | High risk of "Zombie Revert" (Data Loss) |
| ❌ **PROHIBITED** | Multi-file Agent Refactors | Triggers race conditions between Agent and Disk |
| ❌ **PROHIBITED** | "Auto" Model Selection | May default to inferior models; no deterministic control |
| ❌ **PROHIBITED** | Trust "Revert" Button | UI button may be desynced from git history |
| ⚠️ **CAUTION** | Visual Editor | Prone to infinite "Apply/Undo" loops in 2.3.9 |
| ✅ **PERMITTED** | Single-file Edits | Lower risk of context desynchronization |
| ✅ **PERMITTED** | Planning Mode | Safe text generation; doesn't touch file I/O |
| ✅ **PERMITTED** | Sequential Agents | Running one agent at a time avoids race conditions |
| ✅ **RECOMMENDED** | Playwright MCP | Safe, isolated workflow for testing ("PlayWhite" Meta) |

---

## 🎯 Why This Protocol Exists

### The "Split-Brain" Scenario

Cursor 2.3 moved extensions into a separate process (Process Separation). While intended to improve stability, this created a synchronization failure:

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   Editor    │ ←?→ │  Composer   │ ←?→ │ File System │
│  (Process 1)│     │  (Process 2)│     │   (Disk)    │
└─────────────┘     └─────────────┘     └─────────────┘
                          ↓
              Desync = "Zombie Revert" Bug
```

When you manually edit a file while the agent is "thinking," the Agent's shadow buffer fails to receive the update signal. The Agent then overwrites your manual work with its stale cached version.

---

## 🛡️ Defensive Measures

### 1. Version Lock

```bash
# Check current version
cursor --version

# If on 2.3.x, consider downgrade (if possible)
# Lock at v2.2.35 until 2.4 release
```

### 2. Pre-Agent Commit (CRITICAL)

```bash
# BEFORE every agent operation:
git add -A && git commit -m "pre-agent-$(date +%s)"
```

### 3. Enable Local History

```
Settings → Editor → Files → Auto Save: afterDelay
Settings → Editor → Files → Auto Save Delay: 1000
```

### 4. Enable Legacy Terminal (Windows/WSL)

```
Settings → Features → Agent → Enable "Legacy Terminal Tool"
```

This fixes the PID -1 terminal paralysis bug in 2.3.

---

## 📅 Expected Resolution

| Date | Event |
|------|-------|
| Dec 22, 2025 | Cursor 2.3 released |
| Dec 28, 2025 | Patch 2.3.9 (partial fix) |
| Jan 6-10, 2026 | Expected "all clear" from community |
| Q1 2026 | Cursor 2.4 expected (full fix) |

---

## 🔗 Related Resources

- [Zombie Revert Bug Details](zombie-revert-bug.md)
- [Windows Terminal Fixes](windows-terminal-fixes.md)
- [Cursor 2.3 Features](cursor-23-features.md)

---

## Sources

- [Cursor Forum: Cursor 2.3 Discussion](https://forum.cursor.com/t/cursor-2-3-is-here/147076)
- [Reddit: Cursor reverted two weeks of development](https://www.reddit.com/r/cursor/comments/1pjktkx/cursor_just_reverted_two_weeks_worth_of/)
- Community intelligence from r/cursor power users
