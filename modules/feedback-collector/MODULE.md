# feedback-collector

> Version: 1.1.0
> Status: active

## Responsibility
Scan agent conversation logs, extract signals such as corrections, praises, insights, innovations, etc.

## Detailed Execution Guide

**Read at execution time**: `prompts/feedback-collector.md`

Includes:
- Semantic understanding extraction method
- Signal type recognition rules
- Multi-dimensional label judgment
- Summary/detail layered format
- Closed-loop input (reflections.md)

## Input
- `~/.openclaw/agents/*/memory/YYYY-MM-DD.md` — Recent 3 days of conversation logs
- `data/reflections.md` — Previous round reflection (closed-loop input)

## Output
- `data/feedback/YYYY-MM-DD.jsonl` — Structured feedback records

## Dependencies
None (base module)
