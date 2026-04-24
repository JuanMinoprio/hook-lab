# Hook Lab — Example Run

This is an annotated example showing what a typical Hook Lab run looks like.

---

## Input

```
/hook-lab:hook-lab Anthropic just revealed the three moves that make Claude so different from every other AI model.
```

---

## Step 1 — Parse + Context

Hook Lab detects: Content pillar = AI & Productivity. Platform = LinkedIn (from default config).

---

## Step 2 — 20 Variations Generated

```
#0 (Original): Anthropic just revealed the three moves that make Claude so different from every other AI model.

Specificity Swap:
#1: Anthropic just revealed the three product decisions that made Claude the #2 AI by market share.
#2: Anthropic's leaked strategy doc reveals why Claude outperforms GPT-4 on 6 of 8 benchmarks.
#3: Three internal Anthropic principles explain why Claude feels fundamentally different from every other chatbot.

Format Flip:
#4: What makes Claude feel different from every other AI? Anthropic just answered that.
#5: Why does Claude feel smarter than GPT on nuanced tasks? Anthropic finally explained.

Data Injection:
#6: Claude now holds 18% of the AI assistant market. Anthropic revealed the 3 decisions that got them there.
#7: 3 product moves. 1.8B users later. Anthropic just explained their strategy.
#8: In 18 months, Claude went from unknown to #2. Here's the 3-move playbook Anthropic used.

Contrarian Inversion:
#9: Everyone is copying OpenAI. Anthropic made 3 deliberate choices NOT to.
#10: The AI race is about parameters and compute. Anthropic built Claude to compete differently.
#11: Claude isn't trying to beat GPT-4 at its own game. Here's what Anthropic is doing instead.

Story-Led:
#12: In 2023, Anthropic made a bet that every AI company called wrong. It's paying off now.
#13: The Anthropic team spent 3 years arguing about one principle. Claude's behavior is the result.
#14: I read Anthropic's full model spec so you don't have to. Here's what's actually different.

Emotional Amplifier:
#15: Anthropic quietly built the most honest AI model in the world. Most people still haven't noticed.
#16: The three decisions Anthropic made that no one else was willing to make.
#17: This changes how I think about every AI tool I've recommended in the last year.

Length Compression:
#18: Anthropic just explained why Claude is different. (It's not the model size.)
#19: 3 moves. That's why Claude feels different.

Audience Shift:
#20: If you're a marketer using AI tools, Anthropic's Claude strategy reveals something about your own product.
```

> Note: The tool also generates two "control" hooks — deliberately bland versions used to calibrate the scoring. If both controls score below 3.5 across all personas, the scoring system is working correctly. Controls are not shown in the leaderboard.

---

## Step 3 — Jury Scores (top 10)

| Rank | # | Hook | Score | Strategy | Click | Share |
|------|---|------|-------|----------|-------|-------|
| 1 | 9 | Everyone is copying OpenAI. Anthropic made 3 deliberate choices NOT to. | 8.7 | Contrarian | 5/5 | 4/5 |
| 2 | 12 | In 2023, Anthropic made a bet... | 8.4 | Story-Led | 5/5 | 3/5 |
| 3 | 6 | Claude now holds 18% of the AI assistant market... | 8.1 | Data Injection | 4/5 | 3/5 |
| 4 | 14 | I read Anthropic's full model spec so you don't have to... | 7.9 | Story-Led | 5/5 | 4/5 |
| 5 | 10 | The AI race is about parameters and compute... | 7.7 | Contrarian | 4/5 | 2/5 |
| 6 | 16 | The three decisions Anthropic made that no one else was willing to make. | 7.5 | Emotional | 4/5 | 3/5 |
| 7 | 1 | Anthropic just revealed the three product decisions... | 7.3 | Specificity | 4/5 | 2/5 |
| 8 | 15 | Anthropic quietly built the most honest AI model in the world... | 7.2 | Emotional | 3/5 | 3/5 |
| 9 | 8 | In 18 months, Claude went from unknown to #2... | 7.0 | Data Injection | 4/5 | 2/5 |
| 10 | 13 | The Anthropic team spent 3 years arguing about one principle... | 6.9 | Story-Led | 3/5 | 2/5 |

---

## Winner: #9

```
WINNER (#9): "Everyone is copying OpenAI. Anthropic made 3 deliberate choices NOT to."
  Score: 8.7/10 | Strategy: Contrarian Inversion
  Scroll-Stop: 9.1 | Curiosity: 8.8 | Specificity: 7.2 | Emotion: 9.0 | Brand: 8.3
  Click: 5/5 | Share: 4/5 | Consensus: 5/5 in top 5

  Persona reactions:
    Maya: "Finally — a contrarian take that's actually specific. 'NOT to' is doing a lot of work here."
    Raj: "I want to know what the 3 choices were. Immediately."
    Diane: "Stops my scroll in 0.8 seconds. That's the bar."
    Alex: "Would prefer a number. But the emotional charge earns it."
    Priya: "Would add to swipe file. Perfect contrarian structure."
```

---

## Rewrite Strategy Insights

```
REWRITE STRATEGY INSIGHTS
- Contrarian Inversion produced 2 of the top 5 — this audience responds to "everyone does X, we did Y" framing
- Story-Led also strong (2 of top 5) — scene-setting earns curiosity before the claim
- Data Injection hit top 3 with the market share hook — specificity validates the claim
- Length Compression underperformed — audience wants depth signals, not fragments
- Control calibration: PASS — both controls scored below 3.5 across all personas

ORIGINAL HOOK BENCHMARK
Your original scored 6.2/10 — winner is a 40% improvement.
```

---

*This is a realistic example run. Your results will vary based on your hook, your content, and your configured personas.*
