# 🪟 Windows Terminal Fixes

> ⚠️ **Windows Users**: Critical terminal bugs affect agent functionality. Apply these fixes.

## Known Issues

### 1. "QC" Character Injection

**Symptom**: Terminal prepends random characters (`q`, `qc`, `c`) to commands:

```
Expected: npm install
Actual:   qnpm install
Result:   Command not found
```

### 2. Null Output Bug

**Symptom**: Commands execute successfully (Exit Code 0) but return no output to the agent:

```
Agent: Runs "npm test"
Terminal: Tests pass (visible to human)
Agent Context: [empty]
Result: Agent thinks nothing happened
```

---

## Root Cause

Both issues stem from conflicts between:
- Cursor's new terminal interception layer
- Windows Pseudo Console (ConPTY) API
- PowerShell/CMD host signal handling

The "qc" characters are likely malformed ANSI escape sequences or misinterpreted control signals.

---

## Fix 1: Enable Legacy Terminal Tool

The most reliable fix:

```
Settings → Features → "Legacy Terminal Tool" → ON
```

Or in settings JSON:

```json
// .cursor/settings.json
{
  "agents": {
    "legacy_terminal_tool": true
  }
}
```

**Note**: This reverts to the older terminal implementation, which is more compatible with Windows.

---

## Fix 2: Use PowerShell 7+ Instead of Windows PowerShell

Windows PowerShell 5.x has known ConPTY issues. PowerShell 7+ is more stable:

### Install PowerShell 7

```powershell
winget install Microsoft.PowerShell
```

### Set as Default in Cursor

```json
// .cursor/settings.json OR VS Code settings
{
  "terminal.integrated.defaultProfile.windows": "PowerShell",
  "terminal.integrated.profiles.windows": {
    "PowerShell": {
      "source": "PowerShell",
      "path": "C:\\Program Files\\PowerShell\\7\\pwsh.exe"
    }
  }
}
```

---

## Fix 3: Disable Oh My Posh / Custom Prompts

Custom PowerShell prompts (Oh My Posh, Starship) can interfere with output capture:

### Temporary Disable

```powershell
# In PowerShell profile, comment out:
# oh-my-posh init pwsh | Invoke-Expression
```

### Test Without Profile

```powershell
pwsh -NoProfile
```

If agent works without profile, the prompt customization is the issue.

---

## Fix 4: Use CMD Instead of PowerShell

If PowerShell issues persist, try CMD:

```json
{
  "terminal.integrated.defaultProfile.windows": "Command Prompt"
}
```

This avoids PowerShell-specific ConPTY issues entirely.

---

## Workaround: Manual Terminal Execution

If agent terminal is completely broken:

1. **Disable Terminal Auto-Execution**
   ```
   Settings → Agent → "Auto-run terminal commands" → OFF
   ```

2. **Copy Commands Manually**
   - Agent generates command
   - You copy and run in external terminal
   - Paste output back to agent

Not ideal, but functional.

---

## WSL-Specific Issues

### Agent Can't Run WSL Commands

**Symptom**: Agent terminal doesn't work in WSL distribution.

**Fix**: Enable Legacy Terminal + use WSL explicitly:

```json
{
  "terminal.integrated.defaultProfile.windows": "Ubuntu (WSL)",
  "agents": {
    "legacy_terminal_tool": true
  }
}
```

### Path Translation Errors

**Symptom**: Agent uses Windows paths in WSL context.

**Fix**: Add to your `.mdc` rules:

```markdown
---
description: "WSL Path Rules"
globs: ["**/*"]
---

When running terminal commands:
- Use Linux-style paths: `/home/user/project`
- NOT Windows paths: `C:\Users\...`
- Translate paths before execution
```

---

## Diagnostic Commands

### Check Terminal Type

```powershell
$PSVersionTable
```

Expected for PowerShell 7:
```
PSVersion                      7.x.x
```

### Test Agent Terminal

Ask agent to run:
```
"Run 'echo hello world' and show me the output"
```

If output is captured correctly, basic terminal works.

### Check ConPTY

```powershell
Get-WinEvent -LogName "Microsoft-Windows-Terminal-Services-RemoteConnectionManager/Operational" | Select-Object -First 5
```

---

## Configuration Checklist

```
[ ] Legacy Terminal Tool = ON
[ ] PowerShell 7 installed and set as default
[ ] Oh My Posh / Starship disabled (or tested)
[ ] WSL paths configured (if using WSL)
[ ] Terminal profiles in settings.json configured
```

---

## Still Not Working?

If all fixes fail:

1. **Report to Cursor Forum** - Include Windows version, PowerShell version, error screenshots
2. **Use Browser-Based Cursor** - web.cursor.com may have fewer terminal issues
3. **Dual Boot / VM** - macOS Cursor has fewer terminal bugs

---

## Related Guides

- [Zombie Revert Bug](zombie-revert-bug.md) - Other critical bugs
- [Troubleshooting](troubleshooting.md) - General fixes
- [Cursor 2.3 Features](cursor-23-features.md) - Platform stability

---

*Last updated: December 24, 2025*
