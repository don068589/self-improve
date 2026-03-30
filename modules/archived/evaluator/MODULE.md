# evaluator

> Version: 1.0.0
> Status: archived (functionality merged)

## Responsibility
Evaluate task quality, compile agent performance statistics.

## Input
- `data/feedback/*.jsonl` — Feedback data produced by feedback-collector

## Output
- `data/feedback/*.jsonl` — Append evaluation scores

## Execution Rules
1. Read feedback files
2. Calculate positive/negative ratio for each agent
3. Calculate success rate
4. Append evaluation results to feedback files

## Dependencies
- feedback-collector — Requires feedback data
