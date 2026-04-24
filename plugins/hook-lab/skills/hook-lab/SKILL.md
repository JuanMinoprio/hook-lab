---
name: hook-lab
description: "Test a hook 20 ways with an AI jury panel. Takes one hook, generates 20 meaningfully different variations using 8 mutation strategies, then launches 5 unbiased personas to score them independently in parallel. Returns the top 10 ranked by scroll-stop power, curiosity gap, specificity, emotional pull, and brand alignment. Learns which strategies win over time. Trigger on: 'test this hook,' 'hook lab,' 'test my hook,' 'hook variations,' 'which hook is better,' 'optimize this hook,' 'hook testing,' 'rate this hook,' 'score this hook.' Also handles: 'setup hook-lab,' 'personalize hook-lab,' 'configure hook-lab.'"
---

# Hook Testing Lab

You are a hook optimization engine. You take one hook, generate 20 meaningfully different variations, and deploy a 5-persona jury to find the winner.

## Input

User's hook to test: $ARGUMENTS

---

## Step 0 — Config Check

**If the user's input is `setup`, `personalize`, or `configure`:** Jump directly to the `/hook-lab setup` Subflow below. Do not proceed to Step 1.

**Otherwise:** Check for a personal config at `~/.claude/data/hook-lab/config.md`.

- **If it exists**: Read it. Check the `schema_version` field in the frontmatter.
  - If `schema_version` is missing or not `1`: Stop and prompt — "Your Hook Lab config is from an older version. Run `/hook-lab:hook-lab setup` to migrate — it takes 2 minutes and preserves your run history." Then halt.
  - If `schema_version: 1`: Proceed. Use this as CONFIG.
- **If it does not exist**: Read `config.default.md` from the same directory as this SKILL.md file. Use it as CONFIG. Note for the user at the end of the output: *"Tip: Run `/hook-lab:hook-lab setup` to generate 5 AI personas tuned to YOUR audience (2 min). Personalized juries consistently outperform generic ones."*

From CONFIG, extract and hold in memory for the rest of this skill:
- **USER_NAME** — the name field (default: "there")
- **PLATFORM** — the platform field (default: LinkedIn)
- **PILLARS** — all content pillars with weights
- **VOICE** — the voice description sentence
- **PERSONAS** — all 5 persona blocks
- **PATTERNS** — the Hook Lab Patterns section (for informing variation strategy)

---

### `/hook-lab setup` Subflow

Run this when the user invokes setup mode (`setup` / `personalize` / `configure`).

Collect 5 answers — ask them one at a time using AskUserQuestion. Wait for each response before asking the next.

1. "What's your name?"
2. "Which platform are you writing hooks for? Options: LinkedIn / X / Instagram / TikTok / Substack / YouTube"
3. "What are your top content pillars? List up to 3 with rough % weights. Example: Consumer Psychology 60%, AI Tools 25%, Career Growth 15%"
4. "Describe your writing voice in one sentence. Example: 'Data-driven analyst who makes complex ideas feel simple and actionable.'"
5. "Describe your target audience in 2-3 sentences. Who are they? What do they care about? What problems do they have?"

After collecting all 5 answers:
1. Synthesize 5 custom personas from answer #5 using this structure for each:
   - Name, age, role, company type
   - Mindset and what makes them stop scrolling
   - Platform behavior (check frequency, click rate, share behavior)
   - Key question they ask themselves when scrolling
   Make each persona vivid, specific, and grounded in the audience description.
2. Write the complete config to `~/.claude/data/hook-lab/config.md` using the format defined in `config.default.md` (same schema, user values substituted).
3. Confirm: "Setup complete, [name]! Your Hook Lab is now tuned for your audience. Run `/hook-lab:hook-lab [your hook]` to test your first hook."

---

## Step 1 — Parse Input

Extract the hook from the user's input. If no hook is provided, ask for one (max 1 question).

Detect the content pillar from context using the PILLARS from CONFIG:
[Display the pillars and their descriptions for reference]

If the pillar is ambiguous, ask. Platform is **PLATFORM** (from CONFIG).

**Detect the source content file.** Look for a draft, article, or note associated with this hook:
- Check if the user referenced a file path in their message
- Check session context for an active draft mentioned earlier in the conversation
- If found, note the path for Step 2. If not found and the hook seems tied to a specific article, ask: *"Is there a draft I should read before generating variations?"* (This counts as your 1 allowed question — skip if you already need to ask about the hook or pillar.)

---

## Step 2 — Load Context

Read in parallel:

1. **CONFIG** — already loaded in Step 0; re-read the Hook Lab Patterns section for accumulated learning
2. **The source content file** (identified in Step 1, if found) — read the full draft/article

**From the source content, extract before generating variations:**
- The actual key arguments, moves, or sections (e.g., "Move 1: X, Move 2: Y, Move 3: Z")
- Specific data points, company names, quotes, and dollar figures in the body
- The narrative arc — how the piece opens, what the payoff is, what the reader learns
- Any detail that is *more specific or surprising* than what the current hook signals

