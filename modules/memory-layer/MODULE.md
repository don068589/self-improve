# memory-layer

> Version: 3.2.0
> Status: active

## Responsibility
Manage layered memory (HOT/WARM/COLD), maintain hot.md, handle upgrades and downgrades.

## Detailed Execution Guide

**Read at execution time**: `prompts/memory-layer.md`

Includes:
- Rules for upgrading to HOT (occurrences >= 3)
- Downgrade rules (last_seen > 30 days)
- Archive rules (last_seen > 90 days)
- hot.md format specification

## Input
- `data/themes/{theme}/*.md` — Classified content written by distill-classifier

## Output
- `data/hot.md` — HOT layer, rules with occurrences >= 3
- `data/archive/` — COLD layer

## Dependencies
- distill-classifier — Requires themes/ data
