# 🐛 Visual Editor "Infinite Loop" Bug (Cursor 2.3.9)

**Severity:** 🟧 HIGH (Usability)
**Status:** Active in v2.3.9
**Workaround:** Avoid "Visual" tab

## What is it?
The new Visual Editor (GUI for CSS tweaking) has a race condition where the Agent gets stuck in a loop trying to apply styles.

## Symptoms
*   The Agent repeatedly asks "Where did the previous plan go?"
*   You see the same CSS change being applied, checked, and re-applied 10+ times.
*   Massive API credit drain.

## 🛡️ Workaround
**Strictly use Source View.**
Do not open the "Visual" tab in the side panel until Anysphere releases patch 2.4.

```text
Visual Editor: ❌ AVOID
Source Editor: ✅ SAFE
```
