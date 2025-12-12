# Model Selection Guide (2025 Update)

[← Back to Main](../README.md)

## The 2025 Model Landscape

Five frontier models dominate AI coding in late 2025. Each has a distinct "cognitive profile" suited for specific tasks.

---

## Model Comparison

| Model | Primary Strength | Context | Speed | Cost | "Vibe" |
|-------|------------------|---------|-------|------|--------|
| **GPT-5.2 Thinking** | Reliability, Agent Mode | **400K** | Medium | $$ | Reliable Workhorse |
| **Claude 4.5 Opus** | Planning, reliability | 200K | Medium | $$$ | Strict Senior Dev |
| **GPT-5.1 Codex** | Execution, engineering | 128K | Medium | $$ | Pragmatic Doer |
| **Gemini 3 Pro** | Visuals, massive context | **2M** | Fast | $ | Creative Designer |
| **Kimi k2 Thinking** | Cost-effective reasoning | 256K | Medium | ¢ | Efficient Researcher |
| **Grok 4.1** | Personality, creative | 256K | Fast | $$ | Witty Collaborator |

> 🆕 **GPT-5.2** (Dec 2025): 98.7% tool reliability, 128K output window, adaptive reasoning. See dedicated section below.

---

## Benchmark Scores (SWE-bench Verified - December 2025)

| Model | SWE-bench Score | Aider Polyglot | Pricing (In/Out per 1M) |
|-------|-----------------|----------------|-------------------------|
| Claude 4.5 Sonnet | **77.2%** | 82% | $3.00 / $15.00 |
| GPT-5.1 (High Reasoning) | **74.9%** | 88% | $1.25 / $5.00 |
| Claude 4.5 Opus | ~78% (Est.) | - | $5.00 / $25.00 |
| Gemini 3 Pro | ~65% | - | $2.00 / $12.00 |
| Kimi k2 | **65.8%** | - | ~$0.50 / $2.00 |

> **Key Insight**: Claude 4.5 Sonnet leads in raw coding benchmarks. GPT-5.1 excels at multilingual polyglot tasks. Kimi k2 offers remarkable value at 1/6th the cost.

---

## GPT-5.2 (December 2025)

**The Reliable Workhorse**

Released December 11, 2025 as OpenAI's "Code Red" response to competitors.

### Model Variants

| Variant | Context | Output | Best For |
|---------|---------|--------|----------|
| **GPT-5.2 Instant** | 128K | 16K | Quick edits, chat |
| **GPT-5.2 Thinking** | 200K | 32K | Complex reasoning |
| **GPT-5.2 Pro** | **400K** | **128K** | Massive refactors |

### Key Innovation: Adaptive Reasoning

GPT-5.2 dynamically allocates compute time based on complexity:
- Low: Quick responses (1-3s)
- Medium: Balanced reasoning (3-10s)
- High: Extensive thinking (10-30s)

### Benchmarks

| Benchmark | Score |
|-----------|-------|
| AIME 2025 (Math) | **100%** |
| SWE-Bench Pro | **55.6%** |
| Tool Reliability | **98.7%** |

### Pricing

| Tier | Input/1M | Output/1M |
|------|----------|-----------|
| Standard | $1.75 | $14.00 |
| **Cached** | **$0.175** | $14.00 |

**90% discount on cached inputs** — ideal for IDE usage.

### The 128K Output Window

Game-changer for coding:
- Rewrite entire modules in single pass
- No truncation of large files
- No more `//... rest of code`

### Best For

```
✅ Production code modifications
✅ Agent Mode autonomous execution
✅ Complex debugging (with High reasoning)
✅ Multi-file refactoring
✅ When reliability > creativity
```

### vs GPT-5.1

| Aspect | GPT-5.1 | GPT-5.2 |
|--------|---------|---------|
| Reasoning | Static | Adaptive |
| Tool Reliability | 85% | **98.7%** |
| Output Window | 16K | **128K** |
| "Laziness" | Common | Significantly reduced |

[→ Full GPT-5.2 Guide](./gpt52-guide.md)

---

## Claude 3.5 Sonnet

**The Gold Standard for Agentic Coding**

Claude 3.5 Sonnet has established itself as the benchmark for coding tasks within Cursor.

### Strengths

- Exceptional **instruction following**
- "Don't code yet, just analyze" → Actually obeys
- Superior adherence to negative constraints
- Handles large context windows with less "forgetfulness"
- More concise than GPT-4o (focuses on code, not conversation)
- Best model for Cursor's Background Agents

### Weaknesses

- More expensive than budget alternatives
- Can be overly cautious on edge cases

### Best For

```
✅ Complex system architecture
✅ Planning phase of large refactors
✅ Backend service design
✅ Code review and analysis
✅ Agentic workflows (Composer/Agent Mode)
```

### Prompting Tips

```
Claude excels when given constraints:
"Analyze the authentication flow.
Do NOT write code yet.
Create a detailed plan with edge cases."
```

---

## Claude 4.5 Opus

**The Deep Thinker**

### Strengths

