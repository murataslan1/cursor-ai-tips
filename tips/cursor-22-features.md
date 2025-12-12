# Cursor 2.2 New Features

[← Back to Main](../README.md)

Released December 10, 2025, Cursor 2.2 introduces groundbreaking agentic capabilities.

---

## Debug Mode

**From Static Analysis to Dynamic Observation**

### How It Works

1. **Instrumentation**: Agent injects logging statements and probes into execution path
2. **Human Trigger**: Agent pauses, you trigger the bug manually
3. **Root Cause Analysis**: Agent analyzes runtime data (timestamps, stack traces) for empirical deduction

### Usage

```
1. Describe bug in Chat/Composer
2. Agent instruments code with logging
3. YOU trigger the bug
4. Agent analyzes logs
5. Agent proposes fix based on actual runtime behavior
```

### Best Models

- GPT-5.2 Thinking (best instrumentation accuracy)
- Claude 4.5 Opus

---

## Visual Editor

**Bidirectional DOM ↔ Source Code**

### Features

- **Direct Manipulation**: Select elements in browser, modify CSS via GUI
- **Source-Aware**: Changes write to source files (Tailwind, .css), not just DOM
- **Point and Prompt**: Multi-select + natural language:

```
"Align these cards and give them a drop shadow"
"Center this form vertically"
```

> ⚠️ **Known Bug**: Visual Editor has infinite loop issue in 2.2. See [Cursor 2.2 Bugs](./cursor-22-bugs.md).

---

## Multi-Agent Judging

**Probabilistic Search for Better Results**

### Architecture

```
Your Prompt
    │
    ├── Agent 1 (GPT)
    ├── Agent 2 (Claude)
    └── Agent N (Gemini)
          │
      Judge Agent
          │
    Best Solution
```

### How It Works

1. Multiple agents tackle your prompt in parallel
2. "Judge" agent evaluates outputs (correctness, efficiency, adherence)
3. Best solution presented with rationale

### Cost Warning

Dramatically increases token cost. Enable only for complex/critical tasks.

```
Settings → Features → Multi-Agent Judging
```

---

## Plan Mode Improvements

| Before (2.1) | After (2.2) |
|--------------|-------------|
| Basic planning | Clarifying questions first |
| Single analysis | Risk analysis included |
| Manual checkpoints | Auto-creates restore points |

### Example Flow

```
Prompt: "Refactor auth to use JWT"

Agent:
1. [CLARIFYING] Which JWT library?
2. [ANALYSIS] Current auth: 12 files affected
3. [RISK] Breaking change for existing sessions
4. [PLAN] 5-step migration proposed
5. [CHECKPOINT] Created restore point

Proceed? [Yes/No/Modify]
```

---

## Related

- [Cursor 2.2 Bugs](./cursor-22-bugs.md)
- [GPT-5.2 Guide](./gpt52-guide.md)

[← Back to Main](../README.md)
