# Troubleshooting Common Cursor Issues

> Solutions from Reddit and forum community.

---

## 1. "Connection Failed" Error

### Root Cause
NOT a network issue! Serialization bugs with large data packets.

### Fix
```
1. Open new chat (Cmd+L)
2. Settings → Disable HTTP/2
3. Keep conversations < 20 messages
```

---

## 2. "Stuck Generating" / Infinite Loading

### Root Cause
Model fails to send termination signal, context too large.

### Fix
```
1. Press Escape
2. Cmd+N for new Composer
3. Use "Single Purpose Composer" pattern - one task per window
```

---

## 3. Agent Deleting Files

### Root Cause
Agent decides recreate is easier than edit.

### Fix
```bash
# Always commit before Agent mode
git add -A && git commit -m "checkpoint"

# Use explicit instructions
"Edit src/UserService.ts - do NOT delete and recreate"
```

---

## 4. AI Ignoring .cursorrules

### Fix
```
1. File must be in project root
2. Restart Cursor after changes
3. Start new chat
4. Consider migrating to .mdc files
```

---

## 5. Slow/Stuck Indexing

### Fix
```
# Add .cursorignore
node_modules/
dist/
.git/
*.log
coverage/

# Reset index
Cmd+Shift+P → "Cursor: Clear Codebase Index"
```

---

## 6. High Token Usage

### Fix
```
1. Set spending limits in API dashboard
2. Use "Auto" model selection
3. Start fresh chats (old messages use tokens)
4. Use Kimi k2 for routine tasks
```

---

## 7. Diff View Empty

### Fix
```
1. Click away and back
2. Check git status for actual changes
3. Restart Cursor if persists
```

---

## 8. "Request Type Deprecated" Error (Windows)

### Root Cause
Cursor v2.1 attempting to contact legacy `api.cursor.sh` instead of new `api.cursor.com` infrastructure.

### Symptoms
```json
{ "error": "ERROR_DEPRECATED" }
```

### Fix
```
1. Clear Cursor cache:
   - Windows: %APPDATA%\Cursor\Cache
   - Mac: ~/Library/Caches/Cursor
2. Or reinstall Cursor completely
3. Verify you're on latest version (v2.1.39+)
```

---

## 9. "Ghost Process" Spawn (Instant Grep Bug)

### Root Cause
Initial Cursor 2.1 release spawned hundreds of `rg.exe` (ripgrep) processes for Instant Grep feature.

### Symptoms
- System freeze on MacOS/Windows
- High CPU usage
- Hundreds of `rg.exe` or `rg` processes in Task Manager

### Fix
```
1. Update to Cursor v2.1.39 or later (patched)
2. If stuck, force kill all rg processes:
   - Windows: taskkill /f /im rg.exe
   - Mac/Linux: pkill -9 rg
3. Restart Cursor
```

---

## 10. Custom Modes Removed (2.1 Breaking Change)

### Root Cause
Cursor 2.1 removed "Custom Modes" feature in favor of MDC system.

### Impact
Users who built workflows around QA Mode, Architect Mode, etc. must migrate.

### Migration Path
```
1. Create .cursor/rules/ directory
2. Convert each custom mode to an .mdc file
3. Use globs to scope rules to specific file types
4. Reference: tips/mdc-examples.md
```

---

## Quick Fixes

| Problem | Fix |
|---------|-----|
| Connection Failed | New chat |
| Stuck Generating | Cmd+N |
| Files Deleted | Checkpoint restore |
| Rules Ignored | Restart Cursor |
| Slow Indexing | Add .cursorignore |
| High Usage | Set limits |
| Request Deprecated | Clear cache, update to v2.1.39+ |
| Ghost Processes | Update to v2.1.39+, kill rg processes |
| Custom Modes Gone | Migrate to .mdc files |

---

## Prevention Checklist

- [ ] Commit before Agent mode
- [ ] Keep chats < 20 messages
- [ ] Use .cursorignore
- [ ] Set API spending limits
- [ ] Restart after config changes