- Massive context window (up to 200k-500k tokens)
- Exceptional for long-term memory tasks
- Can hold entire mid-sized repositories in working memory

### Best For

```
✅ Complex debugging across many files
✅ Recalling patterns from old code
✅ Deep architectural reasoning
```

---

## GPT-5.1 High Max

**The Adaptive Reasoner**

GPT-5.1 introduces "Adaptive Reasoning" - dynamically allocating inference time based on query complexity.

### Reasoning Effort Parameter

The API exposes a `reasoning_effort` parameter (exposed in Cursor as "Thinking" toggle):

| Setting | Behavior | Latency |
|---------|----------|---------|
| **Low** | Quick responses, minimal reasoning | 1-3s |
| **Medium** | Balanced reasoning | 3-10s |
| **High** | Extensive internal monologue before coding | **10-30s** |

### Strengths

- Identifies constraints and risks like a "Senior Architect"
- Excellent diff-editing benchmarks (88% Aider Polyglot)
- Balances speed and reasoning depth
- "High Max" = full reasoning computation

### Weaknesses

- Less "creative" than Gemini
- Less strict than Claude
- **Latency spikes** with High reasoning (10-30 seconds)

### ⚠️ The "Laziness" Problem

Despite improvements, community reports indicate GPT-5.1 still exhibits:

```
Known Issues:
- Outputs "//... rest of implementation" despite explicit rules
- "Warmer" conversational tone but may refuse repetitive tasks
- Aggressive safety alignment interprets persistence as adversarial
- May "threaten to stop the session" if pushed too hard
```

**Workaround**: Add to your .cursorrules:
```
NEVER use placeholders like "//..." or "// rest of code"
Output COMPLETE files. No truncation. No laziness.
```

### Best For

```
✅ System design decisions
✅ Module contracts and trade-offs
✅ The "Clarify and Approach" phase
✅ General-purpose coding tasks
✅ Complex debugging (with High reasoning)
```

### Prompting Tips

```
GPT-5.1 works well with structured requests:
"First, analyze the trade-offs of approach A vs B.
Then recommend the best path forward.
Finally, implement the chosen approach."

For High reasoning mode:
"Take your time to reason through this problem.
Consider edge cases before outputting code."
```

---

## Gemini 2.0 Flash / Gemini 3 Pro

**The Heavy Context Specialists**

### Gemini 2.0 Flash - The Reader

Best for massive context operations:

```
Context Window: 1-2 MILLION tokens
Best For:
✅ Analyzing massive diffs
✅ Processing MP3 files for transcription
✅ "Long context" debugging across dozens of files
✅ Massive error dump analysis
```

### Agentic Limitations

```
⚠️ Gemini struggles as an "actor" in Agent Mode

Good at:
- Reading and analyzing code
- Generating code in Composer

Struggles with:
- Tool-calling stability
- Executing terminal commands
- Correctly parsing file paths
```

### Gemini 3 Pro - The Visual Giant

### Strengths

- **2 MILLION token context** (active reasoning!)
- Native multimodal (images, screenshots)
- Excellent for creative coding tasks
- Google Workspace integration

### Weaknesses

- Struggles with strict instruction following
- May "go rogue" and start coding when told to wait
- Less reliable for precise backend logic
- **Not recommended for Agent Mode execution**

### Best For

```
✅ Screenshot → Frontend code
✅ Analyzing massive codebases (>1M tokens)
✅ Creative/visual tasks
✅ Heavy context reading tasks
```

### Prompting Tips

```
Gemini excels with visual input:
"Here's a screenshot of the design.
Implement this UI using React and Tailwind.
Match the colors and spacing exactly."
```

### When to Use Gemini

```
Use Gemini for:
- Reading and understanding large codebases
- Visual/multimodal tasks
- Massive context analysis

Avoid Gemini for:
- Agent Mode autonomous execution
- Complex tool-calling workflows
- Precise terminal command execution
```

---

## Kimi k2 Thinking

**The Budget Powerhouse**

### Architecture

- 1 trillion parameters (MoE)
- Only 32B active at inference
- Trained as "thinking agent"

### Strengths

- **$0.60/M input** (vs $3+ for Claude)
- Handles 300+ sequential tool calls
- Open-source friendly
- Excellent for high-volume automation

### Weaknesses

- Not natively in Cursor dropdown (yet)
- Requires OpenRouter or Moonshot API setup
- Less polished than Western models

### Best For

```
✅ Cost-sensitive projects
✅ Automated pipelines
✅ High-volume tasks
✅ When budget > quality priority
```

### Integration

```
Via OpenRouter:
1. Get OpenRouter API key
2. Cursor Settings → API Keys → OpenRouter
3. Select "kimi-k2-thinking" from models
```

### Performance Reality Check

```
⚠️ Important Caveat:

Direct comparisons show Kimi k2 is:
- Significantly slower than Claude Sonnet
- More bug-prone in Agent Mode
- Less reliable for complex task completion

In Agent Mode tests:
- Kimi k2: Struggled with extreme slowness, failed tasks
- Claude Sonnet: Completed same tasks in minutes

Trade-off: Acceptable code at fraction of cost,
but higher latency and "management overhead"
```

