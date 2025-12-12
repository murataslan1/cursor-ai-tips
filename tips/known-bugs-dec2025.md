# Known Bugs & Issues (December 2025)

A forensic analysis of critical bugs affecting Cursor during the agentic transition period.

> ⚠️ **Cursor 2.2 Alert**: Version 2.2 introduced several critical bugs. See [Cursor 2.2 Bugs](./cursor-22-bugs.md) for detailed workarounds.

---

## Bug Severity Taxonomy

| Severity | Issue Type | Description | Frequency |
|----------|-----------|-------------|-----------|
| **CRITICAL** | Revert/Undo Broken (2.2) | Revert fails, causing data loss | High |
| **CRITICAL** | State Desynchronization | "Plan" mode shows diffs but fails to write to disk | High |
| **CRITICAL** | Data Corruption | Todo List resets or duplicates after edits | Moderate |
| **HIGH** | Visual Editor Loop (2.2) | Changes re-apply infinitely | Moderate |
| **HIGH** | WSL Terminal Failure (2.2) | Agent can't spawn shell in WSL | High (Windows) |
| **HIGH** | Hallucination | Model invents routing constraints or system requirements | High |
| **MEDIUM** | AWS Region Reset (2.2) | Bedrock region reverts to US | Moderate |
| **MEDIUM** | Context Decay | Model forgets file structure in long sessions | High |
| **MEDIUM** | Integration Failure | MCP server fails to resolve relative paths | Moderate |
| **LOW** | UI Regression | Sidebar cannot be reorganized; terminal window blank | Low |

---

## CRITICAL: The "Plan" Disconnect Bug

**The most alarming anomaly detected in December 2025.**

### Symptoms

1. User engages Composer (Plan) mode to refactor multiple files
2. UI displays loading animations
3. Diff view shows expected changes (green additions, red deletions)
4. User clicks "Accept"
5. **File system is NEVER updated**

### Impact

- Changes exist only in chat's transient memory
- Forces manual verification of every file
- Negates efficiency gains of AI assistance
- **Catastrophic failure of tool integrity**

### Root Cause (Suspected)

Race condition or corruption in the local state management database that handles synchronization between the AI agent's "mind" and the physical disk. High resource load of GPT-5.1 may cause timeouts in file-write confirmation callbacks.

### Workaround

```bash
# After EVERY "Apply" action in Composer:
ls -la --time=modified <filename>

# Or use git to verify
git diff
```

**Always verify file modification timestamps after accepting changes.**

### Reference

- [GitHub Issue #3844](https://github.com/cursor/cursor/issues/3844)

---

## CRITICAL: Todo List Corruption

### Symptoms

- Todo entries duplicate unexpectedly
- List reverts to previous state after edits
- Entries disappear and reappear

### Correlation

Often paired with the "Plan" Disconnect bug, suggesting a shared database corruption issue.

### Workaround

- Export/backup todo lists manually
- Use external task tracking for critical items

---

## HIGH: Context Decay

### The Problem

After a certain token threshold, the AI's understanding of project structure **degrades into "confident lying"**.

### Mechanism

As the context window fills:
1. Model truncates older instructions/file references
2. Unlike humans, model doesn't know it has "forgotten"
3. Attempts to "complete the pattern" from fragments
4. Results in hallucinations

### The "Session Reset" Cadence

Users must manually clear chat context and re-inject Codebase Context every few hours:

```
Recommended Cadence:
├── Light work: Reset every 4-6 hours
├── Heavy refactoring: Reset every 2-3 hours
├── Debugging sessions: Reset after ~20 messages
└── Always reset if AI starts "confident lying"
```

### Signs You Need to Reset

- AI claims files exist that don't
- AI forgets recent changes you discussed
- AI contradicts earlier statements confidently
- AI hallucinates function signatures

---

## MEDIUM: PowerShell Variable Expansion Bug (Windows)

### The Issue

When AI generates GitHub CLI commands containing PowerShell variables:

```powershell
# AI generates:
gh issue create --body "Check $env:Path"

# PowerShell expands BEFORE execution:
gh issue create --body "Check C:\Windows\system32;C:\Windows;..."
```

The literal PATH string ends up in the issue body instead of the variable reference.

### The Fix

Manually escape variables:

```powershell
# Correct:
gh issue create --body "Check `$env:Path"

# Or use single quotes:
gh issue create --body 'Check $env:Path'
```

### Reference

- [GitHub Issue #3831](https://github.com/cursor/cursor/issues/3831)

---

## MEDIUM: MCP Path Resolution Failures

### Symptoms

MCP server fails to connect with "Red Light" errors.

### Cause

Relative paths in `playwright.config.ts` fail to resolve when MCP server launches from different working directory.

### Workaround

Create a wrapper script:

```bash
#!/bin/bash
# playwright-test-wrapper.sh
cd "$(dirname "$0")"
npx playwright test "$@"
```

Update `mcp.json` to use the wrapper:

```json
{
  "mcpServers": {
    "playwright": {
      "command": "./playwright-test-wrapper.sh"
    }
  }
}
```

### Reference

- [GitHub Issue #37739](https://github.com/microsoft/playwright/issues/37739)

---

## Best Practices to Mitigate Bugs

### Before Every Session

```bash
git add -A && git commit -m "checkpoint before AI session"
```

### During Sessions

1. **Verify file writes** — check timestamps after Accept
2. **Reset context regularly** — don't let sessions run too long
3. **Use Confidence Scoring** — force AI to verify before acting
4. **External task tracking** — don't rely solely on built-in Todo

### After Sessions

```bash
git diff  # Review all changes
git status  # Ensure nothing unexpected
```

---

## Reporting New Bugs

When reporting bugs to Cursor:

1. Include Cursor version
2. Include OS and version
3. Provide steps to reproduce
4. Attach relevant logs from `~/.cursor/logs/`
5. Note which model was being used
