# Cursor 2.2 Critical Bugs & Workarounds

[← Back to Main](../README.md)

> ⚠️ **WARNING**: Cursor 2.2 has critical bugs causing **data loss**. Read before using.

---

## Bug Summary (Dec 12, 2025)

| Bug | Severity | Workaround |
|-----|----------|------------|
| Revert/Undo Broken | **CRITICAL** | Git + Timeline |
| Visual Editor Loop | **HIGH** | Window Reload |
| WSL Terminal Failure | **HIGH** | Legacy Terminal |
| AWS Region Reset | **MEDIUM** | Manual Config |

---

## CRITICAL: Revert Bug (Data Loss)

### Symptoms

- "Revert" option disappears after Composer changes
- "Checkout" reverts to hours/days ago, losing recent work
- Shadow Git loses track of previous state

### Workarounds

#### 1. Git Discipline (MANDATORY)

```bash
# BEFORE every AI agent invocation:
git add -A && git commit -m "checkpoint"
```

#### 2. Timeline Recovery

If Revert fails:
1. Right-click file → "Open Timeline"
2. Find state before AI changes
3. Click to restore

#### 3. Verify File Writes

```bash
# After EVERY "Accept":
ls -la --time=modified <filename>
git diff
```

---

## HIGH: Visual Editor Infinite Loop

### Symptoms

- Apply Change A
- Apply Change B → system re-applies A + B
- Changes accumulate with each edit

### Workaround

Immediately reload window:
```
Cmd/Ctrl + Shift + P → "Reload Window"
```

---

## HIGH: WSL Terminal Failure

### Symptoms

- Agent commands fail silently (PID: -1)
- Terminal hangs
- Agent cannot run tests/builds

### Workaround

Enable Legacy Terminal:
1. `Settings → Agents → Inline Editing`
2. Toggle "Legacy Terminal Tool" → **ON**
3. `Ctrl+Shift+P → "Terminal: Kill All Terminals"`
4. Restart Cursor

---

## MEDIUM: AWS Region Reset

### Symptoms

- Region reverts from `eu-west-3` to `us-east-2`
- GDPR compliance issue for EU users

### Workaround

Manually verify region before **every** sensitive prompt.

---

## Mitigation Protocol

### Before Session
```bash
git add -A && git commit -m "session-start"
```

### During Session
- Verify file timestamps after Accept
- Reload window if Visual Editor loops
- Use Legacy Terminal on WSL

### After Session
```bash
git diff && git status
```

---

## Downgrade Option

Stable versions:
- **Cursor 2.1.x** — Last stable
- **Cursor 2.0.77** — Community recommended

---

## The Trust Battery

> "Until bugs are fixed, use defensive practices: commit frequently, verify every AI action."

---

[← Back to Main](../README.md)
