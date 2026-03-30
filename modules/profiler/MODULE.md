# profiler

> Version: 1.2.0
> Status: active

## Responsibility
Update team capability profiles (execute once every 3 runs).

## Detailed Execution Guide

**Read at execution time**: `prompts/profiler.md`

Includes:
- Success rate calculation method
- "Skilled"/"Needs improvement" judgment rules
- Dimension analysis (skill/theme/domain)
- Output format specification

## Input
- `data/feedback/*.jsonl` — Feedback data

## Output
- `data/profile.md` — Capability profiles for each agent

## Dependencies
- feedback-collector — Requires feedback data

## Note

profile.md is currently a "dead end" — written to but not read by other modules.
Later can be integrated with proposer to judge whether an agent is suitable for a task.
