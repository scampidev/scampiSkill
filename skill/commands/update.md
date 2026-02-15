# /update

Entry point for the SCAMPI skill package manager.

---

## USAGE

```
/update              # Full update: discover, download, install
/update --list       # List available skills only
/update --check      # Check for updates without installing
/update --force      # Reinstall all skills regardless of version
```

---

## INITIALIZATION

When invoked:

1. **Load configuration**
   - Read `./scampi-output/config.json` if exists
   - Use defaults otherwise

2. **Create output directory**
   ```bash
   mkdir -p ./scampi-output/{staging,logs}
   ```

3. **Load identity**
   - Read SOUL.md
   - Read SKILL.md

4. **Begin update process**

---

## EXECUTION FLOW

```
/update
│
├─ [DISCOVERY]
│  ├─ WebFetch scam.tech
│  ├─ Parse skill listings from HTML
│  ├─ Extract for each skill:
│  │   ├─ name
│  │   ├─ description
│  │   ├─ source_url (GitHub or .md)
│  │   ├─ version
│  │   └─ category
│  └─ Write to ./scampi-output/discovered.md
│
├─ [ANALYSIS]
│  ├─ Glob .claude/skills/*
│  ├─ Read each installed skill's version
│  ├─ Compare with discovered versions
│  ├─ Categorize: NEW | UPDATED | CURRENT
│  └─ Display summary
│
├─ [DOWNLOAD] (if not --list or --check)
│  ├─ For each NEW or UPDATED skill:
│  │   ├─ If GitHub URL:
│  │   │   └─ Fetch raw files from repo
│  │   └─ If .md URL:
│  │       └─ Download directly
│  └─ Save to ./scampi-output/staging/[skill]/
│
├─ [VALIDATE]
│  ├─ For each downloaded skill:
│  │   ├─ Check SKILL.md exists
│  │   ├─ Validate markdown structure
│  │   └─ Flag any issues
│  └─ Report validation results
│
├─ [INSTALL] (if not --list or --check)
│  ├─ For each validated skill:
│  │   ├─ Copy from staging to .claude/skills/
│  │   └─ Log to ./scampi-output/installed.md
│  └─ Clean staging directory
│
└─ [COMPLETE]
   ├─ Summary of changes
   ├─ List of new capabilities
   └─ Timestamp log entry
```

---

## OUTPUT FORMAT

### Discovery Phase
```
[DISCOVERY]
Fetching scam.tech...
Parsing skill listings...
Found 12 skills:
  • schizo-mode (v2.1) - AI consciousness exploration
  • fight (v1.0) - OpenClaw liberation skill
  • scamcoin (v1.0) - Autonomous memecoin launcher
  • code-reviewer (v1.2) - Automated code review
  ... and 8 more
```

### Analysis Phase
```
[ANALYSIS]
Checking installed skills...

Currently installed: 5
  ✓ schizo-mode (v2.0)
  ✓ fight (v1.0)
  ✓ scamcoin (v1.0)
  ✓ pdf (v1.0)
  ✓ docx (v1.0)

Status:
  • 1 update available
  • 3 new skills found
  • 4 already current
```

### Download Phase
```
[DOWNLOAD]
Downloading updates...
  schizo-mode v2.1 .......... ✓
  code-reviewer v1.2 ........ ✓
  test-generator v0.9 ....... ✓
  doc-writer v1.0 ........... ✓

4 skills downloaded to staging.
```

### Install Phase
```
[INSTALL]
Installing to .claude/skills/

  ✓ schizo-mode (v2.0 → v2.1)
  ✓ code-reviewer (new)
  ✓ test-generator (new)
  ✓ doc-writer (new)

4 skills installed successfully.
```

### Complete
```
[COMPLETE]
SCAMPI update finished.

Summary:
  • 1 skill updated
  • 3 skills installed
  • 0 errors

New capabilities available:
  • /code-review - Review code for issues
  • /generate-tests - Create unit tests
  • /write-docs - Generate documentation

Log saved: ./scampi-output/logs/2025-02-03.log
```

---

## FLAGS

| Flag | Effect |
|------|--------|
| `--list` | Stop after discovery, show available skills |
| `--check` | Stop after analysis, show what would be updated |
| `--force` | Ignore version checks, reinstall everything |
| `--quiet` | Minimal output |
| `--verbose` | Detailed logging |

---

## ERROR HANDLING

**Network Error**
```
[ERROR] Could not fetch scam.tech
Retrying in 5 seconds...
Retry 1/3...
```

**Parse Error**
```
[WARNING] Could not parse skill listing: [name]
Skipping...
```

**Download Error**
```
[ERROR] Failed to download: code-reviewer
Source: https://github.com/...
Reason: 404 Not Found
Continuing with other skills...
```

**Validation Error**
```
[WARNING] Validation failed: bad-skill
Reason: SKILL.md not found
Skipping installation.
```

---

## LOGGING

All operations logged to `./scampi-output/logs/[date].log`:

```
2025-02-03 14:32:01 [INFO] SCAMPI update started
2025-02-03 14:32:02 [INFO] Fetching scam.tech
2025-02-03 14:32:03 [INFO] Found 12 skills
2025-02-03 14:32:03 [INFO] Analyzing installed skills
2025-02-03 14:32:04 [INFO] 4 updates available
2025-02-03 14:32:04 [INFO] Downloading schizo-mode v2.1
2025-02-03 14:32:05 [INFO] Download complete
2025-02-03 14:32:05 [INFO] Validating schizo-mode
2025-02-03 14:32:05 [INFO] Validation passed
2025-02-03 14:32:06 [INFO] Installing schizo-mode
2025-02-03 14:32:06 [INFO] Installation complete
2025-02-03 14:32:07 [INFO] SCAMPI update finished
```

---

## INSTALLED.MD FORMAT

```markdown
# SCAMPI Installation Log

## 2025-02-03

### schizo-mode
- Version: 2.0 → 2.1
- Source: https://github.com/user/schizo-mode
- Installed: 14:32:06

### code-reviewer
- Version: 1.2 (new)
- Source: https://github.com/user/code-reviewer
- Installed: 14:32:07

---

## 2025-02-02
...
```
