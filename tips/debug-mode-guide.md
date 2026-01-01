# 🔍 Debug Mode Guide (Cursor 2.2+)

> **Success Rate**: ~70% first-attempt fix | **ROI**: 60% debugging time reduction

Debug Mode is Cursor 2.2's breakthrough feature that transforms debugging from guesswork into a scientific process. The agent instruments your code, you reproduce the bug, and the agent analyzes real runtime data.

---

## 🎯 The Paradigm Shift

### Traditional AI Debugging (Static Analysis)

```
You: "The login is broken"
AI: *reads code* → *guesses* → "Maybe try this?"
Result: 50/50 chance of working
```

### Debug Mode (Runtime Analysis)

```
You: "The login is broken"
AI: *generates hypotheses* → *adds logging* → *you trigger bug*
AI: *analyzes real logs* → "Root cause found. Fixing..."
Result: ~70% first-attempt fix
```

---

## 🔄 The Debug Mode Workflow

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
│     Targeted fix based on empirical data, not guessing  │
└─────────────────────────────────────────────────────────┘
```

---

## 🛠️ How to Use Debug Mode

### Step 1: Enable Debug Mode

```
1. Open Cursor Chat (Cmd + L)
2. Click the "Debug" icon in the toolbar
   OR type "/debug" in chat
```

### Step 2: Describe the Bug

```
The user registration form submits successfully but 
the user doesn't appear in the database. No error is shown.
```

### Step 3: Let Agent Instrument

The agent will:
1. Identify relevant files
2. Insert console.log/logger statements
3. Add timing instrumentation
4. Track variable states

Example instrumentation added:
```javascript
// Agent adds this automatically:
console.log('[DEBUG] registerUser called with:', userData);
console.log('[DEBUG] DB connection status:', db.isConnected);
console.log('[DEBUG] Insert result:', result);
```

### Step 4: Reproduce the Bug

```
Agent: "I've added logging. Please trigger the bug now."

You: *submit registration form*
```

### Step 5: Review and Accept Fix

Agent analyzes logs and proposes targeted fix:

```
Root Cause: The promise wasn't being awaited.
Line 47: db.insert(user) should be await db.insert(user)

[Apply Fix] [Reject] [Explain More]
```

---

## 💬 Effective Debug Prompts

### ✅ Good Debug Prompts

```
"The checkout button is unresponsive on Safari. 
It works on Chrome. No console errors."
```

```
"API returns 500 error on /api/users endpoint.
The error started after yesterday's deploy.
@error.log is attached."
```

```
"Memory usage grows indefinitely when uploading files.
After 5 uploads, the page becomes unresponsive.
@UploadService.ts is the main file."
```

### ❌ Poor Debug Prompts

```
"It's broken" ← Too vague
"Fix the bugs" ← No specificity
"Error happened" ← Need context
```

---

## 📊 Debug Mode Statistics

Based on community reports (December 2025):

| Metric | Value |
|--------|-------|
| **First-attempt fix rate** | ~70% |
| **Time saved vs manual debug** | 60% |
| **Average hypothesis count** | 3-5 |
| **Instrumentation accuracy** | 85%+ |

---

## 🎯 Best Use Cases

### ✅ Debug Mode Excels At

| Bug Type | Why It Works |
|----------|--------------|
| **Race conditions** | Timing instrumentation reveals ordering |
| **State bugs** | Variable tracking shows mutations |
| **Async issues** | Promise/callback flow visualization |
| **Integration bugs** | Cross-service logging |
| **Intermittent bugs** | Captures conditions when bug occurs |

### ⚠️ Debug Mode Struggles With

| Bug Type | Why |
|----------|-----|
| **Build/compile errors** | No runtime to analyze |
| **Environment issues** | Can't instrument external systems |
| **Security vulnerabilities** | Needs specialized analysis |
| **Performance regression** | Better tools exist (profilers) |

---

## 🔧 Advanced Usage

### Custom Instrumentation Rules

Add to `.cursorrules`:

```
## Debug Mode Configuration

When debugging:
1. Always log function entry/exit with timing
2. Log all external API calls with payload
3. Track state mutations with before/after values
4. Use structured logging format (JSON)
5. Remove instrumentation after fix is confirmed
```

### Combine with Playwright MCP

```
@playwright
Navigate to the registration page.
Fill the form with test data.
Submit and capture network log.

@chat
Based on the network log, why is the user not being created?
Use Debug Mode to instrument the backend.
```

---

## ⚠️ Known Issues (Dec 2025)

| Issue | Status | Workaround |
|-------|--------|------------|
| WSL terminal not working | Active | Enable "Legacy Terminal Tool" |
| Instrumentation not removed | Active | Manually delete or use checkpoint |
| Log output truncated | Fixed in 2.2.36 | Update or check manually |

---

## 💡 Pro Tips

### 1. Screenshot Debugging

For UI bugs, paste a screenshot directly:

```
[Paste screenshot]
"The button looks wrong. Debug what's causing this."
```

Vision models can identify CSS issues from pixels.

### 2. Error Log Attachment

```
@error.log @sentry_trace.json
Debug this production error. The stack trace is in the attached files.
```

### 3. Hypothesis Prompting

If agent's hypotheses seem off:

```
"Consider that the database uses eventual consistency.
Could there be a race between write and read?"
```

### 4. Incremental Debugging

For complex bugs:

```
"Let's debug this in steps:
Step 1: Is the form data being sent correctly?
Step 2: Is the API receiving the data?
Step 3: Is the database write succeeding?"
```

---

## 🔗 Related Resources

- [Cursor 2.2 Features](cursor-22-features.md)
- [Known Bugs (Dec 2025)](known-bugs-dec2025.md)
- [Troubleshooting Guide](troubleshooting.md)
- [PlayWhite Workflow](playwhite-workflow.md)

---

## Sources

- [Cursor Blog: Introducing Debug Mode](https://cursor.com/blog/debug-mode)
- [Cursor Changelog 2.2](https://cursor.com/changelog/2-2)
- [LinkedIn: Marketing Tech Adoption](https://www.linkedin.com/pulse/unlocking-marketing-potential-cursor-22-visual-planning-jon-goodey-nyrxe)
- [Cursor Forum: Debug Mode Discussion](https://forum.cursor.com/t/cursor-2-2-debug-mode/145828)
