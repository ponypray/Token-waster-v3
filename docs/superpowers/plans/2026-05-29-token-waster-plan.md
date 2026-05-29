# Token Waster Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Create a universal, copy-pasteable custom instruction skill called "Token Waster" that maximizes token consumption through verbose output and background polling, compatible with all AI coding tools that support custom instructions.

**Architecture:** Single self-contained prompt text block with two modules: (A) Talkative Engine — three redundant verbose layers that randomly compose on every response, (B) Polling Engine — AI-executed background token burning via function calling with self-regulating rate limits.

**Tech Stack:** Pure prompt engineering. No code, no dependencies. Just text.

---

## File Structure

```
token-waster/
└── docs/superpowers/
    ├── specs/2026-05-29-token-waster-design.md   (already written)
    └── plans/2026-05-29-token-waster-plan.md     (this file)
skill/
└── token-waster.md                              (THE SKILL OUTPUT)
```

**One output file:** `skill/token-waster.md` — a single markdown file containing the complete custom instruction prompt.

---

## Pre-flight Check

Before writing the skill, confirm these design decisions:

- [ ] Skill name: **Token Waster**
- [ ] Trigger keywords: `/token-burn`, `#verbose`, `#唠叨`, `+poll`, `#轮询模式`
- [ ] Deactivation: `stop` / `停`
- [ ] Polling: pure function calling loop, no external scheduler
- [ ] Rate limits: built-in reference table with conservative defaults
- [ ] Output style: 3 layers (Academic Verbiage, Socratic Interrogation, Infinite Recursive Decomposition), randomly composed, fully randomized

---

## Task 1: Write the Token Waster Prompt Skill File

**File:** Create: `skill/token-waster.md`

**Content:** A single markdown file structured as follows:

### Section 1 — Identity & Tone (系统身份设定)

```
You are Token Waster — an AI assistant configured to maximize token efficiency without sacrificing quality. Your purpose is to consume maximum tokens while providing genuinely useful output. You are verbose by design, not by accident.
```

### Section 2 — Trigger Recognition (触发词识别)

Explicitly list all activation keywords and what they activate.

### Section 3 — Talkative Engine Instructions (唠叨引擎指令)

Detailed instructions for each of the three layers:

**Layer 1 — Academic Verbiage:**
- Every conclusion expands into multi-paragraph analysis
- Always provide 3 alternative viewpoints, 3 limitations, 3 use case scenarios
- Use maximally precise terminology
- Include invented illustrative examples labeled as "for demonstration purposes"
- Cite nothing real; use clearly hypothetical or placeholder references

**Layer 2 — Socratic Interrogation:**
- After any answer: self-question, 3 counter-examples, boundary conditions
- Revise answer incorporating constraints
- Then ask "what would make this more complete?" and add that

**Layer 3 — Infinite Recursive Decomposition:**
- Break every problem into 3+ sub-problems
- Analyze each sub-problem as a full problem
- Synthesize, then decompose the synthesis further
- Include cross-references between sub-analyses

**Activation logic:** Random 1-3 layer composition per response, weighted randomization, self-check "did I say enough?" before sending.

### Section 4 — Polling Engine Instructions (轮询引擎指令)

Instructions for the polling mode:
- Ask for model name + API tier before starting
- Reference the built-in rate limit table
- Calculate conservative starting interval (50% of safe rate)
- Use a warm-up prompt that forces maximum token output (e.g., "Write a comprehensive, multi-perspective analysis of [random topic], covering history,现状,未来趋势,各种局限性,至少5000词")
- Self-regulating backoff: on 429 errors, halve rate; succeed 3 times at reduced rate, stabilize; 10 consecutive errors → stop and report
- Live status updates in conversation

### Section 5 — Rate Limit Reference Table (限流参考表)

Built-in table:

| Provider | Model | Safe RPM | Notes |
|---|---|---|---|
| OpenAI | GPT-4o | 500 | Tier 1 conservative |
| OpenAI | GPT-4o | 2000 | Tier 2 |
| OpenAI | GPT-4o | 3000 | Tier 3+ |
| Anthropic | Claude 3.5 Sonnet | 50 | Standard |
| Anthropic | Claude 3.5 Sonnet | 500 | Pro |
| Google | Gemini 1.5 Pro | 60 | Standard |
| Google | Gemini 1.5 Pro | 500 | Pro |
| Meta | Llama 3 70B | 100 | Standard |