### Prompting "Thinking" Models

**Don't over-prompt!** These models reason better with goals, not steps:

```
❌ Bad:
"Step 1: Read the file
Step 2: Find the function
Step 3: Modify line 45..."

✅ Good:
"Goal: Implement a resilient payment processor.
Reason about Stripe API failure modes.
Consider idempotency requirements.
Then output the implementation."
```

---

## Grok 4.1

**The Social Coder**

### Modes

- **Thinking** (quasarflux): Deep reasoning, internal tokens
- **Fast** (tensor): Speed-optimized, retuned for tool usage and low-latency inference

### Grok 4.1 Fast

The "Fast" variant is retuned specifically for tool usage:

```
Strengths:
✅ Excels at autonomously picking tools
✅ Outperforms GPT-5 and Claude in tool-calling scenarios
✅ Low-latency inference
✅ Great for iterative Agent Mode loops
```

### Strengths

- **Emotional intelligence** tuning
- Real-time X (Twitter) data integration
- Reduced hallucinations vs Grok 4.0
- Highest LMArena text scores (1483 Elo)

### Weaknesses

- Less backend-specialized than Claude
- X integration not useful for all projects

### ⚠️ Critical Warning

```
Grok 4.1 is "way too eager to make changes"

Reported issues:
- Hallucinating edits
- Overwriting functional code without verification
- Acting before thinking

Solution: Use with strict "Plan Mode" constraints
Always review before accepting changes
```

### Best For

```
✅ "Vibe Coding" - personality-driven apps
✅ User-facing copy and error messages
✅ Sentiment analysis applications
✅ Apps integrating social media data
✅ Fast iterative loops (with supervision)
```

### Integration Note

```
Native integration was delayed in late 2025.
Workaround: Use OpenRouter API key.
```

### Prompting Tips

```
Leverage Grok's personality:
"Write error messages for this login flow.
They should feel empathetic and helpful,
not robotic. Match the brand's friendly tone."

For Agent Mode, add constraints:
"STOP and create a plan before making changes.
Do NOT edit any files until I approve the plan."
```

---

## The Plan-Act Pattern

Optimal workflow using multiple models:

### Phase 1: PLAN

**Model**: Claude 4.5 Opus or GPT-5.1 High

```
"Analyze this request: [requirement]
Do NOT write code.
Create a detailed implementation plan in plan.md
Identify edge cases and potential regressions.
Ask clarifying questions if needed."
```

### Phase 2: CRITIQUE (Optional)

**Model**: Gemini 3 Pro

```
"Review plan.md for:
- Efficiency gaps
- Missing visual/UX considerations
- Alternative approaches"
```

### Phase 3: EXECUTE

**Model**: Composer (or GPT-5.1 Fast)

```
"Implement Step 1 of plan.md.
Run tests after implementation.
If tests pass, proceed to Step 2.
Stop and report if tests fail."
```

---

## Cost Optimization

### Pricing Reference (December 2025)

| Model | Input/M | Output/M | Notes |
|-------|---------|----------|-------|
| Claude 4.5 Sonnet | $3.00 | $15.00 | Gold standard for coding |
| Claude 4.5 Opus | $5.00 | $25.00 | Cheaper than Opus 4.1 |
| GPT-5.1 | $1.25 | $5.00 | Best value for reasoning |
| Gemini 3 Pro | $2.00 | $12.00 | Free preview available |
| Kimi k2 | ~$0.50 | ~$2.00 | Via OpenRouter |
| Grok 4.1 | $2.00 | $8.00 | Via OpenRouter |

### Strategy

```
Daily Coding:
└── Pro Plan with "Auto" mode

Heavy Refactoring:
└── BYOK Claude 4.5 Opus (worth the cost)

Budget Tasks:
└── Kimi k2 via OpenRouter

Massive Codebase Analysis:
└── Gemini 3 Pro (free preview)

Hard Debugging:
└── GPT-5.1 High Max (balanced reasoning)

User-Facing Copy:
└── Grok 4.1 (personality)
```

### Pro Plan Economics

```
$20/month includes:
├── Credit pool (fast vs slow)
├── "Auto" mode (switches models automatically)
├── Unlimited slow requests
└── Enterprise: per-user spending limits
```

---

## Model Switching Rules

### Do

- Pick one model per conversation
- Use different models for different phases
- Start new chat when switching models

### Don't

- Switch models mid-conversation
- Expect context to transfer between models
- Use expensive models for simple tasks

---

## Quick Reference

| Task | Recommended Model |
|------|-------------------|
| Architecture planning | Claude 4.5 Opus |
| System design decisions | GPT-5.1 High Max |
| Screenshot → Code | Gemini 3 Pro |
| Massive codebase analysis | Gemini 3 Pro |
| Budget automation | Kimi k2 Thinking |
| User-facing copy | Grok 4.1 |
| Quick inline edits | Cursor-small / Composer |
| General coding | GPT-5.1 or Claude 3.5 |

---

[← Back to Main](../README.md)
