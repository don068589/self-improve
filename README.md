# Self-Improve: Agent Evolution System

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Node.js >= 18](https://img.shields.io/badge/Node.js->=18-green.svg)](https://nodejs.org/)
[![OpenClaw Compatible](https://img.shields.io/badge/OpenClaw-Compatible-blue.svg)](https://openclaw.ai)

> A pluggable, modular AI Agent self-improvement framework that enables agents to learn from experience and continuously improve execution quality.

## Overview

Self-Improve is a **production-ready self-evolution system** for AI agents. It automatically scans agent memory logs, extracts reusable experience rules, and proposes improvements to system files through a safe approval workflow.

### The Problem It Solves

AI agents make mistakes. They repeat them. They forget lessons learned. Self-Improve solves this by:

- **Learning from failures** - Analyzes error patterns and correction signals
- **Distilling reusable rules** - Converts raw experience into actionable wisdom  
- **Memory management** - Three-tier memory (HOT/WARM/COLD) for efficient retrieval
- **Safe evolution** - All system changes require explicit approval

### Why This System?

Unlike memory frameworks that require complex setup, this is a **zero-dependency approach**:
- **No database** - Pure Markdown + YAML storage
- **Privacy-first** - All data stays local
- **Modular** - Use only what you need
- **Safe evolution** - System changes require explicit approval
## Platform Compatibility

| Platform | Support | Notes |
|----------|---------|-------|
| **OpenClaw** | ✅ Native | Designed for OpenClaw agent teams |
| **Claude Code** | ✅ Compatible | Works with Claude's memory system |
| **Cursor** | ✅ Compatible | Adaptable to Cursor rules |
| **Generic LLM** | ✅ Compatible | Works with any agent with memory logs |

### System Requirements

- **Node.js** >= 18.0.0
- **Operating System**: Windows, macOS, Linux
- **Memory**: ~50MB for data storage
- **Optional**: Cron/task scheduler for automatic runs

## Architecture

\\\
┌─────────────────────────────────────────────────────────────┐
│                    Self-Improve System                       │
├─────────────────────────────────────────────────────────────┤
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐   │
│  │ Reflector│→ │ Profiler │→ │ Classifier│→ │ Proposer │   │
│  └──────────┘  └──────────┘  └──────────┘  └──────────┘   │
│       ↓             ↓             ↓             ↓          │
│  ┌──────────────────────────────────────────────────────┐  │
│  │              Memory Layer (HOT/WARM/COLD)            │  │
│  └──────────────────────────────────────────────────────┘  │
│       ↓                                                     │
│  ┌──────────────────────────────────────────────────────┐  │
│  │  Outputs: System Updates | Knowledge Base | Blog     │  │
│  └──────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────┘
\\\

## Core Modules

| Module | Purpose | Input | Output |
|--------|---------|-------|--------|
| **feedback-collector** | Scan memory logs for signals | Agent sessions | Raw signals |
| **reflector** | Analyze patterns | Raw signals | Pattern insights |
| **profiler** | Build agent profiles | Insights | Profile updates |
| **distill-classifier** | Categorize learnings | Insights | Rules by theme |
| **memory-layer** | Manage memory tiers | Rules | HOT/WARM/COLD storage |
| **proposer** | Generate improvement proposals | Rules | Proposals |
| **notify** | Deliver notifications | Proposals | Alerts |

## Memory Tiers

\\\
┌────────────────────────────────────────────────────────┐
│  HOT (data/hot.md)                                     │
│  • ≤100 lines, loaded every session                    │
│  • Frequently used rules                               │
│  • Direct impact on agent behavior                     │
├────────────────────────────────────────────────────────┤
│  WARM (data/themes/)                                   │
│  • ≤200 lines per file, loaded on demand              │
│  • Theme-based organization (coding, writing, etc.)    │
│  • Queried when relevant context needed                │
├────────────────────────────────────────────────────────┤
│  COLD (data/archive/)                                  │
│  • Unlimited size, explicit query only                │
│  • Historical rules, audit trail                       │
│  • Searchable archive                                  │
└────────────────────────────────────────────────────────┘
\\\

## Installation

### Quick Start

\\\ash
# Clone repository
git clone https://github.com/don068589/self-improve.git
cd self-improve

# Configure
cp user-config.yaml my-config.yaml
# Edit my-config.yaml with your paths

# Setup
node scripts/setup.mjs --config my-config.yaml

# Verify
node scripts/status.mjs
\\\

### Configuration

\\\yaml
# my-config.yaml
storage:
  root: ""/path/to/self-improve""        # Installation directory
  knowledge_root: ""/path/to/learned""   # Output for learned rules
  workspace_root: ""/path/to/.openclaw"" # Agent workspace

owner:
  name: ""Your Name""
  timezone: ""Asia/Shanghai""

agent:
  main_agent: ""main""  # Your main agent ID

cron:
  enabled: true
  interval_days: 3      # Run every 3 days
\\\

## Usage

### Automatic Mode (Recommended)

1. Setup creates a cron job proposal in \proposals/PENDING.md\
2. Approve the cron job in your agent configuration
3. System runs automatically every 3 days

### Manual Mode

\\\ash
# Run full improvement cycle
node scripts/run-all.mjs

# Run specific module
node scripts/improve.mjs --module reflector

# Check status
node scripts/status.mjs

# Generate report
node scripts/report.mjs
\\\

### Integration with OpenClaw

\\\javascript
// In your OpenClaw config
{
  ""skills"": [""self-improve""],
  ""cron"": {
    ""self-improve"": {
      ""schedule"": ""0 10 */3 * *"",
      ""enabled"": true
    }
  }
}
\\\

## Output Types

| Type | Location | Approval Required |
|------|----------|-------------------|
| HOT memory updates | \data/hot.md\ | No (automatic) |
| WARM theme rules | \data/themes/\ | No (automatic) |
| System file changes | \proposals/PENDING.md\ | **Yes** (manual) |
| Knowledge base entries | \knowledge/\ | No (automatic) |
| Blog drafts | \drafts/\ | No (automatic) |

## Security & Privacy

### Never Stored
- Passwords, API keys, tokens
- Financial/medical information
- Third-party personal data
- Location patterns

### Transparency
- \"What did you remember?"\ → Full export available
- Every rule has source and timestamp
- \"Forget X"\ → Delete from all memory layers

## Technical Stack

- **Runtime**: Node.js 18+
- **Language**: JavaScript (ESM)
- **Data Format**: Markdown, YAML, JSONL
- **No external dependencies** - Uses only built-in Node.js modules

## File Structure

\\\
self-improve/
├── SKILL.md              # OpenClaw skill definition
├── SYSTEM.md             # System architecture
├── ENGINE.md             # Core engine logic
├── RUNTIME.md            # Runtime behavior
├── config.yaml           # Module configuration
├── user-config.yaml      # User config template
├── modules/              # Module definitions
│   ├── reflector/
│   ├── profiler/
│   ├── distill-classifier/
│   └── ...
├── prompts/              # Execution prompts
├── scripts/              # CLI tools
│   ├── setup.mjs
│   ├── run-all.mjs
│   ├── improve.mjs
│   └── ...
└── data/                 # Data storage
    ├── hot.md
    ├── themes/
    └── archive/
\\\

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for guidelines.

## License

MIT License - Free to use, modify, and distribute.

## Acknowledgments

Built for the OpenClaw Agent team. Inspired by the need for AI agents to truly improve over time, not just accumulate chat history.