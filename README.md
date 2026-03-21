# better-work-skill

### A verification-first execution protocol for coding agents

**🇺🇸 English** | **[🇨🇳 中文](README.zh-CN.md)**

`better-work-skill` is a lightweight operating protocol for coding agents. It does not add new tools or new model weights. It improves how an agent uses the tools it already has:

- less guessing
- less premature surrender
- less asking the user too early
- less "done" without proof
- more deliberate investigation
- more verification before completion
- better handoff quality when a task is genuinely blocked
- better continuity for multi-step work

This repo packages four layers into one practical system:

- `high-agency` as the behavior core
- `better-work` as the product and command surface
- a lightweight GSD-inspired workflow skeleton for longer tasks
- `round-based-execution` as a conditional complex-task protocol module

## What Changed

Better Work now has three responsibilities:

1. enforce rigorous execution standards
2. give users a clean command surface
3. keep multi-step work from losing context

For complex tasks, it can also switch into a stricter round-by-round execution mode.

In practice, that means:

- more source reading
- more log reading
- more assumption checking
- more build/test/curl/manual verification
- fewer shallow fixes
- lighter but clearer task planning for longer work

## The Workflow Model

Better Work uses a lightweight four-phase model:

1. `intake` -> clarify goal, constraints, and success criteria
2. `plan` -> break work into verifiable slices
3. `execute` -> advance one slice at a time and keep evidence current
4. `closeout` -> end with verified completion or structured handoff

For tiny tasks, skip the written workflow and just execute rigorously.

For larger tasks, use the templates in `templates/`:

- `TASK.md`
- `PLAN.md`
- `STATE.md`
- `HANDOFF.md`

For complex tasks, activate the round protocol and also use:

- `ROUND.md`
- `round-state.json`

These files are intentionally small. They are execution aids, not long project docs.

## When To Use Workflow Files

Use the workflow templates when the task:

- spans multiple files or systems
- is likely to continue across sessions
- has already started looping or drifting
- may need a clean handoff

Skip them when the task is small enough to complete and verify immediately.

## Complex Task Enhancement

`round-based-execution` is a subskill of `better-work`, not a standalone peer skill.

Use it when:

- the task needs multiple stages
- the task spans multiple files, systems, or agents
- the task contains meaningful unknowns
- the task may cross sessions
- verification must happen round by round instead of only at the end

Its role is to convert complex work into bounded rounds with:

- explicit round type selection
- PDCA per round
- quality gates per round
- assumption and evidence tracking
- structured handoff state
- safer multi-agent coordination

See:

- [activation-policy.md](activation-policy.md)
- [subskills/round-based-execution.md](subskills/round-based-execution.md)
- [templates/ROUND.md](templates/ROUND.md)
- [templates/round-state.json](templates/round-state.json)

## Auto vs Explicit Activation

There are now two ways to enter round mode:

- automatic activation through `/better-work` when the task is clearly complex enough
- explicit activation through `/better-work rounds` when you want to force round mode immediately

Use plain `/better-work` when:

- the task might still be small enough for one bounded pass
- you want Better Work to decide whether extra structure is warranted

Use `/better-work rounds` when:

- the task is already obviously multi-stage
- you want every step to run with round-level PDCA and quality gates
- you want structured round state from the start
- you want to prevent the task from drifting into one long linear thread

## Commands

Use `/better-work` as the normalized command language in docs and examples. Depending on the tool, that may map to:

- `$better-work`
- `/prompts:better-work`
- a native slash command alias

### `/better-work`

Use this when you want the full protocol.

### `/better-work rounds`

Use this when you want to force `round-based-execution` for a complex task instead of relying on auto-activation.

### `/better-work verify`

Bias toward evidence, verification, and closeout quality.

### `/better-work unstick`

Bias toward diagnosis, alternate hypotheses, and path switching.

### `/better-work handoff`

Bias toward structured blocker reports with verified facts and next steps.

### `/better-work review`

Bias toward sibling issues, edge cases, and upstream or downstream impact.

### `/better-work plan`

Bias toward intake, success criteria, task slicing, and whether workflow files are warranted.

### `/better-work execute`

Bias toward continuing from the current task, plan, or state without losing context.

## Recommended Command Patterns

### Small or Medium Task

```text
/better-work Fix this failing API test and verify the result.
```

### Complex Multi-Round Task

```text
/better-work rounds Fix this cross-service production bug in bounded rounds.
```

### Cross-Session Research

```text
/better-work rounds Research this unfamiliar codebase and leave resumable round state.
```

### Medium Feature With Stage Gates

```text
/better-work rounds Build this feature in minimal slices with verification after each round.
```

## Lightweight Templates

The templates are intentionally short so they can survive real usage:

- [TASK.md](templates/TASK.md)
- [PLAN.md](templates/PLAN.md)
- [STATE.md](templates/STATE.md)
- [HANDOFF.md](templates/HANDOFF.md)
- [ROUND.md](templates/ROUND.md)
- [round-state.json](templates/round-state.json)

Recommended explicit command for complex tasks:

- `/better-work rounds`

When explicit round mode is active, the most useful state files are:

- `TASK.md` for overall goal and constraints
- `PLAN.md` for the broader path
- `STATE.md` for current progress
- `ROUND.md` for the current bounded round
- `round-state.json` for machine-friendly persistence and handoff

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
curl -o ~/.codex/prompts/better-work-rounds.md \
  https://raw.githubusercontent.com/d-wwei/better-work-skill/main/commands/better-work-rounds.md
```

Then start a conversation and type:

```text
$better-work
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
- produce a more useful handoff if the task is genuinely blocked

When `round-based-execution` is active, the agent should also:

- select one round type at a time
- define one primary objective per round
- enforce PDCA and a quality gate before moving on
- track assumptions, evidence grade, dependencies, and carry-forward items
- leave round-level handoff state instead of relying on chat memory

## Why This Version Is Better

Earlier Better Work was strong on execution discipline but thin on workflow.

This version keeps the original rigor while adding:

- a lightweight planning mode
- an execution mode for longer tasks
- compact state templates for work that spans sessions
- clearer separation between small-task and large-task behavior

With `round-based-execution`, Better Work can stay lightweight for small tasks while becoming stricter for long, risky, or multi-round work.

The result is still lightweight, but less likely to lose context on real engineering work.

## Repository Notes

- `.gitignore` excludes common local noise like `.DS_Store`
- `evals/trigger-prompts/` contains minimal trigger sanity checks
- `evals/closeout-cases.md` captures expected end-state behavior for future iterations
- `subskills/` contains conditional execution overlays such as `round-based-execution`
