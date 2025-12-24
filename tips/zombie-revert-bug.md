# 🚨 CRITICAL: Zombie Revert Bug

> ⚠️ **DATA LOSS WARNING**: This bug can silently delete your code. Read carefully.

## What Is The Zombie Revert Bug?

A critical bug affecting Cursor 2.2.36 through current versions where the IDE **randomly reverts code without user consent**.

### Symptoms

| Symptom | Description |
|:--------|:------------|
| **Silent Data Loss** | Changes disappear minutes after successful `Ctrl+S` |
| **Selective Revert** | Some edits in a file revert, others stay → "Frankenstein" files |
| **Multi-File Corruption** | Entire files reduced to near-empty states |
| **Cross-File Cascade** | Reverting File A corrupts unrelated Files B and C |

---

## Root Cause: Multi-Agent Concurrency

The bug is strongly correlated with:

1. **"Auto" mode** for model selection
2. **Multiple simultaneous agents** running on different files
3. **Agent interruption** (canceling mid-operation)

### Technical Explanation

```
When multiple agents run in parallel:

Agent 1: Refactoring backend/api.ts
Agent 2: Updating frontend/auth.tsx
                    ↓
Both write to "Shadow Workspace"
                    ↓
Race condition in state management
                    ↓
If Agent 1 is interrupted:
  → System "rolls back" to pre-agent checkpoint
  → Agent 2's work is also lost
  → Files may be partially corrupted
```

### Confirmed by Cursor Team

> *"This is a known issue linked to agent interruption and multi-agent scenarios."*
> — Dean Rie, Cursor Support (December 21, 2025)

Bug persists in 2.3.0-pre.9, indicating the 2.3 process separation did NOT fix the core synchronization logic.

---

## 🎄 Holiday Freeze Protocol (Dec 24 - Jan 2)

Given the timing and severity, implement this defensive workflow:

### ❌ DO NOT

- Update Cursor to 2.2.36 or higher
- Run multi-file refactoring (20+ files)
- Use "Auto" model selection
- Run multiple agents simultaneously
- Trust the "Revert" button

### ✅ SAFE TO DO

- Single-file edits
- Test writing (with Playwright MCP)
- Planning with Gemini 3 Pro
- Sequential agent operations (one at a time)

---

## Defensive Workflows

### 1. Strict Serial Execution

```
Rule: ONE agent at a time. Period.

Before each agent session:
1. Wait for previous agent to complete
2. Verify file state with git diff
3. Commit if changes are good
4. THEN start next agent
```

### 2. Defensive Committing (Critical)

```bash
# BEFORE every agent operation:
git add -A && git commit -m "pre-agent-$(date +%s)"

# AFTER successful agent operation:
git add -A && git commit -m "post-agent-feature-name"
```

### 3. Timeline Forensics (Recovery)

If you lose code, use VS Code's Timeline feature (NOT Cursor's history):

```
1. Open file in Cursor
2. Right-click in editor
3. Select "Open Timeline" (VS Code's native feature)
4. Browse file states from past minutes/hours
5. Copy lost code manually
```

### 4. Disable Auto-Apply

```
Settings → Agent → "Always Apply Agent Changes" = OFF
```

This forces you to review every change before application.

---

## Emergency Recovery

### If You Lost Code

```bash
# Option 1: Git reflog (if you committed recently)
git reflog
git checkout HEAD@{n} -- path/to/file.ts

# Option 2: VS Code Timeline
# Right-click file → Open Timeline → Select earlier state

# Option 3: Backup restoration
# If you made .zip backup, restore from there
```

### Backup Strategy

```bash
# Create timestamped backup BEFORE holiday period:
cd your-project
zip -r "../backup-$(date +%Y%m%d).zip" .

# Push all branches to remote:
git push origin --all
git push origin --tags
```

---

## Configure for Safety

### `.cursor/settings.json`

```json
{
  "agents": {
    "auto_apply_changes": false,
    "max_turns_per_session": 40,
    "legacy_terminal_tool": true
  },
  "editor": {
    "auto_fix_lint": false,
    "disable_auto_checkpoint": true
  }
}
```

### Why These Settings

| Setting | Reason |
|:--------|:-------|
| `auto_apply_changes: false` | Forces review before write |
| `max_turns_per_session: 40` | Prevents context decay |
| `legacy_terminal_tool: true` | Avoids terminal bugs |
| `auto_fix_lint: false` | Prevents Prettier infinite loop |
| `disable_auto_checkpoint: true` | Reduces state management conflicts |

---

## When Will This Be Fixed?

**Unknown.** Cursor 2.3 (Dec 22) was marketed as "stability focused" but did not fix this bug.

### Expected Timeline

- **2.3.x hotfixes**: Possible but unconfirmed
- **Safe to trust again**: When changelog explicitly mentions "Shadow Workspace state synchronization fix"

### How to Know It's Fixed

Look for changelog entries mentioning:
- "Revert reliability"
- "Multi-agent state management"
- "Shadow Workspace sync"
- "File system consistency"

Until then, **treat every agent operation as potentially destructive**.

---

## Related Guides

- [Cursor 2.3 Features](cursor-23-features.md) - What IS working in 2.3
- [Windows Terminal Fixes](windows-terminal-fixes.md) - Terminal-specific bugs
- [Troubleshooting](troubleshooting.md) - General recovery tips

---

*Last updated: December 24, 2025*
