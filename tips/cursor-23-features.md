# 🆕 Cursor 2.3 Features (December 22, 2025)

> **"Stability and polish" release** - Cursor 2.3 focuses on architectural improvements and developer ergonomics, preparing for the holiday code freeze.

## Process Separation (The Big Change)

Cursor 2.3 introduces **process separation** between user extensions and core Cursor functionality. This is the most significant architectural change since 2.0.

### What This Means

```
Before 2.3:
├── Extensions + AI Core = Same Process
├── One bad extension → Crashes everything
└── LSP, indexing, agent share memory

After 2.3:
├── Extensions = Isolated Process
├── AI Core = Protected Process
├── Extension crash → AI keeps working
└── Enterprise stability guaranteed
```

### Why It Matters

- **No more "noisy neighbor" problem** - Unoptimized extensions can't crash the AI
- **Background indexing protected** - Vector embeddings survive extension failures
- **Agent resilience** - Reasoning engine runs even if syntax highlighter dies
- **Enterprise-ready** - Critical for large monolithic codebases

---

## Layout Customization Engine

Four preset layout modes with `⌘+⌥+⇥` (Mac) or `Ctrl+Alt+Tab` (Windows):

| Layout Mode | Primary Function | Screen Allocation | Best For |
|:------------|:-----------------|:------------------|:---------|
| **Agent** | Interactive Development | 50/50 Chat + Editor | Pair programming with AI |
| **Editor** | Traditional Coding | Maximized editor | Deep focus, code review |
| **Zen** | Distraction-free | Hidden chrome, no sidebars | Complex algorithms |
| **Browser** | Web Integration | Split with Chromium renderer | Frontend development |

### Key Improvements

- **Layout persistence** - Survives version upgrades
- **Vertical monitor support** - Better (still not perfect, but improved)
- **Quick toggle** - `⌘+⌥+⇥` cycles through modes

> 💡 **Pro Tip**: Agent Mode is no longer an "auxiliary feature" - it's now a first-class workspace state equal to the text editor.

---

## Integrated Browser Improvements

### Multi-Tab Support

- Multiple browser tabs within IDE
- Tab sync with editor state
- Screenshots directly to AI context

### Vibe Coding Loop

```
Plan → Code → Render → Verify
        ↓
   All inside Cursor
        ↓
   No Alt-Tab required
```

This enables the complete "vibe coding" workflow without leaving the IDE.

---

## Enterprise Features (2.3)

### Service Accounts (Non-Human Identities)

> 🏢 **Enterprise Plan Only**

Service Accounts enable "Headless Cursor" - AI agents that work without human presence:

| Capability | Description |
|:-----------|:------------|
| **API Invocation** | Trigger code generation without IDE |
| **CI/CD Integration** | Review PRs, fix linting, generate docs |
| **Credential Rotation** | Survives employee turnover |

### SOC 2 Certification

Cursor has achieved **SOC 2 certification** - mandatory for enterprise adoption.

### New Security Controls

```
Enforcement Hooks:
├── Pre-prompt hooks → Run before AI sees prompt
├── Pre-write hooks → Run before code writes to disk
├── Block sensitive data exfiltration
└── Prevent dangerous terminal commands (rm -rf /)

Linux Sandboxing:
├── Agent sandbox extended to Linux
├── Containerized environment support
└── Enterprise deployment ready
```

---

## ⚠️ Known Issue: RCE Vulnerability (Patched)

A remote code execution vulnerability was discovered in "Workspace Trust":

```
Vulnerability: Malicious repos could execute code via tasks.json
Status: PATCHED in 2.3
Action: Ensure you're on 2.3+
```

**Best Practice**: Always review `tasks.json` and `.vscode/` contents before opening untrusted repos.

---

## Migration from 2.2

### What's Fixed

- [x] Extension crashes no longer freeze AI
- [x] Layout persistence across updates
- [x] Browser tab stability

### What's Still Broken

- [ ] Zombie Revert bug (see [Zombie Revert Guide](zombie-revert-bug.md))
- [ ] Windows terminal QC injection (see [Windows Terminal Fixes](windows-terminal-fixes.md))

---

## Quick Setup

After updating to 2.3:

1. **Test Process Separation**
   ```bash
   # Install a known-unstable extension
   # Use AI while extension is active
   # AI should continue working if extension crashes
   ```

2. **Try Layout Modes**
   ```
   ⌘+⌥+⇥ → Agent Mode
   ⌘+⌥+⇥ → Editor Mode
   ⌘+⌥+⇥ → Zen Mode
   ⌘+⌥+⇥ → Browser Mode
   ```

3. **Configure Enterprise Security** (if applicable)
   ```json
   // .cursor/settings.json
   {
     "security": {
       "enforcement_hooks": true,
       "sandbox_mode": "strict"
     }
   }
   ```

---

## Related Guides

- [Zombie Revert Bug](zombie-revert-bug.md) - Critical data loss issue
- [Windows Terminal Fixes](windows-terminal-fixes.md) - QC injection bug
- [Enterprise Features](enterprise-features.md) - Full enterprise guide

---

*Last updated: December 24, 2025*
