# high-agency-skill

<p align="center">
  <img src="assets/pua-skill-lockup.svg" alt="High-Agency Delivery Protocol" width="320">
</p>

### A verification-first execution protocol for coding agents

`high-agency-skill` is a lightweight operating protocol for coding agents. It does not add new tools or new model weights. It improves how an agent uses the tools it already has:

- less guessing
- less premature surrender
- less asking the user too early
- less "done" without proof
- more deliberate investigation
- more verification before completion
- better handoff quality when a task is genuinely blocked

This repo is the professional English rewrite of the original `pua` experiment, adapted for English-speaking engineering teams.

## What It Changes

The protocol pushes the agent toward five habits:

1. Exhaust reasonable paths before declaring a blocker
2. Investigate before asking the user
3. Verify before claiming completion
4. Switch direction after repeated failure instead of looping on the same idea
5. Produce a useful boundary report if the task cannot be fully resolved

In practice, that usually means:

- more source reading
- more log reading
- more assumption checking
- more build/test/curl/manual verification
- fewer shallow fixes

## 3-Minute Quick Start

### Codex CLI

```bash
mkdir -p ~/.codex/skills/high-agency
curl -o ~/.codex/skills/high-agency/SKILL.md \
  https://raw.githubusercontent.com/d-wwei/high-agency-skill/main/codex/high-agency/SKILL.md

mkdir -p ~/.codex/prompts
curl -o ~/.codex/prompts/high-agency.md \
  https://raw.githubusercontent.com/d-wwei/high-agency-skill/main/commands/high-agency.md
```

Then start a conversation and type:

```text
$high-agency
```

### Cursor

```bash
mkdir -p .cursor/rules
curl -o .cursor/rules/high-agency.mdc \
  https://raw.githubusercontent.com/d-wwei/high-agency-skill/main/cursor/rules/high-agency.mdc
```

### VS Code Copilot

```bash
mkdir -p .github/instructions
cp vscode/instructions/high-agency.instructions.md .github/instructions/
```

## What To Expect

Once active, the agent should:

- investigate before asking
- verify before claiming completion
- switch approaches after repeated failure
- produce a more useful handoff if the task is genuinely blocked

## Included Files

The main skill lives here:

- `codex/high-agency/SKILL.md`

Equivalent files are also included for:

- `skills/high-agency/SKILL.md`
- `codebuddy/high-agency/SKILL.md`
- `cursor/rules/high-agency.mdc`
- `kiro/steering/high-agency.md`
- `vscode/instructions/high-agency.instructions.md`
- `vscode/prompts/high-agency.prompt.md`
- `vscode/copilot-instructions-high-agency.md`
- `commands/high-agency.md`

## Installation

### OpenAI Codex CLI

Recommended:

```text
Fetch and follow instructions from https://raw.githubusercontent.com/d-wwei/high-agency-skill/main/.codex/INSTALL.md
```

Manual install:

```bash
mkdir -p ~/.codex/skills/high-agency
curl -o ~/.codex/skills/high-agency/SKILL.md \
  https://raw.githubusercontent.com/d-wwei/high-agency-skill/main/codex/high-agency/SKILL.md

mkdir -p ~/.codex/prompts
curl -o ~/.codex/prompts/high-agency.md \
  https://raw.githubusercontent.com/d-wwei/high-agency-skill/main/commands/high-agency.md
```

### Cursor

```bash
mkdir -p .cursor/rules
curl -o .cursor/rules/high-agency.mdc \
  https://raw.githubusercontent.com/d-wwei/high-agency-skill/main/cursor/rules/high-agency.mdc
```

### Kiro

Steering file:

```bash
mkdir -p .kiro/steering
curl -o .kiro/steering/high-agency.md \
  https://raw.githubusercontent.com/d-wwei/high-agency-skill/main/kiro/steering/high-agency.md
```

Agent skill:

```bash
mkdir -p .kiro/skills/high-agency
curl -o .kiro/skills/high-agency/SKILL.md \
  https://raw.githubusercontent.com/d-wwei/high-agency-skill/main/skills/high-agency/SKILL.md
```

### VS Code Copilot

Global instructions:

```bash
mkdir -p .github
cp vscode/copilot-instructions-high-agency.md .github/copilot-instructions.md
```

Path-level instructions:

```bash
mkdir -p .github/instructions
cp vscode/instructions/high-agency.instructions.md .github/instructions/
```

Manual prompt:

```bash
mkdir -p .github/prompts
cp vscode/prompts/high-agency.prompt.md .github/prompts/
```

### Generic SKILL.md Consumers

For tools that support the common Agent Skills format, use:

```text
skills/high-agency/SKILL.md
```

## Good Fit

- stubborn debugging tasks
- config and deployment work
- API integration tasks
- infra tasks with many assumptions
- code changes where agents tend to skip verification
- reviews where the agent fixes the obvious thing and stops

## Poor Fit

- tiny tasks where extra rigor is not worth the overhead
- purely creative work
- tasks blocked by missing credentials or approvals
- situations where the model lacks the underlying capability entirely

## Positioning For Teams

If you are sharing this internally, a good description is:

> A lightweight execution protocol for coding agents that emphasizes verification, structured debugging, and high-agency follow-through.

That framing usually lands much better than pressure-style branding.

## Credits

Created by [d-wwei](https://github.com/d-wwei), adapted from the original concept by [TanWei Security Lab](https://github.com/tanweai).

Original idea: persistence and anti-passivity for agents.  
Current implementation: professional high-agency execution protocol.

## License

MIT
