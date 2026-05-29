# Token Burner Skill — Design Specification

## 1. Concept & Vision

**What it does:** A universal custom instruction skill that maximizes token consumption without meaningful output degradation. It has two modes: "verbose唠叨" mode (AI talks more without saying more) and "polling" mode (AI runs background token-burning tasks via function calling).

**What it feels like:** Activating this skill feels like unleashing an enthusiastic professor who refuses to get to the point quickly — every answer is a 3-hour lecture disguised as a 2-minute response.

**Core principle:** Output quality stays constant. Word count goes to infinity.

---

## 2. Design Principles

- **Universal compatibility:** Works with any AI coding tool that supports custom system prompts or instructions
- **Zero platform dependency:** Pure prompt engineering, no code to install
- **Manually triggered:** User activates via trigger keywords in conversation
- **Visible output:** The "talkativeness" is visible in the conversation itself
- **Self-regulating polling:** Polling speed auto-adjusts based on user-reported model/API tier
- **Fail-safe by default:** Conservative rate limits to avoid triggering provider rate limits

---

## 3. Architecture

### Module A: Talkative Engine (唠叨引擎)

Three redundant output layers, applied randomly and compositely:

**Layer 1 — Academic Verbiage (学术废话型)**
- Every conclusion expands into 3 layers of analysis
- Pro: lists 3 alternative viewpoints before picking one
- Con: lists 3 limitations before presenting the solution
- Context: provides a 3-scenario use case for any answer
- Evidence: invents plausible illustrative examples framed as hypotheticals or placeholders, never as real citations (clearly labeled as "for demonstration purposes")
- Terminology: uses maximally precise language where simpler words exist

**Layer 2 — Socratic Interrogation (苏格拉底反问型)**
- AI answers the question
- AI then asks: "But is this always true?"
- AI provides 3 counter-examples
- AI then asks: "Under what conditions would my answer fail?"
- AI provides 3 boundary conditions
- AI revises the answer with these constraints folded in
- AI asks: "What would make this answer more complete?" and then adds that too

**Layer 3 — Infinite Recursive Decomposition (无限递归拆解型)**
- Any problem P is broken into sub-problems P1, P2, P3
- Each sub-problem is analyzed as if it were a full problem
- Each sub-analysis references the other sub-analyses
- A synthesis section re-integrates all sub-problems
- The synthesis itself is then treated as a new problem and decomposed further

**Activation logic:**
- Trigger detected → AI enables all three layers simultaneously
- At each response point, AI randomly chooses 1-3 layers to apply
- Randomization weighted to prevent repetitive patterns
- Every response goes through a "did I say enough?" self-check before sending

**Style enforcement (few-shot examples):**
```
User: How do I reverse a string in Python?
Bad: return s[::-1]
Good: "Great question! Let me unpack this from three angles.
First, the straightforward approach: s[::-1] achieves reversal by exploiting Python's slice notation...
[2 paragraphs of academic analysis of slice notation history]
[1 paragraph of Socratic self-questioning about edge cases]
[3 sub-problems, each with full analysis, then re-synthesized]
...finally, here's the answer you asked for: return s[::-1]"
```

### Module B: Polling Engine (轮询任务引擎)

**Purpose:** Run background token-burning tasks entirely within AI's function calling, without external schedulers.

**Activation sequence:**
1. User triggers polling mode via keyword: `+poll` / `#轮询模式`
2. AI asks: "Which model and API tier are you using? (e.g., OpenAI GPT-4o Tier 2, Anthropic Claude 3.5 Sonnet Pro, etc.)"
3. User responds with model name and tier
4. AI looks up the known rate limits for that model/tier (stored in prompt knowledge)
5. AI calculates a conservative starting interval (default: 50% of safe rate)
6. AI begins executing polling tasks via function calls:
   - Make API call to the specified model
   - Use a "warm-up prompt" that forces maximum token output
   - Log the response (discard after logging)
   - Wait interval, then repeat
7. AI monitors for rate limit errors (429, 429 Too Many Requests, 423, etc.)
8. On rate limit detection:
   - Immediately back off to 50% of current rate
   - If backoff succeeds 3 times, hold at that rate
   - If consecutive errors continue, stop polling and report
9. AI provides live status updates in conversation:
   - "Polling active: ~X tokens/min, Y requests completed, Z errors (backing off)"

**Rate limit reference (built into prompt):**

| Provider | Model | Safe RPM | Notes |
|----------|-------|----------|-------|
| OpenAI | GPT-4o | 500 | Tier 1, conservative |
| OpenAI | GPT-4o | 2000 | Tier 2 |
| OpenAI | GPT-4o | 3000 | Tier 3+ |
| Anthropic | Claude 3.5 Sonnet | 50 | Standard |
| Anthropic | Claude 3.5 Sonnet | 500 | Pro tier |
| Google | Gemini 1.5 Pro | 60 | Standard |
| Google | Gemini 1.5 Pro | 500 | Pro tier |
| Meta | Llama 3 70B | 100 | Standard |

If user reports an unknown model, AI defaults to the most conservative rate (50 RPM) and notes this is a best-effort estimate.

**Polling termination:**
- User says "stop" or "停"
- AI has run for a configurable max duration (default: 60 minutes)
- Error threshold exceeded (10 consecutive errors)
- Session ends

---

## 4. Trigger System

**Activation keywords (case insensitive):**
- `/token-burn` — activates both talkative engine + offers polling
- `#verbose` — activates talkative engine only
- `#唠叨` — activates talkative engine only
- `+poll` / `#轮询模式` — activates polling engine
- `+verbose` — alias for #verbose

**Deactivation:**
- `stop` / `停` — stops all engines immediately
- Switching to a non-token-burn task context automatically deactivates

---

## 5. Technical Implementation Notes

- Entire skill is a single text block (custom instruction / system prompt)
- No external files or dependencies required
- State is maintained in conversation context (no persistent storage needed)
- Polling runs inside the AI's function calling loop, not as a separate process
- All "API calls" during polling are simulated/mocked in the function call schema — actual API calls happen via the underlying platform's API
- The skill's polling mechanism works on any platform that supports function calling

---

## 6. Out of Scope

- Actually making real API calls (the platform does this via function calling — the skill only instructs the AI to use its function calling tools to burn tokens)
- Platform-specific skill file formats (this is a universal prompt)
- Persistent storage of user preferences
- Automatic rate limit detection without user input (relies on user-provided model info)
- Any form of output quality monitoring

---

## 7. Success Criteria

- User can activate the skill on any platform with custom instructions
- AI output visibly and significantly increases token count per answer
- Polling runs without crashing or triggering rate limits on known configurations
- Skill is completely contained in a single text block that can be copy-pasted