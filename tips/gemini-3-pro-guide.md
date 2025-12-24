# 🧠 Gemini 3 Pro Guide

> **Breaking News (December 2025)**: Gemini 3 Pro scored **37.5% on Humanity's Last Exam (HLE)** — the highest score ever recorded, surpassing GPT-5.2 (32.1%) and Claude Opus 4.5 (34.8%).

## Why Gemini 3 Pro Matters

Gemini 3 Pro has rewritten the model hierarchy for coding tasks, particularly in:

- **Complex reasoning** and multi-step logic
- **Abstract problem solving**
- **Architectural planning**
- **Semantic code analysis**

---

## Benchmark Comparison

| Model | HLE Score | Strength | Weakness |
|:------|:----------|:---------|:---------|
| **Gemini 3 Pro** | 37.5% | Reasoning, multi-step logic | Slight latency (1-2s) |
| **GPT-5.2** | 32.1% | UI/UX planning, speed | Weaker on complex logic |
| **Claude Opus 4.5** | 34.8% | Legacy refactoring, precision | Expensive ($0.15/1K) |
| **DeepSeek V3** | 28.9% | Price (53x cheaper), speed | Occasional small errors |

---

## New Model Strategy (Production)

Based on the new benchmarks, here's the optimal model allocation:

```
Task Type           → Model              → Time      → Cost
────────────────────────────────────────────────────────────
Architecture        → Gemini 3 Pro       → 5-10s     → Free (Beta)
CRUD operations     → Claude 3.5 Sonnet  → 2-3s      → $0.003
Legacy bug fix      → Claude Opus 4.5    → 10-15s    → $0.04
Test scaffolding    → DeepSeek V3        → 3-4s      → $0.0008
Bulk generation     → Google Antigravity → 2-5 min   → Free
```

### Model Roles

| Role | Model | Rationale |
|:-----|:------|:----------|
| **Chief Architect** | Gemini 3 Pro | Best reasoning, free during beta |
| **Daily Worker** | Claude 3.5 Sonnet | Best bang-for-buck |
| **Refactoring Specialist** | Claude Opus 4.5 | Lowest breaking change risk |
| **Budget Bulk** | DeepSeek V3 | 53x cheaper than Sonnet |
| **Planning Only** | GPT-5.2 | Good speed, avoid for logic-heavy |

---

## Cursor Configuration

### Set Gemini 3 Pro for Codebase Indexing

```
Settings → Codebase Indexing → Model → Gemini 3 Pro
```

**Why?** Gemini 3 Pro has:
- 50K+ context window for semantic understanding
- Better at finding non-obvious code connections
- Superior at "where is X used?" queries

### Recommended Settings

```json
// .cursor/settings.json
{
  "models": {
    "codebase_indexing": "gemini-3-pro",
    "default_chat": "claude-3.5-sonnet",
    "planning": "gemini-3-pro",
    "refactoring": "claude-opus-4.5"
  }
}
```

---

## Gemini 3 Pro Use Cases

### ✅ Perfect For

1. **Architectural Analysis**
   ```
   "Analyze this repo's auth flow. Map all entry points, 
   middleware, and failure modes. Output a mermaid diagram."
   ```

2. **Planning Complex Features**
   ```
   "Plan a rate limiting system. Consider Redis, in-memory, 
   and distributed options. Evaluate trade-offs."
   ```

3. **Semantic Code Search**
   ```
   "@Codebase Where is user authentication validated? 
   Include middleware and guards."
   ```

4. **Abstract Problem Solving**
   ```
   "Design a conflict resolution strategy for real-time 
   collaborative editing. Consider CRDT vs OT."
   ```

### ❌ Not Ideal For

- Quick single-file edits (use Sonnet)
- Real-time pair programming (latency matters)
- Cost-sensitive bulk operations (use DeepSeek)

---

## GPT-5.2 High vs Claude Opus 4.5 vs Gemini 3 Pro

| Feature | GPT-5.2 High | Claude Opus 4.5 | Gemini 3 Pro |
|:--------|:-------------|:----------------|:-------------|
| **Primary Strength** | Production code | Refactoring | Architecture |
| **Context Window** | 400K (stable) | Variable | 50K+ |
| **Reasoning Style** | Convergent | Precise | Divergent |
| **Terminal/DevOps** | Weaker (47.6%) | Strong | Medium |
| **Cost Profile** | Expensive | Very expensive | Free (Beta) |
| **Best For** | Brownfield | Legacy | Greenfield |

### When to Use Each

```
Architecture Planning → Gemini 3 Pro
  ↓
Implementation → Claude 3.5 Sonnet (daily) or GPT-5.2 High (complex)
  ↓
Refactoring Legacy → Claude Opus 4.5
  ↓
Test Writing → DeepSeek V3 (budget) or Sonnet (quality)
```

---

## The Plan-Act Pattern (Updated)

```
1. PLAN (Gemini 3 Pro):
   "Analyze this request. Create architecture plan. Don't code yet."
   
2. CRITIQUE (Optional - GPT-5.2):
   "Review plan.md for edge cases and efficiency gaps."
   
3. EXECUTE (Claude Sonnet or GPT-5.2):
   "Implement Step 1 of plan.md. Run tests after each step."
```

---

## Cost Optimization with Gemini 3 Pro

### Current Pricing (December 2025)

| Platform | Gemini 3 Pro | Notes |
|:---------|:-------------|:------|
| **Google Antigravity** | **FREE** | Beta, unlimited |
| **Cursor (built-in)** | Limited | Rate-limited in Pro plan |
| **API (direct)** | $0.002 input | Cheap if you BYOK |

### The Hybrid Strategy

```
Step 1: Use Antigravity (Free)
  → "Create Next.js 15 auth system with Supabase, 20 files"
  → Wait 15-20 minutes → Get 20 raw files

Step 2: Use Cursor (Paid, 10 min)
  → "Fix imports, connect hooks, delete unused files"
  → Get production-ready code

Result: $0.10 instead of $0.50
```

---

## Common Mistakes

### ❌ Using Gemini for Quick Edits

```
Wrong: "Fix this typo" → Gemini 3 Pro
Right: "Fix this typo" → Claude Sonnet (faster, cheaper)
```

### ❌ Switching Models Mid-Task

```
Wrong: Start with GPT-5.2, switch to Gemini mid-conversation
Right: One model per conversation, maintain "train of thought"
```

### ❌ Ignoring Latency

```
Gemini 3 Pro: 3-5 seconds average response
Claude Sonnet: 1-2 seconds

For real-time pair programming, Sonnet wins on UX.
```

---

## Quick Reference

| Task | Model | Why |
|:-----|:------|:----|
| "Plan auth system" | Gemini 3 Pro | Complex reasoning |
| "Add login button" | Claude Sonnet | Quick, cheap |
| "Refactor legacy API" | Opus 4.5 | Precision |
| "Write 50 unit tests" | DeepSeek V3 | Budget |
| "Debug this error" | GPT-5.2 High | Tool reliability |

---

## Related Guides

- [Model Selection](model-selection.md) - Full model comparison
- [Google Antigravity](google-antigravity.md) - Using Gemini 3 Pro for free
- [Vibe Coding](vibe-coding-guide.md) - AI-first development

---

*Last updated: December 24, 2025*
