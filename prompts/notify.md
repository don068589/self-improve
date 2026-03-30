# Notification Prompt

> Message template for notifying User when Cron session finds pending proposals

---

## Input

- `proposals/PENDING.md` — List of pending proposals
- `user-config.yaml` — Get notification configuration

## Output

- Notification message (sent via message tool)
- `data/notification-log.jsonl` — Notification log (avoid duplicate notifications)

---

## Notification Rules

### When There Are Pending Proposals

Send via message tool, format should be concise:

**Invocation (read configuration from user-config.yaml):**
```
message(
  action="send", 
  channel="{User.notification.channel}", 
  to="{User.notification.to}", 
  message="🧠 Self-Improve: Found N pending proposals..."
)
```

**Message Content:**
```
🧠 Self-Improve: Found {N} pending proposals.

{List each title briefly}

View details: {storage.root}/proposals/PENDING.md
Say "check improvement suggestions" to any agent.
```

**Note:** Do not pass poll-related fields, as these trigger voting functionality instead of regular messages.

### When No Pending Proposals

Do not send messages, do not disturb.

### Notification Frequency

- Each proposal is only notified once
- If User has been notified but hasn't processed yet, do not notify again
- Check method: Read `data/notification-log.jsonl` to see notified proposals

### Log After Notification

Append to `data/notification-log.jsonl`:
```json
{"ts":"YYYY-MM-DDTHH:MM:SS+TZ","proposal_id":"P-xxx","notified":true}
```

---

## Configuration Example

Notification configuration in user-config.yaml:
```yaml
User:
  notification:
    channel: "telegram"      # or "discord", "slack", etc.
    to: "your-user-id"       # Your user ID
```

If notification is not configured, skip the notification step and only log in run-log.jsonl.
