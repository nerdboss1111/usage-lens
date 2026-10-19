# Estimation Heuristics

Usage Lens estimates per-category usage because no LLM platform exposes per-task
consumption through a consumer API. This document shows every formula so you can
audit, calibrate, or override the numbers.

All estimates are labeled as estimates in the UI. They are starting points, not
measurements. If you know your real numbers, enter them directly.

## Constants

| Constant | Value | Source |
|----------|-------|--------|
| Spark rate | 1.9 credits per response | Measured from Muse usage panel deltas |
| Wonder rate | 6.1 credits per response | Measured from Muse usage panel deltas |
| Spark-to-Wonder ratio | 3.2x | 6.1 / 1.9 |
| Tokens per credit | ~400 | Heuristic; varies by model and task |
| Avg tokens per chat message | ~760 | Heuristic for context-tax math |

These are rough. Platform pricing changes, model routing is opaque, and token
counts are inferred, not observed. Calibrate against your own usage panel.

## Per-automation weekly cost

```
credits_per_week = runs_per_week * (tokens_per_run / TOKENS_PER_CREDIT) * (model_rate / 1.9)
```

Where:
- `runs_per_week` is how often the automation fires
- `tokens_per_run` is your estimate of input + output tokens per run
- `model_rate` is 1.9 for Spark-class models, 6.1 for Wonder-class
- Dividing by 1.9 normalizes to the Spark baseline

Example: a daily briefing (7 runs/week) using ~8,000 tokens per run on Spark:
`7 * (8000 / 400) * (1.9 / 1.9) = 7 * 20 * 1 = 140 credits/week`

The same job on Wonder: `7 * 20 * (6.1 / 1.9) = 449 credits/week`

## Per-chat context tax

```
credits_per_week = (messages_in_history * AVG_TOKENS_PER_MSG * calls_per_week) / TOKENS_PER_CREDIT
```

This models the cost of re-reading conversation history on every call. Long
side chats with big histories are the silent killer: the model re-processes
the full context window each time.

Example: a side chat with 240 messages, called 12 times per week:
`(240 * 760 * 12) / 400 = 5,472 credits/week`

That single chat can exceed an entire automation fleet. Archive or summarize
bloated chats to cut this number.

## Top-line usage

The headline percentage and credit count come from YOUR entered numbers:
```
used_credits = weekly_limit * (usage_percent / 100)
```

Nothing is inferred here. Read your platform's usage panel and type in the
numbers.

## Calibration

To calibrate the heuristics for your setup:

1. Note your usage percentage now.
2. Disable or pause one automation for a full week.
3. Note the new percentage.
4. The difference is that automation's real weekly cost.

File a GitHub issue with your findings. Real-world calibration data makes the
heuristics better for everyone.

## What the estimates cannot do

- They cannot see inside the platform's accounting. If the platform changes
  how it counts, the heuristics drift.
- They cannot measure background processes you forgot to list. The inventory
  is only as complete as your input.
- They are not a substitute for the platform's own usage panel. Always cross-check
  the top-line numbers.
