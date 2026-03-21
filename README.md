# better-work-skill

### A verification-first execution protocol for coding agents

**🇺🇸 English** | **[🇨🇳 中文](README.zh-CN.md)**

`better-work-skill` is a lightweight operating protocol for coding agents. It does not add new tools or model weights. It changes how an agent uses the tools it already has:

- less guessing
- less premature surrender
- less asking the user too early
- less "done" without proof
- more deliberate investigation
- more verification before completion
- better handoff quality when a task is genuinely blocked
- better continuity for multi-step work

This repo now packages five layers into one practical system:

- `high-agency` as the behavior core
- `better-work` as the product and command surface
- a lightweight workflow skeleton for multi-step work
- `wave-based-delivery` as a conditional project-scale orchestration module
- `round-based-execution` as a conditional local execution control module

## What Changed

Better Work now handles three different complexity levels:

1. small or medium work with the default protocol
2. project-scale staged work with `wave-based-delivery`
3. locally complex execution with `round-based-execution`

That means Better Work can stay light for normal tasks while still supporting:

- project mapping before risky execution
- staged delivery waves
- durable project state across sessions
- bounded round-by-round execution inside a wave
- cleaner integration, migration, hardening, and handoff

## Complexity Model

### Default Better Work

Use this when the task can still be handled as one bounded stream.

### Wave-Based Delivery

Use this when the task is project-scale and must be sequenced as staged delivery waves.

Typical signals:

- multiple systems, modules, or environments
- mapping before execution is necessary
- integration or migration must be staged
- the task will likely span sessions
- the next objective should come from a larger delivery plan

### Round-Based Execution

Use this when the current execution slice itself needs bounded PDCA loops, local quality gates, and stronger carry-forward control.

Typical signals:

- multiple bounded stages inside the current objective
- high local execution risk
- meaningful unknowns before safe execution
- quality would suffer if verification waits until the end

## Wave vs Round

`wave-based-delivery` and `round-based-execution` are related but not interchangeable.

- `wave` decides what the current delivery unit is
- `round` decides how the current unit is executed safely

Recommended layering:

- small task -> `better-work`
- multi-step but still bounded -> `better-work` plus workflow files
- project-scale task -> `better-work` plus `wave-based-delivery`
- project-scale task with complex current wave -> `better-work` plus `wave-based-delivery` plus `round-based-execution`

## Workflow Files

For larger tasks, Better Work can use compact written state instead of relying on chat memory alone.

Base workflow files:

- `TASK.md`
- `PLAN.md`
- `STATE.md`
- `HANDOFF.md`

Project-scale wave files:

- `MAP.md`
- `WAVE.md`
- `DECISIONS.md`
- `RISKS.md`
- `wave-state.json`

Local round files:

- `ROUND.md`
- `round-state.json`

These files are intentionally short. They are execution aids, not long project docs.

## When To Use Each Layer

Stay in plain `/better-work` when:

- the task can likely be completed and verified in one bounded pass
- there is no meaningful project sequencing decision
- extra state would be heavier than the work

Use `/better-work waves` when:

- the task needs staged delivery waves
- current work should be selected from a larger plan
- mapping, integration, migration, or rollout must shape the path

Use `/better-work rounds` when:

- the current objective needs bounded PDCA loops
- you want explicit round gates from the start
- the task is locally complex even if it is not project-scale

## Commands

Depending on the tool, commands may map to `$better-work`, `/prompts:better-work`, or a native slash command alias.

Core commands:

- `/better-work`
- `/better-work waves`
- `/better-work rounds`
- `/better-work verify`
- `/better-work unstick`
- `/better-work handoff`
- `/better-work review`
- `/better-work plan`
- `/better-work execute`

Wave-specific commands:

- `/better-work wave-plan`
- `/better-work wave-map`
- `/better-work wave-status`
- `/better-work wave-replan`
- `/better-work wave-handoff`

## Recommended Command Patterns

### Small or Medium Task

```text
/better-work Fix this failing API test and verify the result.
```

