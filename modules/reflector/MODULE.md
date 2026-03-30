# reflector

> Version: 1.1.0
> Status: active

## Responsibility
Self-reflection, distill reusable practices, form closed-loop learning.

## Detailed Execution Guide

**Read at execution time**: `prompts/reflector.md`

Includes:
- Reflection format specification
- Reusable practice distillation method
- Closed-loop explanation (reflections.md -> next round step 1)

## Input
- Current run process

## Output
- `data/reflections.md` — Self-reflection log

## Closed-Loop Mechanism

reflections.md will be read by feedback-collector in **next round step 1**, forming closed-loop learning.

## Dependencies
None (base module)
