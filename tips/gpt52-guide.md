# GPT-5.2 Guide: The "Code Red" Model

[← Back to Main](../README.md)

Released December 11, 2025 — OpenAI's response to Gemini 3 and Claude 4.5.

---

## Model Family

| Model | Use Case | Context | Output | Latency |
|-------|----------|---------|--------|---------|
| **GPT-5.2 Instant** | Chat, quick edits | 128K | 16K | 1-3s |
| **GPT-5.2 Thinking** | Complex reasoning | 200K | 32K | 10-30s |
| **GPT-5.2 Pro** | Massive operations | **400K** | **128K** | Variable |

---

## Key Innovation: Adaptive Reasoning

GPT-5.2 dynamically allocates compute time based on complexity.

### Extended Thinking

Can pause 10-30 seconds to:
- Plan architectural changes
- Analyze multi-file dependencies
- Consider edge cases before coding

### Reasoning Settings in Cursor

| Setting | Behavior | Best For |
|---------|----------|----------|
| Low | Quick responses | Simple edits |
| Medium | Balanced | General coding |
| High | Extensive reasoning | Architecture, debugging |

---

## Benchmark Results

| Benchmark | GPT-5.2 | Previous Best |
|-----------|---------|---------------|
| **AIME 2025 (Math)** | **100%** | 83% |
| **SWE-Bench Pro** | **55.6%** | 49% |
| **Tool Reliability** | **98.7%** | 85% |

> SWE-Bench >50% = model can resolve **majority** of mid-level engineering tickets autonomously.

---

## Pricing

| Tier | Input/1M | Output/1M |
|------|----------|-----------|
| Standard | $1.75 | $14.00 |
| **Cached** | **$0.175** | $14.00 |

**90% discount on cached inputs** — ideal for IDE usage where same codebase context is sent repeatedly.

---

## GPT-5.2 Pro: 128K Output

The game-changer:
- Rewrite entire modules in single pass
- No truncation of large files
- No more `//... rest of code`

---

## vs GPT-5.1

| Aspect | GPT-5.1 | GPT-5.2 |
|--------|---------|---------|
| Reasoning | Static | Adaptive |
| Context | 128K | Up to 400K |
| Output | 16K | Up to 128K |
| Tool Reliability | 85% | **98.7%** |
| "Laziness" | Common | Significantly reduced |

---

## vs Competitors

| Model | Strength | Best For |
|-------|----------|----------|
| **GPT-5.2** | Reliability, tool-use | Production code, Agent Mode |
| **Claude 4.5** | Deep reasoning | Architecture planning |
| **Gemini 3** | Massive context, free | >1M token analysis |

---

## Usage Recommendations

| Task | Model |
|------|-------|
| Architecture | GPT-5.2 Thinking (High) |
| Implementation | GPT-5.2 Thinking (Medium) |
| Quick edits | GPT-5.2 Instant |
| Massive refactors | GPT-5.2 Pro |
| Budget | Gemini 3 / Kimi k2 |

---

## Quick Reference

```
Fast response     → GPT-5.2 Instant
Complex problem   → GPT-5.2 Thinking (High)
Large codebase    → GPT-5.2 Pro
Reliability       → GPT-5.2 (98.7% tool accuracy)
Budget            → Gemini 3 or Kimi k2
```

---

[← Back to Main](../README.md)