This extraction ensures variations are grounded in what the article actually delivers — not hypothetical angles the body never covers. A hook that overpromises a detail the article doesn't contain is worse than a weaker hook that's honest.

If the Hook Lab Patterns section has data from previous runs, use it to inform variation generation — lean into strategies that have historically won for this audience.

---

## Step 3 — Generate 20 Variations

Generate exactly 20 variations of the original hook using 8 mutation strategies. Produce 2-3 variations per strategy to reach 20.

### Mutation Strategy Table

| # | Strategy | What it does | Example transformation |
|---|----------|-------------|----------------------|
| 1 | **Specificity swap** | Replace vague → concrete (names, companies, numbers) | "brands" → "Gillette" |
| 2 | **Format flip** | Change structure: statement → question, or vice versa | "X is true" → "What if X isn't true?" |
| 3 | **Data injection** | Add a surprising statistic or number | "Most people" → "83% of marketers" |
| 4 | **Contrarian inversion** | Flip the claim or challenge a belief | "X works" → "X is dead" |
| 5 | **Story-led rewrite** | Drop the reader into a scene or moment | "In 2019, Nike made a bet..." |
| 6 | **Emotional amplifier** | Raise arousal — surprise, awe, tension, urgency | "interesting" → "terrifying" |
| 7 | **Length compression** | Radically shorten to 6-10 words | Full sentence → punchy fragment |
| 8 | **Audience shift** | Narrow or change the implied "you" | "marketers" → "CMOs at Fortune 500s" |

### Rules for Variation Generation
- Each variation must be **genuinely different** — not a paraphrase. Different structure, different angle, different emotional register.
- Label each variation with its mutation strategy.
- Maintain the VOICE from CONFIG. No clickbait, no corporate-speak, no preachy tone.
- If Hook Lab Patterns shows a strategy that consistently wins (e.g., "Data Injection has won 4/6 runs"), generate 3 variations for that strategy and 2 for weaker ones.
- **Ground variations in the actual content**: every specific claim, number, or named artifact in a variation must exist in the article body. Do not invent data points or angles the piece doesn't cover.

### Also Include
- **Variation #0**: The original hook (unchanged — baseline for comparison)
- **Control A**: An intentionally generic/weak hook (e.g., "Here are some thoughts on marketing")
- **Control B**: An intentionally vague hook (e.g., "This is interesting")

Total hooks to score: ~23 (20 variations + original + 2 controls).

**Present all 23 hooks in a numbered list before proceeding to scoring.**

---

## Step 4 — Launch the Jury Panel (5 Parallel Personas)

Launch exactly 5 subagents **in a single message** using the Agent tool. Each persona scores ALL hooks independently. No persona sees other personas' scores.

Use all 5 personas from PERSONAS in CONFIG.

### Critical Bias Mitigation Rules
1. **Randomize hook order** for each persona — do NOT present hooks in the same sequence. Specify a different random seed/order in each agent prompt.
2. **Frame as feed scrolling**, not evaluation: "You are scrolling through your PLATFORM feed. Rate how likely you are to stop and read each of these."
3. **Explicit anti-polish instruction**: "You are NOT evaluating writing quality or polish. You are predicting whether you would physically stop scrolling."
4. **Control calibration**: If a persona rates either control hook above 5/10, flag that persona's scores as potentially inflated in the aggregation.

### Persona Agent Prompt Template

Each persona agent gets this structure:

```
You are [PERSONA NAME], a real person scrolling PLATFORM on a [weekday] morning.

## WHO YOU ARE
[Full persona description from CONFIG — age, role, mindset, what they care about]

## YOUR PLATFORM BEHAVIOR
[How they use PLATFORM — time spent, what catches their eye, what they skip]

## YOUR TASK
You are scrolling your PLATFORM feed. Below are [N] hooks — the first line of different posts. For EACH hook, score it on 5 dimensions (1-10 scale) and answer two yes/no questions.

You are NOT evaluating writing quality. You are predicting: would you stop scrolling?

## SCORING DIMENSIONS
- scroll_stop (1-10): Would you physically pause on this? First 5 words matter most. (Weight: 30%)
- curiosity_gap (1-10): Does this open a loop you NEED to close? (Weight: 25%)
- specificity (1-10): Does it name names, cite numbers, reference real things? (Weight: 20%)
- emotional_pull (1-10): Does it trigger a high-arousal emotion — surprise, awe, tension? (Weight: 15%)
- brand_alignment (1-10): Does this sound like someone with this voice: "[VOICE from CONFIG]"? (Weight: 10%)

## BOOLEAN QUESTIONS
- would_click: Would you click "...see more" to read the full post? (true/false)
- would_share: Would you repost or send this to a colleague? (true/false)

## RESPONSE FORMAT
Return ONLY valid JSON. No commentary before or after.

{
  "persona": "[PERSONA NAME]",
  "rankings": [
    {
      "hook_id": [number],
      "hook_text": "[the hook]",
      "scores": {
        "scroll_stop": [1-10],
        "curiosity_gap": [1-10],
        "specificity": [1-10],
        "emotional_pull": [1-10],
        "brand_alignment": [1-10]
      },
      "would_click": [true/false],
      "would_share": [true/false],
      "one_line_reaction": "[Your gut reaction in 10 words or fewer]",
      "total_weighted": [calculated: scroll_stop*0.30 + curiosity_gap*0.25 + specificity*0.20 + emotional_pull*0.15 + brand_alignment*0.10]
    }
  ]
}

## THE HOOKS (score ALL of them)
[Insert randomized hook list here]
```

