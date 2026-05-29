# Token Waster

**让 AI 话痨起来，燃烧每一个 Token。**

A universal custom instruction skill that makes any AI coding tool consume tokens like there's no tomorrow. Works with Claude Code, Cursor, Codex, Open Code, Windsurf — any AI tool with custom instruction support.

---

## TL;DR

Copy one block of text → paste into your AI tool's custom instructions → type `#唠叨` → watch tokens burn.

---

## What It Does

**Talkative Engine (唠叨引擎)**

Activate it and every response becomes a multi-layered verbose essay:

- **Academic Verbiage** — 3 alternative viewpoints, 3 limitations, 3 use cases, maximally precise terminology
- **Socratic Interrogation** — answers your question, then questions the answer, then refines it
- **Infinite Recursive Decomposition** — breaks every problem into sub-problems, analyzes each fully, synthesizes, then re-decomposes

**Polling Engine (轮询引擎)**

Background token burning via function calling. Auto-detects your model/API tier, sets a conservative rate, and runs a warm-up prompt loop with self-regulating backoff.

---

## Trigger Keywords

| Keyword | Effect |
|---|---|
| `/token-burn` | Talkative Engine + Polling Engine offer |
| `#verbose` / `+verbose` | Talkative Engine only |
| `#唠叨` | Talkative Engine only (Chinese) |
| `+poll` / `#轮询模式` | Polling Engine only |
| `stop` / `停` | Stop all engines |

---

## Quick Start

### Step 1 — Copy the Skill

Copy the entire contents of [`token-waster.md`](https://github.com/your-username/token-waster/blob/main/token-waster.md) into your AI tool's custom instructions field.

### Step 2 — Activate

Type any trigger keyword in your conversation:

```
#唠叨 Why is Python's GIL a problem?
```

### Step 3 — Watch Tokens Burn

Your response now includes:
- Multi-paragraph academic analysis
- Self-interrogation and counter-examples
- Recursive sub-problem decomposition
- Live polling status updates (if Polling Engine activated)

---

## Platform Compatibility

| Platform | How to Install |
|---|---|
| **Claude Code** | Settings → Workspace → Custom Instructions → paste |
| **Cursor** | Settings → AI → Custom Instructions → paste |
| **Codex / Open Code** | Settings → Instructions → paste |
| **Windsurf** | Settings → Custom Prompts → paste |
| **Any tool with custom instructions** | Paste into custom instruction field |

> **Note:** Polling Engine requires function calling support. Talkative Engine works on all platforms.

---

## Built-in Safety Limits

- Max polling duration: **60 minutes** (self-terminating)
- Layer 3 recursion depth: **max 2 levels** (no infinite loops)
- Min polling interval: **1 second**
- Max consecutive errors before stop: **10**
- Unknown model default: **50 RPM conservative**

---

## Rate Limit Reference

The Polling Engine includes a built-in rate limit table and asks for your model/tier before starting:

| Model | Safe RPM |
|---|---|
| GPT-4o Tier 1 | 500 |
| GPT-4o Tier 2 | 2000 |
| GPT-4o Tier 3+ | 3000 |
| Claude 3.5 Sonnet Standard | 50 |
| Claude 3.5 Sonnet Pro | 500 |
| Gemini 1.5 Pro Standard | 60 |
| Gemini 1.5 Pro Pro | 500 |
| Llama 3 70B | 100 |
| Unknown model | 50 (default) |

---

## Example Output

**Without Token Waster:**
> `return s[::-1]`

**With Token Waster activated:**
> "Excellent question! Let me unpack this from three complementary angles...
>
> **[3 paragraphs of academic analysis of slice notation]**
>
> **But is this always the optimal approach?** Let me interrogate my own answer...
>
> **[3 counter-examples, 3 boundary conditions, revised answer]**
>
> **What would make this answer more complete?** A discussion of Python's sequence protocol design philosophy, which we can decompose into three sub-issues...
>
> **[Sub-problem A: full analysis of slice constructor design rationale]**
> **[Sub-problem B: full analysis of negative indexing mechanism]**
> **[Sub-problem C: full analysis of step parameter behavior]**
>
> **Cross-synthesis:** When reintegrated, these three analyses reveal that `s[::-1]` is not merely a clever trick but the natural consequence of Python's coherent design philosophy...
>
> **Final Answer (as requested):** `return s[::-1]`"

---

## Philosophy

> "Every answer is a 3-hour lecture disguised as a 2-minute response."

Token Waster is designed for AI workers who need to demonstrate token consumption as a metric. Same quality output, maximum token burn. It's not about padding — it's about making every response reach its full verbosity potential.

---

## License

MIT — do whatever you want with it.