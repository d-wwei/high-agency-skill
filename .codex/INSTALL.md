# Installing Better Work for Codex

Install `better-work` via native skill discovery (`~/.codex/skills/`).

## Prerequisites

- Git

## Installation

### macOS / Linux

```bash
# 1. Clone the repo
git clone https://github.com/d-wwei/better-work-skill.git ~/.codex/better-work-skill

# 2. Create skill symlink
mkdir -p ~/.codex/skills
ln -s ~/.codex/better-work-skill/codex/better-work ~/.codex/skills/better-work

# 3. Install prompt triggers
mkdir -p ~/.codex/prompts
ln -s ~/.codex/better-work-skill/commands/better-work.md ~/.codex/prompts/better-work.md
ln -s ~/.codex/better-work-skill/commands/better-work-waves.md ~/.codex/prompts/better-work-waves.md
ln -s ~/.codex/better-work-skill/commands/better-work-rounds.md ~/.codex/prompts/better-work-rounds.md
```

### Windows (PowerShell)

```powershell
git clone https://github.com/d-wwei/better-work-skill.git "$env:USERPROFILE\.codex\better-work-skill"
New-Item -ItemType Directory -Force "$env:USERPROFILE\.codex\skills"
cmd /c mklink /J "$env:USERPROFILE\.codex\skills\better-work" "$env:USERPROFILE\.codex\better-work-skill\codex\better-work"
New-Item -ItemType Directory -Force "$env:USERPROFILE\.codex\prompts"
cmd /c mklink "$env:USERPROFILE\.codex\prompts\better-work.md" "$env:USERPROFILE\.codex\better-work-skill\commands\better-work.md"
cmd /c mklink "$env:USERPROFILE\.codex\prompts\better-work-waves.md" "$env:USERPROFILE\.codex\better-work-skill\commands\better-work-waves.md"
cmd /c mklink "$env:USERPROFILE\.codex\prompts\better-work-rounds.md" "$env:USERPROFILE\.codex\better-work-skill\commands\better-work-rounds.md"
```

## Verify

Type one of these in a Codex conversation:

- `$better-work`
- `$better-work waves`
- `$better-work rounds`

## Modes

Core:

- `$better-work`
- `$better-work waves`
- `$better-work rounds`
- `$better-work verify`
- `$better-work unstick`
- `$better-work handoff`
- `$better-work review`
- `$better-work plan`
- `$better-work execute`

Wave-specific:

- `$better-work wave-plan`
- `$better-work wave-map`
- `$better-work wave-status`
- `$better-work wave-replan`
- `$better-work wave-handoff`

## Workflow Files

Base files:

- `TASK.md`
- `PLAN.md`
- `STATE.md`
- `HANDOFF.md`

Project-scale files:

- `MAP.md`
- `WAVE.md`
- `DECISIONS.md`
- `RISKS.md`
- `wave-state.json`

Local execution files:

- `ROUND.md`
- `round-state.json`

Use only the files that add leverage for the current task.

## Maintenance

If you modify the skill later, sanity-check:

- trigger behavior with `evals/trigger-prompts/`
- closeout quality with `evals/closeout-cases.md`
- that wave mode stays project-scale
- that round mode stays local to the current objective or current wave

## Update

```bash
cd ~/.codex/better-work-skill
git pull
```

## Uninstall

### macOS / Linux

```bash
rm ~/.codex/skills/better-work
rm ~/.codex/prompts/better-work.md
rm ~/.codex/prompts/better-work-waves.md
rm ~/.codex/prompts/better-work-rounds.md
rm -rf ~/.codex/better-work-skill
```

### Windows (PowerShell)

```powershell
Remove-Item "$env:USERPROFILE\.codex\skills\better-work"
Remove-Item "$env:USERPROFILE\.codex\prompts\better-work.md"
Remove-Item "$env:USERPROFILE\.codex\prompts\better-work-waves.md"
Remove-Item "$env:USERPROFILE\.codex\prompts\better-work-rounds.md"
Remove-Item -Recurse "$env:USERPROFILE\.codex\better-work-skill"
```
