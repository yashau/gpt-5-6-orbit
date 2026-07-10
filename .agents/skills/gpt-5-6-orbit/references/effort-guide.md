# GPT-5.6 effort guide

Use this reference to choose effort levels, explain routing cost, or evaluate an escalation. Treat it as evidence for defaults, not as a promise about a particular phase.

## DeepSWE snapshot

Source: [DeepSWE v1.1 all-effort-levels leaderboard](https://deepswe.datacurve.ai/), observed July 10, 2026; leaderboard updated July 9, 2026. The benchmark contains 113 original long-horizon engineering tasks. All models use the same mini-swe-agent harness.

| Model | Effort | Pass@1 | Avg cost | Output tokens | Steps |
| --- | --- | ---: | ---: | ---: | ---: |
| Sol | `low` | 45% ±2% | $1.07 | 11k | 23 |
| Sol | `medium` | 61% ±2% | $1.86 | 18k | 31 |
| Sol | `high` | 69% ±1% | $3.47 | 28k | 37 |
| Sol | `xhigh` | 71% ±1% | $4.70 | 41k | 44 |
| Sol | `max` | 73% ±3% | $8.39 | 60k | 61 |
| Terra | `low` | 24% ±1% | $0.43 | 8.6k | 21 |
| Terra | `medium` | 35% ±3% | $0.58 | 12k | 25 |
| Terra | `high` | 54% ±4% | $1.13 | 22k | 34 |
| Terra | `xhigh` | 60% ±2% | $2.13 | 40k | 43 |
| Terra | `max` | 70% ±3% | $4.95 | 72k | 76 |
| Luna | `low` | 2% ±1% | $0.07 | 3.1k | 12 |
| Luna | `medium` | 11% ±1% | $0.22 | 8.2k | 24 |
| Luna | `high` | 44% ±3% | $0.78 | 26k | 49 |
| Luna | `xhigh` | 57% ±2% | $1.54 | 45k | 71 |
| Luna | `max` | 67% ±4% | $3.03 | 73k | 102 |

## Price/performance knees

- **Sol:** `medium` is the value default. `high` buys eight points for $1.61 more and is justified by ambiguity or high downstream cost. `xhigh` adds only two points; `max` adds another two for a large premium.
- **Terra:** `high` is the strongest default jump, adding nineteen points over `medium` for $0.55. `xhigh` is a selective escalation, not a routine default.
- **Luna:** `high` is the long-horizon knee. Use lower effort for genuinely deterministic, much smaller phases; the benchmark is not evidence that a single exact command needs high reasoning.
- Sol `xhigh` (71%, $4.70) slightly outperforms and costs less than Terra `max` (70%, $4.95). Prefer a Sol diagnosis plus a focused Terra correction when a hard implementation route fails.

## Interpretation rules

1. Compare configurations directionally; confidence intervals overlap at the frontier.
2. Do not add benchmark costs to predict relay cost. Each benchmark number covers a complete task, while Orbit children receive smaller phases.
3. Do not apply long-horizon pass rates literally to planning-only or command-only phases.
4. Do not infer an `ultra` result. DeepSWE does not show Ultra in this chart.
5. Recheck the live leaderboard before changing durable defaults because prices and evaluations can change.