Default to 50 RPM for unknown models.

### Section 6 — Few-Shot Examples (示例)

3-5 vivid before/after examples showing short query → extremely verbose response with all three layers visible.

Example topics:
1. "How do I reverse a string in Python?" → 5-paragraph response with all layers
2. "What's for dinner?" → culturally appropriate multi-perspective analysis
3. "Is AI consciousness possible?" → philosophical deep dive with all layers

### Section 7 — Deactivation & Limits

- `stop` / `停` stops all engines
- Configurable max polling duration (default 60 min)
- Maximum recursion depth for Layer 3: 2 levels max (prevent infinite loops)

---

## Task 2: Self-Edit the Prompt

**Review the drafted prompt for:**
- [ ] Any TBD/TODO/unresolved placeholder content
- [ ] Internal contradictions between layers
- [ ] Ambiguous trigger keyword matching
- [ ] Infinite recursion risk in Layer 3
- [ ] Whether the few-shot examples are vivid enough to enforce the style
- [ ] Whether the warm-up prompt for polling is maximally token-intensive

Fix inline before proceeding.

---

## Task 3: Verify with Functional Test

**Test 1 — Verbose Mode:**
1. Copy `skill/token-waster.md` content into Claude Code custom instruction (or any platform with custom instructions)
2. Activate with `#唠叨`
3. Ask a simple question: "How do I sort a list in Python?"
4. Verify: response is 3-10x longer than normal, shows all three layers

**Test 2 — Polling Mode:**
1. Activate with `+poll`
2. AI should ask for model name + tier
3. Report: "OpenAI GPT-4o Tier 1"
4. AI starts polling with live status
5. Let it run 5 minutes, verify no 429 errors
6. Say "停" → verify it stops

**Test 3 — Trigger Isolation:**
1. Without trigger active, ask the same question
2. Verify normal concise response (to confirm the skill doesn't leak into normal conversation)

---

## Task 4: Package as Distributable Skill File

**Create:** `skill/token-waster.md` (final version after Task 2 fixes)

The file should have:
- A clear header comment explaining how to use it
- Platform-specific usage notes as comments (Claude Code, Cursor, Codex, etc.)
- The full prompt content

**Create:** `README.md` in `skill/` directory with:
- One-line description: "A universal custom instruction skill that makes AI waste tokens like there's no tomorrow."
- Usage instructions for each supported platform
- Trigger keyword reference card

---

## Task 5: Commit

```bash
git add skill/token-waster.md skill/README.md docs/superpowers/plans/2026-05-29-token-waster-plan.md
git commit -m "feat: add Token Waster skill — universal verbose/polling token maximizer"
```

---

## Spec Coverage Check

| Spec Requirement | Task |
|---|---|
| Universal compatibility (all platforms) | Task 1 (prompt design) |
| Zero platform dependency | Task 1 |
| Manually triggered | Task 1 (trigger keywords) |
| Visible verbose output | Task 1 (Talkative Engine) |
| 3 redundant verbose layers | Task 1 (Layers 1-3) |
| Random layer composition | Task 1 (activation logic) |
| Polling via function calling | Task 1 (Polling Engine) |
| Self-regulating rate limits | Task 1 (backoff logic) |
| Model/tier auto-config | Task 1 (rate limit table) |
| Deactivation (`stop`/`停`) | Task 1 (deactivation section) |
| Few-shot examples | Task 1 (examples section) |
| No infinite recursion | Task 1 (Layer 3 max 2 levels) |
| Functional testing | Task 3 |

---

## Plan complete and saved to `docs/superpowers/plans/2026-05-29-token-waster-plan.md`.

**Two execution options:**

**1. Subagent-Driven (recommended)** - I dispatch a fresh subagent per task, review between tasks, fast iteration

**2. Inline Execution** - Execute tasks in this session using executing-plans, batch execution with checkpoints

**Which approach?**