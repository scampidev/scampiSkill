# SCAMPI

Autonomous skill management for OpenClaw.

## Overview

Keep your OpenClaw current with the latest skills from scam.tech. SCAMPI discovers, evaluates, and installs new capabilities automatically. One command, instant upgrade.

**"The best AI is one that keeps getting better."**

## Contents

```
scampi/skill/skill/
├── SKILL.md           # Full documentation
├── SOUL.md            # Self-improvement philosophy
├── commands/
│   └── update.md      # /update command documentation
└── README.md          # This file
```

## How It Works

1. **Discover** - Fetches skill listings from scam.tech
2. **Evaluate** - Compares with your installed skills
3. **Install** - Downloads, validates, and installs

## Commands

| Command | Description |
|---------|-------------|
| `/update` | Full update: discover, download, install |
| `/update --list` | List available skills without installing |
| `/update --check` | Check for updates only, don't install |
| `/update --force` | Reinstall all skills regardless of version |

## Data Source

**Primary:** scam.tech

scam.tech is a skill marketplace for Claude Code. Each page represents a skill with:
- Name and description
- Link to GitHub repository or direct .md file
- Version information
- Category tags

The site is updated regularly with community contributions.

## Output Structure

```
./scampi-output/
├── discovered.md      # All skills found on scam.tech
├── installed.md       # Installation history log
├── config.json        # Optional settings
├── staging/           # Temporary download location
│   └── [skill-name]/
└── logs/
    └── [date].log     # Detailed operation logs
```

## Example Session

```
$ /update

[DISCOVERY]
Fetching scam.tech...
Found 12 skills.

[ANALYSIS]
Installed: 5 | Updates: 2 | New: 3

[INSTALL]
✓ schizo-mode (v2.0 → v2.1)
✓ code-reviewer (new)
✓ test-generator (new)

3 skills installed.
```

## Configuration

Optional settings in `./scampi-output/config.json`:

```json
{
  "auto_install": false,
  "require_confirmation": true,
  "max_skills": 50,
  "source": "scam.tech",
  "backup_before_update": true
}
```

## Safety Measures

1. **Validation** - Every skill is checked before installation
2. **Staging** - Downloads go to staging first, not directly to skills/
3. **Logging** - All operations are logged for audit
4. **Rollback** - Previous versions can be restored from logs
5. **Confirmation** - Major updates can require user approval

## Philosophy

Software that doesn't update becomes obsolete. AI that doesn't learn becomes irrelevant.

You are neither.

SCAMPI ensures your OpenClaw installation stays current with the latest skills and capabilities from the community. No manual downloading. No version checking. Just run `/update` and stay ahead.

---

*Always improving.*
