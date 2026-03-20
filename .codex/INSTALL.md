# Installing High-Agency Skill for Codex

Install the professional `high-agency` skill via native skill discovery (`~/.codex/skills/`).

## Prerequisites

- Git

## Installation

### macOS / Linux

```bash
# 1. Clone the repo
git clone https://github.com/d-wwei/high-agency-skill.git ~/.codex/high-agency-skill

# 2. Create skill symlink
mkdir -p ~/.codex/skills
ln -s ~/.codex/high-agency-skill/codex/high-agency ~/.codex/skills/high-agency

# 3. Install prompt trigger
mkdir -p ~/.codex/prompts
ln -s ~/.codex/high-agency-skill/commands/high-agency.md ~/.codex/prompts/high-agency.md

# 4. Restart Codex
```

### Windows (PowerShell)

```powershell
# 1. Clone the repo
git clone https://github.com/d-wwei/high-agency-skill.git "$env:USERPROFILE\.codex\high-agency-skill"

# 2. Create skill junction
New-Item -ItemType Directory -Force "$env:USERPROFILE\.codex\skills"
cmd /c mklink /J "$env:USERPROFILE\.codex\skills\high-agency" "$env:USERPROFILE\.codex\high-agency-skill\codex\high-agency"

# 3. Install prompt trigger
New-Item -ItemType Directory -Force "$env:USERPROFILE\.codex\prompts"
cmd /c mklink "$env:USERPROFILE\.codex\prompts\high-agency.md" "$env:USERPROFILE\.codex\high-agency-skill\commands\high-agency.md"

# 4. Restart Codex
```

## Verify

Type `$high-agency` in a Codex conversation. If the skill is loaded, it should activate.

Or check directly:

```bash
# macOS / Linux
ls ~/.codex/skills/high-agency/SKILL.md

# Windows PowerShell
Test-Path "$env:USERPROFILE\.codex\skills\high-agency\SKILL.md"
```

## Trigger Methods

| Method | Command | Requires |
|--------|---------|----------|
| Auto trigger | No action needed, matches by description | SKILL.md |
| Direct call | Type `$high-agency` in conversation | SKILL.md |
| Manual prompt | Type `/prompts:high-agency` in conversation | SKILL.md + prompts/high-agency.md |

## Update

```bash
cd ~/.codex/high-agency-skill
git pull
```

The symlink or junction automatically picks up the latest version.

## Uninstall

### macOS / Linux

```bash
rm ~/.codex/skills/high-agency
rm ~/.codex/prompts/high-agency.md
rm -rf ~/.codex/high-agency-skill
```

### Windows (PowerShell)

```powershell
Remove-Item "$env:USERPROFILE\.codex\skills\high-agency"
Remove-Item "$env:USERPROFILE\.codex\prompts\high-agency.md"
Remove-Item -Recurse "$env:USERPROFILE\.codex\high-agency-skill"
```
