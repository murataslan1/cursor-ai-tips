# Confidence Scoring: Anti-Hallucination Technique

A powerful prompt technique discovered by the community to combat model hallucinations and force accurate diagnoses.

---

## The Problem

AI models often provide **confident-sounding but incorrect** diagnoses. The Reinforcement Learning from Human Feedback (RLHF) layer prioritizes "helpfulness" over accuracy, causing the model to:

- Invent plausible-sounding explanations
- Skip verification steps
- Hallucinate constraints or requirements

---

## The Solution: Confidence Scoring Prompt

```
Fix this only if you are 100% confident. Tell me your confidence score.
```

### How It Works

This prompt **bypasses the "helpful assistant" persona** and accesses the model's raw probability assessment:

1. Model re-evaluates its own logic
2. Searches for actual evidence (logs, files)
3. Returns diagnosis with confidence marker
4. If uncertain, admits it instead of hallucinating

---

## Real-World Case Study

### The Routing Hallucination Incident

**Scenario**: SPA routing issue where an endpoint was failing.

**Without Confidence Scoring**:
```
AI: "The endpoint fails because of an 'exact match' constraint 
that conflicts with query parameters."

Result: Factually incorrect. User wasted time on wrong fix.
```

**With Confidence Scoring**:
```
User: "Fix this only if you are 100% confident. Tell me your confidence score."

AI: [Re-evaluates, locates actual log files]
"100% Confidence - Found in error.log line 247: 
Connection timeout to database on port 5432"

Result: Correct diagnosis backed by evidence.
```

---

## When to Use

### Always Use For:

- **Debugging sessions** — when AI proposes a fix
- **Error diagnosis** — when AI explains why something failed
- **Architecture decisions** — when AI suggests a specific approach
- **Dependency claims** — when AI says you "need" something

### Skip For:

- Simple syntax questions
- Documentation lookups
- Code formatting requests

---

## Variations

### Basic

```
Fix this only if you are 100% confident. Tell me your confidence score.
```

### With Evidence Requirement

```
Diagnose this error. Rate your confidence 0-100%.
If below 90%, tell me what evidence you'd need to be certain.
```

### For Debugging

```
Before proposing a fix, locate the actual error in the logs.
Only suggest fixes you're 100% confident about.
Show me the evidence.
```

### For Architecture

```
Is this the best approach? Rate confidence 0-100%.
If below 95%, list alternatives you'd consider.
```

---

## Why It Works (Technical)

The models maintain an **internal probability metric** regarding their own accuracy. This metric is typically suppressed by RLHF training that optimizes for user satisfaction.

By explicitly demanding a confidence score:

1. You trigger the model's self-evaluation circuits
2. You bypass the "always be helpful" training
3. You access the raw probability assessment
4. Low-confidence responses get flagged instead of hidden

---

## Integration with .cursorrules

Add to your `.cursorrules` or `.cursor/rules/*.mdc`:

```
When debugging or diagnosing errors:
1. Always search for actual log files and error messages
2. Rate your confidence before proposing fixes
3. If confidence < 90%, state what evidence is missing
4. Never guess at root causes without evidence
```

---

## Pro Tips

1. **Use at decision points** — not every message, but when the AI proposes action
2. **Demand evidence** — "Show me where in the code/logs you found this"
3. **Challenge low confidence** — if AI says 70%, ask what would make it 100%
4. **Verify the 100%s** — even confident answers can be wrong; spot-check

---

## References

- [Reddit: The Prompt That Finally Stopped Cursor's Hallucinations](https://www.reddit.com/r/cursor/comments/1pepxk1/the_prompt_that_finally_stopped_cursors/)
