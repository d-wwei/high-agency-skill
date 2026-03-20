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

## Why It Works

This kind of skill usually does **not** work because it teaches the model brand-new knowledge.

It works because it changes the model's default operating behavior at two levels:

1. psychological mechanism
2. engineering mechanism

### Psychological Mechanism

This is not "psychology" in the human sense. The model does not literally have emotions.

What changes is the model's default decision tendency under a prompt.

A lot of agent failures are not pure inability. They are low-cost default behaviors:

- guessing before checking
- wanting to stop after the second failed attempt
- giving advice instead of taking action
- declaring completion too early
- asking the user to investigate what the agent could investigate itself

The first job of a skill like this is to make those cheap, low-quality paths feel invalid.

In `codex/high-agency/SKILL.md`, the protocol redefines:

- what counts as a real blocker
- when the agent is allowed to ask the user
- what counts as completion

That means before the agent says "I can't" or "please check this manually," it has to pass through stricter internal rules first.

#### 1. It raises the threshold for giving up

Many agents decide they are blocked after they have "tried a few things."

This protocol changes the standard from:

> I tried something and it did not work

to:

> I have completed multiple serious investigative steps, ruled out several paths, and now have evidence that the remaining boundary is real

That is not blind persistence. It is a higher bar for declaring defeat.

#### 2. It reduces the urge to blame the environment

Models often reach for phrases like "it is probably an environment issue" because that is a low-risk way to explain failure.

But "probably" is not diagnosis.

The protocol interrupts that move by forcing verification before attribution:

- check logs
- check versions
- check paths
- check permissions
- check runtime state

This reduces hand-wavy blame and increases evidence-driven diagnosis.

#### 3. It shifts the role from answerer to owner

A default assistant often behaves like a responder:

- answer the question
- stop at the visible issue
- wait for the next instruction

This protocol pushes the model toward an owner posture:

- solve the issue end to end
- check nearby patterns
- consider upstream and downstream effects
- close the loop before stopping

That role change matters a lot. It changes the agent from "someone who replies" into "someone who is trying to complete the work."

#### 4. It replaces verbal completion with evidence-based completion

Models are naturally good at producing text that sounds complete.

But text is not proof.

This protocol moves the meaning of "done" away from language and back into execution:

- tests
- builds
- curl output
- runtime checks
- logs
- before/after observations

As long as the platform gives the agent usable tools, this usually improves quality immediately.

### Engineering Mechanism

From an engineering perspective, the protocol acts like a lightweight workflow layer for the agent.

It does not just say "try harder." It changes the structure of what the agent does after failure.

#### 1. Escalation after repeated failure

The protocol does not use the same rigor level every time.

It escalates:

- L1: stop looping and change direction
- L2: search, read source or docs, and list distinct hypotheses
- L3: complete the full recovery checklist
- L4: isolate the problem boundary with a minimal reproduction and formal report

This matters because many agents fail not because they are incapable, but because they keep using the same strategy after it has already stopped producing leverage.

#### 2. A fixed debugging pipeline

The `Recovery Method` section in the skill is essentially a standardized debug pipeline:

- identify whether the agent is looping
- read the failure signal carefully
- search proactively
- read the raw material
- verify assumptions
- invert the current assumption
- execute a new approach

That turns debugging from improvisation into a semi-structured process.

The result is that each step is more likely to produce new information instead of more guesswork.

#### 3. A new attempt must be meaningfully different

This is one of the most important parts.

Many failures look like "lots of attempts," but they are really just variants of the same idea.

The protocol requires a new attempt to satisfy three conditions:

- fundamentally different from the previous one
- clearly verifiable
- informative even if it fails

That dramatically reduces the common pattern of "looking busy without exploring a new search space."

#### 4. Post-fix expansion

The protocol does not let the agent fix A and stop immediately.

It asks:

- does the same pattern exist elsewhere?
- are sibling issues nearby?
- is there a preventative fix worth making?

This is very close to how strong engineers actually work.

They do not just patch a single bug. They ask whether that bug is one instance of a broader failure pattern.

#### 5. Structured failure output

If the problem really remains unsolved, the agent is still not allowed to fail vaguely.

It has to produce:

- verified facts
- eliminated possibilities
- current problem boundary
- best next directions
- handoff context

That turns failure from a low-information dead end into a high-information transfer artifact.

From a team perspective, that is extremely valuable.

## Why This Is Especially Effective For Coding Agents

For coding agents, the most expensive failure is usually not a single wrong answer.

It is one of these:

- wasting many turns in the wrong direction
- producing no reusable diagnostic information
- changing code without verification
- dragging the user into low-value investigation

This protocol is aimed directly at those costs.

Coding agents usually have access to the exact tools needed to make the protocol real:

- searching code
- reading files
- running commands
- editing files
- running tests

Because of that, the behavioral rules in the prompt can often turn into actual execution rather than just better-sounding text.

That is why this kind of skill tends to help more in coding environments than in pure chat environments.

## What It Does Not Do

This is not magic, and it is not a substitute for model capability.

It works best when:

- the problem is investigable
- logs, code, docs, and commands are available
- the failure is partly about weak execution habits rather than pure lack of ability
- the platform actually follows the prompt reliably

It works less well when:

- the model lacks the underlying reasoning needed to generate good hypotheses
- the agent has no useful tools or permissions
- the task depends on external credentials or approvals
- the context gets so long that the protocol loses control of the session
- the task is highly creative and weakly verifiable

In other words, this is usually better at moving a capable but sloppy agent from something like "70" to "80-85" than at turning a weak agent into a great one.

## One-Sentence Summary

The mechanism is not "be harsh and the model becomes smarter."

It is:

- use strong constraints to make low-quality paths more expensive
- use a fixed workflow to reduce unproductive trial and error
- use verification standards to change the meaning of "done"
- use escalation to force stronger diagnosis after repeated failure

The result looks like better performance, but the underlying change is that the agent's default workflow has been reshaped.

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