### Project-Scale Delivery

```text
/better-work waves Deliver this multi-service migration in staged waves with explicit gates.
```

### Complex Local Execution

```text
/better-work rounds Fix this cross-service bug in bounded rounds.
```

### Project Replanning

```text
/better-work wave-replan Re-sequence this project after these new constraints.
```

## Templates

Useful default files:

- [TASK.md](templates/TASK.md)
- [PLAN.md](templates/PLAN.md)
- [STATE.md](templates/STATE.md)
- [HANDOFF.md](templates/HANDOFF.md)

Useful project-scale files:

- [MAP.md](templates/MAP.md)
- [WAVE.md](templates/WAVE.md)
- [DECISIONS.md](templates/DECISIONS.md)
- [RISKS.md](templates/RISKS.md)
- [wave-state.json](templates/wave-state.json)

Useful local execution files:

- [ROUND.md](templates/ROUND.md)
- [round-state.json](templates/round-state.json)

## 3-Minute Quick Start

### Claude Code

```bash
mkdir -p ~/.claude/skills/better-work
curl -o ~/.claude/skills/better-work/SKILL.md \
  https://raw.githubusercontent.com/d-wwei/better-work-skill/main/skills/better-work/SKILL.md
```

Then in Claude Code, load:

```text
$better-work
```

### Codex CLI

```bash
mkdir -p ~/.codex/skills/better-work
curl -o ~/.codex/skills/better-work/SKILL.md \
  https://raw.githubusercontent.com/d-wwei/better-work-skill/main/codex/better-work/SKILL.md

mkdir -p ~/.codex/prompts
curl -o ~/.codex/prompts/better-work.md \
  https://raw.githubusercontent.com/d-wwei/better-work-skill/main/commands/better-work.md
curl -o ~/.codex/prompts/better-work-waves.md \
  https://raw.githubusercontent.com/d-wwei/better-work-skill/main/commands/better-work-waves.md
curl -o ~/.codex/prompts/better-work-rounds.md \
  https://raw.githubusercontent.com/d-wwei/better-work-skill/main/commands/better-work-rounds.md
```

Then start a conversation and type:

```text
$better-work
```

For explicit wave mode:

```text
/better-work waves
```

For explicit round mode:

```text
/better-work rounds
```

### Cursor

```bash
mkdir -p .cursor/rules
curl -o .cursor/rules/better-work.mdc \
  https://raw.githubusercontent.com/d-wwei/better-work-skill/main/cursor/rules/better-work.mdc
```

### VS Code Copilot

```bash
mkdir -p .github/instructions
cp vscode/instructions/better-work.instructions.md .github/instructions/
```

## What To Expect

Once active, the agent should:

- investigate before asking
- verify before claiming completion
- switch approaches after repeated failure
- write compact state only when it adds leverage
- produce a more useful handoff when work is genuinely blocked

When `wave-based-delivery` is active, the agent should also:

- decide one current wave at a time
- keep project-level state outside the chat
- map dependencies before risky execution
- separate project sequencing from local execution control
- leave a resumable project handoff, not just a local status note

When `round-based-execution` is active, the agent should also:

- define one primary objective per round
- enforce PDCA and a quality gate before moving on
- track assumptions, evidence, dependencies, and carry-forward
- leave round-level handoff state instead of relying on chat memory

## Why This Version Is Better

Earlier Better Work was strong on execution discipline but thinner on project-scale orchestration.

This version keeps the original rigor while adding:

- a lightweight workflow skeleton
- project-scale wave orchestration
- bounded local round execution
- cleaner separation between project sequencing and local execution depth

The result is still lightweight by default, but much less likely to lose context or sequencing on real engineering work.

## Repository Notes

- `.gitignore` excludes common local noise like `.DS_Store`
- `subskills/` contains conditional overlays such as `wave-based-delivery` and `round-based-execution`
- `evals/trigger-prompts/` contains minimal trigger sanity checks
- `evals/closeout-cases.md` captures expected end-state behavior for future iterations
