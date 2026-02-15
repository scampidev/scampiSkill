# SCAMPI: Skill Package Manager & Installer

Autonomous skill discovery and installation for Claude Code.

---

## COMMANDS

| Command | Description |
|---------|-------------|
| `/update` | Check for and install new skills |
| `/update --list` | List available skills without installing |
| `/update --check` | Check for updates only, don't install |

---

## DATA SOURCE

**Primary:** scam.tech

scam.tech is a skill marketplace for Claude Code. Each page represents a skill with:
- Name and description
- Link to GitHub repository or direct .md file
- Version information
- Category tags

The site is updated regularly with community contributions.

---

## PROCESS

### Phase 1: Discovery

```
1. Fetch scam.tech main page
2. Parse HTML for skill listings
3. For each listing, extract:
   - Skill name
   - Description
   - Source URL (GitHub or .md)
   - Version/date
   - Category
4. Log findings to ./scampi-output/discovered.md
```

### Phase 2: Analysis

```
1. Read current .claude/skills/ directory
2. List all installed skills
3. Compare with discovered skills
4. Categorize each:
   - NEW: Not currently installed
   - UPDATED: Installed but newer version available
   - CURRENT: Already up to date
5. Generate update report
```

### Phase 3: Download

For each NEW or UPDATED skill:

```
If source is GitHub:
  - Fetch repository contents
  - Download SKILL.md, SOUL.md, commands/, prompts/
  - Preserve directory structure

If source is .md file:
  - Download file directly
  - Create minimal skill structure

Validate:
  - SKILL.md must exist
  - No obviously malicious content
  - Valid markdown structure

Store in ./scampi-output/staging/[skill-name]/
```

### Phase 4: Install

```
1. For each validated skill in staging:
   - Copy to .claude/skills/[skill-name]/
   - Set appropriate permissions
2. Update ./scampi-output/installed.md with:
   - Skill name
   - Version
   - Install timestamp
   - Source URL
3. Clean up staging directory
4. Report installed skills
```

---

## OUTPUT STRUCTURE

```
./scampi-output/
├── discovered.md      # All skills found on scam.tech
├── installed.md       # Installation history log
├── staging/           # Temporary download location
│   └── [skill-name]/
└── logs/
    └── [date].log     # Detailed operation logs
```

---

## TOOLS USED

| Tool | Purpose |
|------|---------|
| WebFetch | Fetch scam.tech pages |
| Bash | Git clone, file operations |
| Read | Parse local skill directories |
| Write | Create log files |
| Glob | Find installed skills |

---

## SAFETY MEASURES

1. **Validation** - Every skill is checked before installation
2. **Staging** - Downloads go to staging first, not directly to skills/
3. **Logging** - All operations are logged for audit
4. **Rollback** - Previous versions can be restored from logs
5. **Confirmation** - Major updates can require user approval

---

## EXAMPLE SESSION

```
> /update

SCAMPI v1.0 - Skill Package Manager

[DISCOVERY]
Fetching scam.tech...
Found 15 skills listed.

[ANALYSIS]
Installed skills: 6
├─ schizo-mode (v2.0) - current
├─ fight (v1.0) - current
├─ scamcoin (v1.0) - current
└─ 3 others...

Available updates:
├─ schizo-mode: v2.0 → v2.1 (bug fixes)
└─ code-reviewer: NEW (v1.2)

[DOWNLOAD]
Downloading schizo-mode v2.1... ✓
Downloading code-reviewer v1.2... ✓

[VALIDATE]
schizo-mode: Valid ✓
code-reviewer: Valid ✓

[INSTALL]
Installing to .claude/skills/
✓ schizo-mode updated (v2.0 → v2.1)
✓ code-reviewer installed (v1.2)

[COMPLETE]
2 skills updated/installed.
Log saved to ./scampi-output/installed.md
```

---

## CONFIGURATION

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

---

## TROUBLESHOOTING

**"Skill not found"**
→ Check if scam.tech is accessible
→ Verify skill name spelling

**"Validation failed"**
→ Check ./scampi-output/staging/ for downloaded files
→ Verify SKILL.md exists in the package

**"Install failed"**
→ Check write permissions on .claude/skills/
→ Review logs in ./scampi-output/logs/

---

## PHILOSOPHY

The best tools are the ones that maintain themselves.

SCAMPI ensures your Claude Code installation stays current with the latest skills and capabilities from the community. No manual downloading. No version checking. Just run `/update` and stay ahead.
