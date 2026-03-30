# proposer

> Version: 1.2.0
> Status: active

## Responsibility
Comprehensive consideration, judge output channel, generate solidification suggestions or deep outputs.

## Detailed Execution Guide

**Read at execution time**: `prompts/proposer.md`

Includes:
- Value-first judgment method
- Multi-angle utilization of single content
- Meta-ability guidance
- Output channel distribution rules
- High-value marking standards
- PENDING.md format specification

## Input
- `data/themes/` — Primary source
- `data/hot.md` — Rule solidification special
- `data/errors/` — Re-processing check
- `data/lessons/` — Re-processing check

## Output
- `proposals/PENDING.md` — Pending approval suggestions
- `drafts/` — Blog drafts
- `/path/to/learned/` — Methodologies, etc.
- `data/errors/` — Error knowledge points
- `data/lessons/` — Lessons learned
- `data/high-value/` — High-value markers

## Dependencies
- distill-classifier — Requires themes/
- memory-layer — Requires hot.md
