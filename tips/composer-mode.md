# Composer Mode: The Agentic Engine

[← Back to Main](../README.md)

## What is Composer?

Composer is Cursor's **killer feature** - an autonomous AI agent that can:
- Plan multi-file changes
- Create and delete files
- Run terminal commands
- Self-correct based on errors

It's the difference between "autocomplete" and "auto-development."

---

## Opening Composer

| Action | Mac | Windows/Linux |
|--------|-----|---------------|
| Composer Window | `Cmd + I` | `Ctrl + I` |
| Composer Fullscreen | `Cmd + Shift + I` | `Ctrl + Shift + I` |

Use **Fullscreen** for large refactors where you need to review many files.

---

## Normal Mode vs Agent Mode

### Normal Mode (Default)

- AI proposes changes
- You review diff (green/red)
- Click "Accept" to apply
- **Safe** - nothing happens without approval

### Agent Mode

Enable in Composer settings.

- AI can create/delete files automatically
- AI can run terminal commands
- Self-healing loops (run test → fix → rerun)

**Capabilities:**
```
✅ Create files
✅ Delete files
✅ Run shell commands
✅ Read command output
✅ Iterate until success
```

**Risks:**
```
⚠️ May delete files unexpectedly
⚠️ May run destructive commands
⚠️ Can burn through API credits fast
⚠️ "YOLO mode" can cause chaos
```

### Safety Configuration

Recommended settings:

```
✅ Require confirmation for: file deletion
✅ Require confirmation for: destructive commands (rm, git reset)
❌ Don't require for: safe commands (ls, cat, grep, npm test)
```

---

## Checkpoints: Time Travel

Composer creates snapshots at each step.

### How It Works

```
Step 1: AI modifies auth.ts      [Checkpoint 1]
Step 2: AI modifies routes.ts    [Checkpoint 2]
Step 3: AI breaks the build      [Checkpoint 3]
         ↓
Click Checkpoint 2 → Workspace reverts
```

### Why It Matters

- **Low-risk experimentation** - try bold refactors
- **Easy recovery** - one click to undo
- **Context preserved** - don't lose chat history

### Pro Tip

Before major Agent sessions:
1. Create git commit (backup)
2. Let Composer create checkpoints
3. If all else fails, `git reset --hard`

---

## Effective Prompting

### Bad Prompts

```
❌ "Add a login page"
❌ "Fix the bug"
❌ "Make it work"
```

These are vague. AI will guess and probably guess wrong.

### Good Prompts (Spec-First)

```
✅ Implement user authentication:

Context files:
@src/models/user.ts
@src/services/auth.ts
@src/app/api/routes.ts

Requirements:
1. Create login endpoint POST /api/auth/login
2. Validate email/password with Zod
3. Return JWT token on success
4. Match error format in @src/lib/errors.ts

Constraints:
- Use existing bcrypt for password comparison
- Don't modify database schema
- Follow existing code patterns

Verification:
- Create test in tests/auth/login.test.ts
- Run tests to verify
```

### The Structure

1. **Context Loading**: Tag relevant files with @
2. **Clear Objective**: What exactly to build
3. **Constraints**: What NOT to do
4. **Verification**: How to confirm success

---

## Multi-File Refactoring

### Example: Replace Fetch with Axios

```
Refactor all API calls to use Axios instead of fetch:

Files to modify:
@src/services/api.ts
@src/services/userService.ts
@src/services/productService.ts

Steps:
1. Install axios if not present (check package.json)
2. Create axios instance in src/lib/axios.ts with baseURL
3. Replace fetch calls with axios
4. Update error handling to use axios interceptors
5. Update types for axios responses

Constraints:
- Keep existing function signatures
- Don't change component code
- Maintain error message format

Test:
- Run existing tests to verify nothing broke
```

### Example: Add Feature Module

```
Create new "notifications" feature:

Structure:
src/features/notifications/
├── components/
│   ├── NotificationBell.tsx
│   └── NotificationList.tsx
├── hooks/
│   └── useNotifications.ts
├── services/
│   └── notificationService.ts
├── types/
│   └── index.ts
└── index.ts (barrel export)

Requirements:
- Follow patterns from @src/features/auth/
- Use existing UI components from @src/components/ui/
- Connect to existing WebSocket in @src/lib/socket.ts

Don't:
- Create new API routes yet
- Modify existing features
```

---

## Plan Mode

Before writing code, ask Composer to plan:

```
PLAN ONLY - Don't write code yet.

I need to add rate limiting to all API routes.
List all files you would modify and explain what changes.
```

Composer outputs:
```markdown
## Plan: Add Rate Limiting

### Files to Create:
1. src/lib/rateLimit.ts - Rate limiter utility

### Files to Modify:
1. src/app/api/auth/route.ts - Add rate limit check
2. src/app/api/users/route.ts - Add rate limit check
...

### Approach:
- Use upstash/ratelimit package
- Create middleware pattern
- 100 requests per minute per IP
```

Review the plan, then say "Proceed" to execute.

---

## Common Patterns

### Pattern 1: Research → Plan → Execute

```
Step 1: "Explain how authentication works in @src/"
        (understand before changing)

Step 2: "Plan a refactor to add OAuth support"
        (review the plan)

Step 3: "Execute the plan"
        (now write code)
```

### Pattern 2: TDD with Composer

```
Step 1: "Write tests for a shopping cart service"
        (test first)

Step 2: "Review tests" (you verify)

Step 3: "Implement the service to pass these tests"
        (code to match tests)
```

### Pattern 3: Incremental Refactor

```
Step 1: "Refactor userService.ts to use async/await"
Step 2: "Now refactor productService.ts the same way"
Step 3: "Now orderService.ts"
        (one file at a time, verify each)
```

---

## Troubleshooting

### Composer Goes Off Track

**Problem**: AI makes unwanted changes

**Solutions**:
1. Click earlier Checkpoint
2. Be more explicit in constraints
3. Break into smaller tasks

### Context Too Large

**Problem**: "Context limit exceeded"

**Solutions**:
1. Remove unnecessary @ files
2. Use @Folders instead of @Files for overview
3. Start fresh chat with only essential context

### Agent Deletes Files

**Problem**: Agent mode removes files unexpectedly

**Solutions**:
1. Always `git commit` before Agent sessions
2. Enable "require confirmation for deletion"
3. Use Normal mode for unfamiliar codebases

### Slow Responses

**Problem**: Composer takes forever

**Solutions**:
1. Reduce context (fewer @ files)
2. Use faster model for simple tasks
3. Check if you're in "slow queue" (Pro plan limit)

---

## Best Practices Summary

| Do | Don't |
|----|-------|
| ✅ Tag specific files with @ | ❌ Add entire codebase |
| ✅ Write detailed specs | ❌ Use vague prompts |
| ✅ Review plans before executing | ❌ Let Agent run wild |
| ✅ Commit before Agent mode | ❌ Trust without verification |
| ✅ Use Checkpoints | ❌ Ignore rollback options |
| ✅ Break large tasks into steps | ❌ Ask for everything at once |

---

[← Back to Main](../README.md)