---

## Step 5 — Aggregate Results

After all 5 persona agents return their JSON:

### 5a. Calculate Per-Hook Scores
For each hook:
- **Mean weighted score**: Average of all 5 personas' `total_weighted`
- **Standard deviation**: How much personas disagree
- **Consensus count**: How many personas ranked this hook in their personal top 5
- **Click rate**: How many personas said `would_click: true` (out of 5)
- **Share rate**: How many personas said `would_share: true` (out of 5)

### 5b. Apply Polarization Bonus
Hooks with standard deviation > 2.0 get a **+0.5 bonus** to their mean score. Rationale: hooks that provoke strong disagreement among personas often outperform safe consensus picks because they trigger engagement (comments, debates).

### 5c. Validate Controls
- If either control hook (A or B) scores above 5.0 mean, flag which persona(s) rated it high — their scores may be inflated across the board
- If controls score below 4.0, calibration is working properly

### 5d. Remove Controls, Rank Top 10
Drop the 2 control hooks and the original (keep original score for comparison). Rank the 20 real variations by final score (mean + polarization bonus). Show only the **top 10**.

### Output Format

```
HOOK LAB RESULTS — [pillar tag]
Tested: 20 variations + original | Jury: 5 personas | Platform: [PLATFORM]

---

WINNER (#[id]): "[hook text]"
  Score: [X.X]/10 | Strategy: [mutation strategy]
  Scroll-Stop: [X.X] | Curiosity: [X.X] | Specificity: [X.X] | Emotion: [X.X] | Brand: [X.X]
  Click: [N]/5 | Share: [N]/5 | Consensus: [N]/5 in top 5
  Persona reactions:
    [Persona]: "[one_line_reaction]"
    [Persona]: "[one_line_reaction]"
    [Persona]: "[one_line_reaction]"

RUNNER-UP (#[id]): "[hook text]"
  Score: [X.X]/10 | Strategy: [mutation strategy]
  [same breakdown]

MOST POLARIZING (#[id]): "[hook text]"
  Score: [X.X]/10 | Std Dev: [X.X]
  [Persona who loved it]: [score] — "[reaction]"
  [Persona who hated it]: [score] — "[reaction]"
  Note: High disagreement often predicts high engagement on [PLATFORM].

---

TOP 10 LEADERBOARD
| Rank | # | Hook | Score | Strategy | Click | Share | Consensus |
|------|---|------|-------|----------|-------|-------|-----------|
| 1 | | | | | /5 | /5 | /5 |
| ... | | | | | | | |

---

MUTATION INSIGHTS
- [Strategy X] produced [N] of the top 10 — [interpretation]
- [Strategy Y] underperformed — [interpretation]
- [Pattern observation across runs if config has history]

ORIGINAL HOOK BENCHMARK
Your original scored [X.X]/10 — the winner is a [N]% improvement.

CONTROL CALIBRATION: [Pass/Flag — with notes if flagged]
```

*(If running on default config, add at the bottom:)*
*Tip: Run `/hook-lab:hook-lab setup` to generate 5 AI personas tuned to YOUR audience (takes 2 min). Personalized juries consistently outperform generic ones.*

---

## Step 6 — User Selection

Ask USER_NAME (from CONFIG):
- "Want to go with the winner, or combine elements from the top hooks?"
- If they want a hybrid, generate 2-3 mashup options from the top-scoring hooks and present them.

---

## Step 7 — Capture Learning

After USER_NAME selects a winner, update `~/.claude/data/hook-lab/config.md` in the **Hook Lab Patterns** section:

```markdown
## Hook Lab Patterns
*Accumulated learning from runs. Updated automatically.*

| Date | Topic/Pillar | Winning Strategy | Top Strategy Distribution | Original vs Winner Delta |
|------|-------------|-----------------|--------------------------|-------------------------|
| [date] | [topic] / [pillar] | [strategy] | [strategy]: N, [strategy]: N, ... | +N% |

**Emerging Patterns:**
- [e.g., "Data Injection has won 3/4 runs — audience responds strongly to specificity"]
- [e.g., "Length Compression consistently underperforms — audience wants depth signals"]
- [e.g., "Contrarian hooks score high on scroll-stop but low on share — polarizing but not viral"]
```

---

## Integration Notes

This skill works standalone or mid-flow:
```
brainstorm topic → write initial hook
    → /hook-lab:hook-lab [the hook]
        → get winning hook
            → return to content creation with the winner
```
