# Installation Guide

## Prerequisites

- **OpenClaw** installed and configured
- **Node.js** 18 or higher
- A main agent configured in OpenClaw

## Step 1: Download

### Option A: Clone from GitHub

```bash
git clone https://github.com/openclaw/self-improve.git
cd self-improve
```

### Option B: Download ZIP

Download from [GitHub Releases](https://github.com/openclaw/self-improve/releases) and extract.

## Step 2: Configure

1. Copy the example config:
   ```bash
   cp user-config.yaml my-config.yaml
   ```

2. Edit `my-config.yaml` with your settings:
   ```yaml
   storage:
     root: "/home/user/self-improve"      # Where to install
     knowledge_root: "/home/user/learned" # Knowledge output
     workspace_root: "/home/user/.openclaw"  # OpenClaw root

   owner:
     name: "YourName"
     timezone: "America/New_York"
   ```

## Step 3: Run Setup

```bash
node scripts/setup.mjs --config my-config.yaml
```

The setup script will:
1. Create the directory structure
2. Initialize data files
3. Generate a Cron task proposal

## Step 4: Approve Cron Task

After setup, check `proposals/PENDING.md`:

```bash
cat proposals/PENDING.md
```

You'll see a suggested Cron task. Add it to your OpenClaw config:

**Option A: Edit `openclaw.json` directly**

Add to the `cron` section:
```json
{
  "name": "self-improve",
  "schedule": {
    "kind": "cron",
    "expr": "0 4 */3 * *",
    "tz": "Asia/Shanghai"
  },
  "sessionTarget": "isolated",
  "payload": {
    "kind": "agentTurn",
    "message": "Run self-improve...",
    "model": "bailian/qwen3.5-plus"
  }
}
```

**Option B: Use OpenClaw CLI**

```bash
openclaw cron add --from proposals/PENDING.md
```

## Step 5: Verify

Check system status:
```bash
node scripts/status.mjs
```

## Manual Run (Optional)

To run immediately without waiting for Cron:
```bash
node scripts/run-all.mjs
```

## Updating

```bash
git pull
node scripts/setup.mjs  # Re-run to update structure
```

## Uninstalling

1. Remove the Cron task from OpenClaw config
2. Delete the installation directory
3. (Optional) Archive or delete `knowledge_root` data

## Troubleshooting

### "Permission denied"
Make sure you have write access to all configured paths.

### "Module not found"
Run `node scripts/setup.mjs` to ensure all modules are copied.

### "No feedback collected"
The system scans agent memory logs. Make sure agents are writing to `memory/*.md`.

## Next Steps

- Read [SYSTEM.md](SYSTEM.md) for full documentation
- Read [ENGINE.md](ENGINE.md) for trigger rules
- Customize modules in `modules/`
