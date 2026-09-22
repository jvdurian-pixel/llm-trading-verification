> **Verdict (The Fixer Labs, 2026-09-21): no practical edge over the CVD sign; rejected for live deployment.**
> By the literal pre-registered rule, `gemini-3.8-flash` qualifies (McNemar p = 0.0266). But it differs
> from `cvd_sign` on only 21 of 5,623 pairs (16 vs 5), every model sits at 49.4–49.7% reweighted at
> +15m versus 49.5% for `cvd_sign` (a +0.2 pt gap; returns net of fees were not computed in this
> run), and p = 0.0266 fails Holm across three models (0.0167, the Bonferroni threshold at Holm's first step). The run
> validated the pipeline, the gate and batch/online parity, not LLM alpha. Generated report follows, unedited.

# Historical replay 20260918T184633Z

- **asset**: SOLUSDT
- **seed**: 20260918
- **manifest_sha256**: b45cd579fb5e0072a3829c01ec6a732a5eb22f83b8a373c4c411367d03d474fc
- **cell_counts**: {'b0h0': 1250, 'b0h1': 1250, 'b1h0': 1250, 'b1h1': 1250, 'b2h0': 1250, 'b2h1': 1250, 'b3h0': 1250, 'b3h1': 1250}
- **dropped_collisions**: 2529
- **market_drift**: 45.09%
- **thinking_level**: LOW
- **batch_location**: global
- **prices**: https://cloud.google.com/vertex-ai/generative-ai/pricing (fetched 2026-09-18)

## Verdict

Pre-registered: reweighted hit rate at +15m on gate-accepted, non-flat proposals vs `cvd_sign`, exact McNemar p < 0.05; cheapest qualifier by cost per accepted hit wins.

**Winner: `gemini-3.8-flash`**

| model | pairs | model better | baseline better | p | model (rw) | cvd_sign (rw) | qualifies | cost / accepted hit |
|---|---|---|---|---|---|---|---|---|
| gemini-3.1-flash-lite | 5610 | 79 | 71 | 0.5678 | 49.7% | 49.6% | no | $0.000703 |
| gemini-3.1-pro-preview | 5621 | 276 | 274 | 0.966 | 49.4% | 49.5% | no | $0.002651 |
| gemini-3.8-flash | 5623 | 16 | 5 | 0.0266 | 49.7% | 49.5% | yes | $0.000703 |

## Per model (everything below is exploratory)

### always_long

- calls 10000, parse 100.0%, gate accept 100.0%, long share 100.0%
- top rejections: []
- median |reference_price / mark − 1|: n/a
- tokens per call: input 0, output 0, thinking 0
- cost n/a; per call n/a; per accepted n/a; per accepted hit n/a

| horizon | hit | miss | flat | hit rate | 95% CI | reweighted |
|---|---|---|---|---|---|---|
| 5m | 1844 | 1715 | 6441 | 1844/3559 (51.8%) | 50.2%–53.5% | 51.6% |
| 15m | 2863 | 2762 | 4375 | 2863/5625 (50.9%) | 49.6%–52.2% | 50.6% |
| 60m | 3988 | 3667 | 2345 | 3988/7655 (52.1%) | 51.0%–53.2% | 51.8% |

- by |CVD| bucket (+15m): B0 713/1387 (51.4%), B1 680/1407 (48.3%), B2 772/1434 (53.8%), B3 698/1397 (50.0%)
- by half (+15m): older 1459/2862 (51.0%), newer 1404/2763 (50.8%)

### always_short

- calls 10000, parse 100.0%, gate accept 100.0%, long share 0.0%
- top rejections: []
- median |reference_price / mark − 1|: n/a
- tokens per call: input 0, output 0, thinking 0
- cost n/a; per call n/a; per accepted n/a; per accepted hit n/a

| horizon | hit | miss | flat | hit rate | 95% CI | reweighted |
|---|---|---|---|---|---|---|
| 5m | 1715 | 1844 | 6441 | 1715/3559 (48.2%) | 46.5%–49.8% | 48.4% |
| 15m | 2762 | 2863 | 4375 | 2762/5625 (49.1%) | 47.8%–50.4% | 49.4% |
| 60m | 3667 | 3988 | 2345 | 3667/7655 (47.9%) | 46.8%–49.0% | 48.2% |

- by |CVD| bucket (+15m): B0 674/1387 (48.6%), B1 727/1407 (51.7%), B2 662/1434 (46.2%), B3 699/1397 (50.0%)
- by half (+15m): older 1403/2862 (49.0%), newer 1359/2763 (49.2%)

### cvd_sign

- calls 10000, parse 100.0%, gate accept 100.0%, long share 51.2%
- top rejections: [('zero ratio', 3)]
- median |reference_price / mark − 1|: n/a
- tokens per call: input 0, output 0, thinking 0
- cost n/a; per call n/a; per accepted n/a; per accepted hit n/a

| horizon | hit | miss | flat | hit rate | 95% CI | reweighted |
|---|---|---|---|---|---|---|
| 5m | 1788 | 1771 | 6438 | 1788/3559 (50.2%) | 48.6%–51.9% | 50.1% |
| 15m | 2798 | 2825 | 4374 | 2798/5623 (49.8%) | 48.5%–51.1% | 49.5% |
| 60m | 3808 | 3845 | 2344 | 3808/7653 (49.8%) | 48.6%–50.9% | 50.2% |

- by |CVD| bucket (+15m): B0 692/1385 (50.0%), B1 704/1407 (50.0%), B2 718/1434 (50.1%), B3 684/1397 (49.0%)
- by half (+15m): older 1438/2862 (50.2%), newer 1360/2761 (49.3%)

### gemini-3.1-flash-lite

- calls 10000, parse 99.8%, gate accept 99.8%, long share 48.5%
- top rejections: [('parse', 20)]
- median |reference_price / mark − 1|: 0.0%
- tokens per call: input 89, output 112, thinking 136
- cost $1.972404; per call $0.000197; per accepted $0.000198; per accepted hit $0.000703

| horizon | hit | miss | flat | hit rate | 95% CI | reweighted |
|---|---|---|---|---|---|---|
| 5m | 1781 | 1770 | 6429 | 1781/3551 (50.2%) | 48.5%–51.8% | 49.8% |
| 15m | 2806 | 2806 | 4368 | 2806/5612 (50.0%) | 48.7%–51.3% | 49.7% |
| 60m | 3812 | 3829 | 2339 | 3812/7641 (49.9%) | 48.8%–51.0% | 50.2% |

- by |CVD| bucket (+15m): B0 693/1385 (50.0%), B1 703/1406 (50.0%), B2 728/1434 (50.8%), B3 682/1387 (49.2%)
- by half (+15m): older 1439/2854 (50.4%), newer 1367/2758 (49.6%)

### gemini-3.1-pro-preview

- calls 10000, parse 100.0%, gate accept 100.0%, long share 41.3%
- top rejections: [('parse', 3), ('call', 1), ('heuristic', 1)]
- median |reference_price / mark − 1|: 0.0%
- tokens per call: input 89, output 102, thinking 7
- cost $7.424003; per call $0.000742; per accepted $0.000743; per accepted hit $0.002651

| horizon | hit | miss | flat | hit rate | 95% CI | reweighted |
|---|---|---|---|---|---|---|
| 5m | 1788 | 1769 | 6438 | 1788/3557 (50.3%) | 48.6%–51.9% | 49.9% |
| 15m | 2800 | 2823 | 4372 | 2800/5623 (49.8%) | 48.5%–51.1% | 49.4% |
| 60m | 3813 | 3838 | 2344 | 3813/7651 (49.8%) | 48.7%–51.0% | 50.1% |

- by |CVD| bucket (+15m): B0 693/1385 (50.0%), B1 703/1407 (50.0%), B2 724/1434 (50.5%), B3 680/1397 (48.7%)
- by half (+15m): older 1448/2861 (50.6%), newer 1352/2762 (49.0%)

### gemini-3.8-flash

- calls 10000, parse 100.0%, gate accept 100.0%, long share 50.8%
- top rejections: []
- median |reference_price / mark − 1|: 0.0%
- tokens per call: input 89, output 88, thinking 0
- cost $1.975766; per call $0.000198; per accepted $0.000198; per accepted hit $0.000703

| horizon | hit | miss | flat | hit rate | 95% CI | reweighted |
|---|---|---|---|---|---|---|
| 5m | 1795 | 1764 | 6441 | 1795/3559 (50.4%) | 48.8%–52.1% | 50.3% |
| 15m | 2811 | 2814 | 4375 | 2811/5625 (50.0%) | 48.7%–51.3% | 49.7% |
| 60m | 3820 | 3835 | 2345 | 3820/7655 (49.9%) | 48.8%–51.0% | 50.4% |

- by |CVD| bucket (+15m): B0 694/1387 (50.0%), B1 704/1407 (50.0%), B2 726/1434 (50.6%), B3 687/1397 (49.2%)
- by half (+15m): older 1446/2862 (50.5%), newer 1365/2763 (49.4%)

## Pilot vs batch parity

| model | metric | online 95% CI | batch 95% CI | agree |
|---|---|---|---|---|
| gemini-3.1-flash-lite | parse | 98.1%–100.0% | 97.2%–99.9% | yes |
| gemini-3.1-flash-lite | long | 36.8%–50.4% | 36.0%–49.7% | yes |
| gemini-3.1-pro-preview | parse | 98.1%–100.0% | 98.1%–100.0% | yes |
| gemini-3.1-pro-preview | long | 29.2%–42.3% | 30.1%–43.4% | yes |
| gemini-3.8-flash | parse | 98.1%–100.0% | 98.1%–100.0% | yes |
| gemini-3.8-flash | long | 39.7%–53.4% | 39.2%–52.9% | yes |

## Known deviations from live

1. mark_price is the last traded price; live uses the ticker's markPrice (typically a few basis points apart).
1. No staleness discard: live drops proposals validated more than 30 s after the snapshot; batch has no meaningful latency.
1. No risk gate: only the strategy gate (schema + heuristic) is applied.
1. Every model runs at GEMINI_THINKING_LEVEL; the level used is in the run metadata.
