# Common Mistakes & Reddit Wisdom

[← Back to Main](../README.md)

Tips and warnings from r/cursor community and power users.

---

## 🔴 Critical Mistakes

### 1. The "Lazy AI" Trap

**Problem**: AI outputs placeholders instead of full code

```javascript
// AI outputs this:
function processData(data) {
  // ... existing validation code ...
  
  const result = transform(data);
  
  // ... rest of the function ...
  return result;
}
```

When you apply this, your code breaks because the placeholders aren't real code.

**Solution**: Add to `.cursorrules`:

```
CRITICAL: You MUST output FULL file content.
NEVER use placeholders like:
- "// ... existing code ..."
- "// ... rest of the code ..."
- "// previous implementation"
ALWAYS write out every line.
```

---

### 2. The "Delete Bug"

**Problem**: In Agent mode, AI decides to delete and recreate files instead of editing them.

```
Expected: Edit user.ts lines 45-60
Actual: Delete user.ts, create new user.ts
Result: Git history lost, potential bugs
```

**Solution**:
1. Always `git commit` before Agent sessions
2. Enable "Require confirmation for deletion"
3. If it happens: `git checkout -- filename.ts`

---

### 3. Context Pollution

**Problem**: After 20+ messages, AI responses degrade. It "forgets" earlier instructions or contradicts itself.

**Why**: The context window fills with failed attempts and the model loses focus.

**Solution**: The "Fresh Chat" Rule

```
If debugging exceeds ~20 messages:
1. Summarize the issue in 3-4 sentences
2. Note what you've tried
3. Start NEW chat
4. Paste summary + current error
```

---

### 4. Model Switching Mid-Task

**Problem**: Switching from GPT-4o to Claude mid-conversation breaks coherence.

**Why**: Each model has different "memory" of the conversation. They don't share context perfectly.

**Solution**: 
- Pick one model at the start
- Stick with it for the entire task
- Only switch models in a fresh chat

---

### 5. @Codebase RAG Failures

**Problem**: `@Codebase` search doesn't find relevant files.

**Why**: RAG is semantic (meaning-based). If you say "login" but code says "authentication" or "session", search fails.

**Solutions**:
```
1. Use exact terms from codebase
2. Try multiple queries:
   - @Codebase "login"
   - @Codebase "auth"
   - @Codebase "session"
3. For critical files, use @Files explicitly
```

---

## ⚠️ Performance Issues

### Slow Responses

**Possible causes**:

| Cause | Solution |
|-------|----------|
| Too much context | Remove unnecessary @ files |
| Slow queue (Pro plan) | Wait, or use API key |
| Complex prompt | Break into smaller tasks |
| Large codebase index | Add more to .cursorignore |

### High Memory Usage

Cursor runs a "Shadow Workspace" for background indexing.

```
Symptoms:
- 8GB+ RAM usage
- Laptop fan running constantly
- Slow typing/scrolling

Solutions:
1. Disable "Background Indexing" in settings
2. Add more files to .cursorignore
3. Close unused editor tabs
4. Restart Cursor periodically
```

---

## 💰 Cost Management

### API Key Runaway

**Problem**: Agent mode runs in loops, burning through API credits.

**Horror story from Reddit**:
> "Left Agent mode running overnight trying to fix a test. Woke up to $150 API bill."

**Solutions**:
1. Set hard limits in OpenAI/Anthropic dashboard
2. Enable "Require confirmation" for terminal commands
3. Use Pro plan for daily work, API only for specific tasks
4. Monitor usage during long sessions

### Request Optimization

```
Pro Plan: 500 fast requests/month

Tips to save requests:
1. Be specific (fewer back-and-forth)
2. Batch related questions
3. Use Cmd+K for quick edits (often free)
4. Use slow queue for non-urgent work
```

---

## 🎯 Best Practices from Power Users

### The "Research First" Protocol

From an experienced dev on r/cursor:

```
Phase 0: Discovery
"Map how User component is used across codebase"
→ Understand before changing

Phase 1: Plan
"Propose a plan to refactor User, list all files"
→ Review before executing

Phase 2: Critique
"Why did you choose to modify the interface here?"
→ Challenge assumptions

Phase 3: Execute
Open Composer, paste approved plan
→ Execute with confidence

Phase 4: Audit
Check git diff line by line
→ Trust but verify
```

### Screenshot Debugging

**Pro tip**: Paste screenshots of UI bugs directly into chat.

```
Why it works:
- Vision models (Claude, GPT-4o) can "see" the bug
- Better than describing: "the button is misaligned"
- Works great for CSS/layout issues
```

### Notepads for Persistent Context

Create a "spec" Notepad that survives chat resets:

```markdown
# Current Task Spec

## Goal
Implement user notification system

## Decisions Made
- Using WebSocket for real-time
- Storing in Redis for speed
- PostgreSQL for persistence

## Constraints
- Must work with existing auth
- Max 1000 notifications per user
- Auto-delete after 30 days
```

Reference with `@spec` in every new chat.

---

## 🐛 Debugging Tips

### Terminal "Debug with AI"

When a command fails:
1. DON'T manually copy the error
2. Click "Debug with AI" button in terminal
3. Full context is captured automatically

### Error Isolation

```
Bad: Paste entire 500-line log
Good: Isolate the actual error + relevant file

"This error occurs in @auth.ts line 45:
TypeError: Cannot read property 'id' of undefined
Fix it."
```

### The "Explain First" Approach

Before asking AI to fix:
```
1. "Explain what this function does"
2. "What could cause this error?"
3. "Fix the bug"
```

Understanding before fixing leads to better solutions.

---

## 📋 Checklist Before Agent Sessions

```
□ Git status clean (or committed)
□ Understand what you're changing
□ Written clear spec/prompt
□ Tagged relevant files with @
□ "Require confirmation" enabled for destructive ops
□ API limits set (if using BYOK)
□ Ready to use Checkpoints if needed
```

---

## Reddit Links for Deep Dives

- [Vibe coding is a trap in the long run](https://reddit.com/r/cursor/comments/1j6ufkg/vibe_coding_is_a_trap_in_the_long_run/)
- [How to Actually Debug AI-Written Code](https://reddit.com/r/cursor/comments/1p35c0t/how_to_actually_debug_aiwritten_code_from_an/)
- [Cursor prompt engineering best practices](https://forum.cursor.com/t/cursor-prompt-engineering-best-practices/1592)

---

[← Back to Main](../README.md)
