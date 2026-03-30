# distill-classifier

> Version: 1.0.0
> Status: active
> Merged from: distill + classifier

## Responsibility

Distill three-layer rules from signals, classify and solidify, annotate value density.

## Detailed Execution Guide

**Read at execution time**: `prompts/distill-classifier.md`

Includes:
- Value judgment five questions
- Three-layer distillation method
- Classification label system
- Deduplication and counting rules
- Output format specification

## Input

- `data/feedback/*.jsonl` — Signals produced by feedback-collector

## Output

- `data/themes/{theme}/{rule}.md` — Rules with three layers distilled

## Core Capabilities

### Value Judgment Five Questions

1. Can this rule change behavior?
2. Does this rule have specific trigger scenarios?
3. Can this rule be verified?
4. Does this rule duplicate existing rules?
5. Will users agree with this rule?

### Three-Layer Distillation

1. **Surface Rule**: Trigger scenario + Specific action + Judgment criteria
2. **Behavior Pattern**: Transferable decision logic
3. **Thinking Principle**: One sentence + explanation

### Classification

- Theme classification (theme)
- Domain classification (domain)
- Project classification (project)
- Value density annotation (value_density)

## Execution Rules

1. Filter content worth distilling from feedback
2. Validate with value judgment five questions
3. Distill three layers
4. Deduplication check
5. Classify and write to themes/

## Dependencies

- feedback-collector — Requires feedback data
