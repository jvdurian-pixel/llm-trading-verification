# We asked three Gemini models to trade 10,000 market snapshots. None beat a one-line rule.

AI trading agents are everywhere. Very few are tested honestly before they touch real money.
We tested ours, and it failed. Here is what we did, what we found, and why the test matters
more than the model.

## The question

Our execution engine, Cordis Core, has an LLM "producer": it shows a model the recent order
flow and asks for a trade proposal (direction, size, price) as JSON. Before paying for it in
production, we wanted to know one thing:

**Does the model add anything over the simplest rule it could be replaced with?**

That rule is `cvd_sign`: go long if more volume hit the ask than the bid over the window, short
otherwise. It costs nothing and runs in microseconds.

## The test

- **Data:** 90 days of Bybit SOLUSDT public trades (44.3M trades), rebuilt into 129,539
  one-minute snapshots through the same code the live engine uses.
- **Sample:** 10,000 snapshots, stratified by order-flow strength and by older/newer half of
  the window, then reweighted back to real-world frequencies. The same 10,000 went to every
  model, so the comparisons are paired.
- **Models:** `gemini-3.1-flash-lite`, `gemini-3.8-flash`, `gemini-3.1-pro-preview`, all at
  LOW thinking, all through Vertex AI batch. 30,000 calls in total, plus a 600-call live pilot
  to confirm batch and live behave the same (they did: every parity check agreed).
- **Scoring:** did price move the proposed way 15 minutes later? Every proposal also had to pass
  the same schema and sanity gate the live engine uses.
- **The rule was written down before any results existed.** A model qualifies only if it beats
  `cvd_sign` on the same snapshots with an exact McNemar test at p < 0.05, and its reweighted hit
  rate is higher.

## The result

| | Hit rate at +15 min (reweighted) | Cost per 10,000 calls |
|---|---|---|
| gemini-3.1-flash-lite | 49.7% | $1.97 |
| gemini-3.8-flash | 49.7% | $1.98 |
| gemini-3.1-pro-preview | 49.4% | $7.42 |
| `cvd_sign` (free rule) | 49.5% | $0 |
| always long | 50.6% | $0 |

Every model sat within half a point of a coin flip, and of the free rule. Simply always going
long did best, which is what a market that drifted up 45% over the window would predict.

## The trap we nearly fell into

By the letter of our own rule, `gemini-3.8-flash` **qualified**: p = 0.027. A less careful team
ships that.

Look closer. On the 5,623 snapshots where both it and the rule made a directional call, the
model disagreed with the rule only **21 times** (right 16, wrong 5). On the other 99.6%, it
*was* the rule. And with three models tested, a correction for multiple comparisons (Holm, strictest
threshold 0.0167) rejects it.

The model hadn't found an edge. It had learned to copy the input we gave it, and a small-sample
coincidence made the copy look significant. We rejected it for live trading.

## Why it happened

Our prompt gave the model two numbers: the order-flow ratio and the price. With nothing else to
reason about, the best any model can do is turn that ratio into a direction, which is exactly what
the free rule already does. **A model cannot know more than you tell it.** More compute (Pro cost
3.8x Flash) bought nothing.

## What we changed

Before the next test, the rule gets stricter, again written down before any results:

1. **Multiple-testing control** (Holm) across all candidate models.
2. **A disagreement floor:** a model must disagree with the baseline at least ~260 times before it
   is tested at all, so copies can't qualify.
3. **Net return after fees,** not just direction: positive expected return after 0.05% per leg plus
   slippage.
4. **The free rule is the permanent control** for every forward test.

## Limits, stated plainly

One asset, one 90-day window, one prompt, one model family, 15-minute horizon. This says nothing
about other assets, richer prompts, or other model families. It says this configuration has no
edge. That is a narrower claim than "LLMs can't trade", and it is the one the data supports.

## The point

The model was the cheap part: the full run cost about US$14 in compute. The valuable part was the
harness that refused to be fooled, including by our own rule.

---

### Test your strategy before you fund it

Before putting capital behind an LLM-driven strategy or trading agent, find out whether it has an
edge or is just copying a free rule.

We run the same **Strategy Verification Audit** on your strategy, replayed on historical exchange
trade data:
- paired against free baselines (the order-flow rule, always-long, always-short)
- a decision rule fixed before any results, with multiple-testing control
- live-vs-batch parity checked
- an honest report either way

Currently supported: crypto perpetuals on Bybit trade data, minute-level to hourly decisions.

- **Direct:** [@FIXERLABS](https://x.com/FIXERLABS) on X · [jvdurian-pixel](https://github.com/jvdurian-pixel) on GitHub

---

## In this repository

- [`report/2026-09-18-replay-report.md`](report/2026-09-18-replay-report.md): the full generated
  report, with per-model hit/miss/flat counts at 5, 15 and 60 minutes, per-bucket and per-half
  splits, Wilson intervals, the McNemar table and pilot-vs-batch parity.

The engine, the replay harness and the prompt are not published here.

Text and tables: [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/). © 2026 The Fixer Labs.
