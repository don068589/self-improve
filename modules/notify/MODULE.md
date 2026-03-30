# notify

> Version: 1.0.0
> Status: active

## Responsibility

Notify user of pending approval suggestions.

## Detailed Execution Guide

**Read at execution time**: `prompts/notify.md`

Includes:
- Notification format specification
- Channel configuration

## Input

- `proposals/PENDING.md` — List of pending approval suggestions
- `config.yaml` — Notification configuration

## Output

- Notification message (sent to user via configured channel)

## Execution Rules

1. Check if there are pending approval suggestions in PENDING.md
2. If yes, notify user via configured channel
3. Notification content includes suggestion title and link

## Dependencies

- proposer — Produces PENDING.md
