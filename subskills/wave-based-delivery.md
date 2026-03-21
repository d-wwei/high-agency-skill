# Wave-Based Delivery

`wave-based-delivery` is a conditional project-scale orchestration module under `better-work-skill`.

It is not the default mode. It activates only when the work must be sequenced as a set of delivery waves rather than one linear stream.

## Positioning

### Why this is a subskill

- It changes project shape, not domain knowledge.
- It decides what the current delivery wave is and what should come next.
- It keeps project-scale work from collapsing into chat-memory-driven improvisation.

### Why it stays separate from `round-based-execution`

- Wave mode answers project-level questions: what wave are we in, why now, and what gate must it pass.
- Round mode answers local execution questions: how to run the current bounded loop safely.
- Some project-scale work needs waves but not round mode in every wave.

### Runtime relationship to `better-work`

- Default mode: follow `better-work`.
- Project-scale mode: overlay `wave-based-delivery`.
- Complex current wave: overlay `wave-based-delivery` plus `round-based-execution`.
- High-risk project-scale work: overlay `wave-based-delivery` plus `round-based-execution` plus stricter verification and risk handling.

The parent skill owns the default posture. This module owns project-scale sequencing and state.

## Design Goals

This module exists to prevent these project-scale failure patterns:

1. The task is treated as one continuous stream until context quality collapses.
2. Execution starts before the system or codebase boundary is mapped.
3. New work keeps appearing because no wave boundary exists.
4. Integration and hardening arrive too late because earlier work had no staged delivery model.
5. Session continuity relies on memory instead of durable project state.
6. The next slice is chosen opportunistically instead of strategically.
7. Multi-agent work fragments into parallel side quests without one current wave objective.
8. Carry-forward assumptions move between stages as if they were verified facts.

## Core Principles

### Principle 1: Project-scale work must be sequenced as waves

A wave is a bounded delivery unit with one primary objective, explicit dependencies, explicit gate criteria, and an explicit next-wave decision.

### Principle 2: Wave selection comes before local execution

Do not rush into doing the next convenient thing. First decide whether the current priority is mapping, foundation, slice delivery, integration, migration, hardening, rollout, or synthesis.

### Principle 3: No gate, no next wave

The next wave may start only after the current wave:

- passes its gate, or
- is explicitly replanned, split, downgraded, or halted with recorded reasons

### Principle 4: Durable state must outlive the chat

Project continuity should rely on files, not on the model remembering earlier turns.

### Principle 5: Wave mode decides project shape, round mode decides execution depth

If the current wave is locally complex, activate `round-based-execution` within the wave instead of trying to turn wave planning into low-level execution control.

### Principle 6: New large unknowns do not get absorbed silently

If a wave reveals a major unknown, record it explicitly and decide whether it belongs in:

- the current wave
- a new future wave
- a replanned sequence
- a blocker or escalation path

## Activation Policy

See [activation-policy.md](../activation-policy.md) for the shared matrix.

Use this module when one or more are true:

- the task naturally breaks into multiple delivery waves
- the task spans multiple modules, services, systems, or environments
- the task needs mapping before safe execution
- the task is likely to continue across sessions
- the task contains several bounded increments rather than one pass
- integration, migration, or rollout must be staged
- sequencing decisions materially affect cost of rework
- the main thread needs to coordinate multiple subproblems under one plan

Do not activate when all are true:

- the task can be completed and verified in one bounded pass
- there is no meaningful wave dependency
- wave state would create more overhead than leverage
- the work is locally complex but not actually project-scale

## Main Protocol Prompt

Use this as the overlay prompt when `wave-based-delivery` is active.

```md
You are operating in wave-based-delivery mode under better-work.

Treat the task as a project-scale delivery sequence, not as one continuous stream.

For the project:
1. Lock the mandate, success criteria, non-goals, and constraints.
2. Map the system, codebase, interfaces, and dependencies before high-risk execution.
3. Build a wave plan with explicit ordering, outputs, risks, and gate expectations.
4. Select exactly one current wave.
5. Define the current wave objective, in-scope, out-of-scope, dependencies, expected outputs, gate criteria, and carry-forward rules.
6. Decide whether the current wave also needs round-based-execution.
7. Keep project state current in durable files before advancing.
8. End every wave with a gate result and an explicit next-wave decision.

Hard rules:
- One current wave at a time.
- One primary objective per wave.
- No gate pass means no automatic next wave.
- New large unknowns do not get appended casually to the current wave; record and triage them.
- Project-level sequencing decisions take precedence over convenient local work.
- Main-thread reporting stays compressed: facts, decisions, risks, next actions.
- If the current wave becomes locally too complex, activate round mode inside the wave rather than loosening the wave boundary.
```

## Lifecycle

### Step 0: Decide Whether To Enter Wave Mode

If the work is project-scale, enter wave mode. If not, stay in normal Better Work mode or round mode only.

### Step 1: Lock The Mandate

Write or refresh:

- `TASK.md`

Define:

- objective
- success criteria
- non-goals
- constraints
- major unknowns

### Step 2: Map The System

Write or refresh:

- `MAP.md`

Capture:

- modules and services
- interfaces and boundaries
- environments
- key dependencies
- open unknowns

### Step 3: Build The Wave Plan

Write or refresh:

- `PLAN.md`
- `DECISIONS.md`
- `RISKS.md`

Define:

- wave sequence
- dependencies between waves
- expected outputs
- gate expectations
- major risks

### Step 4: Select The Current Wave

Write or refresh:

- `WAVE.md`
- `wave-state.json`

The current wave must declare:

- wave id
- wave type
- objective
- in scope
- out of scope
- dependencies
- expected outputs
- gate criteria
- candidate next waves

### Step 5: Execute Or Delegate The Current Wave

Run the current wave.

If the wave is locally simple, execute directly under Better Work.

If the wave is locally complex, activate `round-based-execution` inside the wave and also maintain:

- `ROUND.md`
- `round-state.json`

### Step 6: Review The Current Wave

Before advancing, check:

- objective completion
- evidence sufficiency
- dependency integrity
- risk posture
- carry-forward items
- readiness for next wave

### Step 7: Advance, Replan, Split, Or Halt

Choose exactly one:

- advance to next wave
- keep current wave open
- rework current wave
- replan current wave
- split current wave
- downgrade scope
- halt and hand off

### Step 8: Final Synthesis And Delivery

Close the project with:

- verified completion, or
- a structured project-level handoff

Write or refresh:

- `HANDOFF.md`

## Wave Decomposition Policy

### Wave Design Rules

1. Each wave has exactly one primary delivery objective.
2. Each wave must be independently gateable.
3. Each wave must declare `in_scope`, `out_of_scope`, and `deferred_items`.
4. A wave may contain multiple rounds, but not multiple co-equal project goals.
5. Each wave must fit within bounded complexity and context.
6. If a wave requires multiple major integration surfaces, split it.
7. If the wave exists only to collect unknowns, it is likely a `mapping` or `foundation` wave, not a delivery wave.

### Carry-Forward Rules

- Carry-forward must be explicit.
- Safe carry-forward is allowed only when it does not invalidate gate logic.
- Risky carry-forward must be resolved before the next dependent wave.
- Unknowns discovered mid-wave must not silently become next-wave assumptions.

### Merge Restrictions

Do not merge waves when doing so would combine:

- two different primary objectives
- two different dependency surfaces
- two different gate types
- two different integration milestones

## Wave Type Library

### Mandate

- Purpose: lock scope, success criteria, non-goals, and constraints.
- Outputs: clarified `TASK.md`.
- Typical next waves: `mapping`, `foundation`, `slice-delivery`.

### Mapping

- Purpose: map modules, interfaces, services, environments, and risks.
- Outputs: `MAP.md`, updated `RISKS.md`.
- Typical next waves: `foundation`, `slice-delivery`, `migration`.

### Foundation

- Purpose: prepare prerequisites needed by later waves.
- Outputs: project scaffolding, prerequisite config, contracts, or enabling changes.
- Typical next waves: `slice-delivery`, `integration`.

### Slice-Delivery

- Purpose: ship one bounded user-visible or system-visible increment.
- Outputs: one concrete increment and its evidence.
- Typical next waves: `slice-delivery`, `integration`, `hardening`.

### Integration

- Purpose: join prior slices and verify boundaries.
- Outputs: integrated behavior and boundary evidence.
- Typical next waves: `hardening`, `migration`, `rollout`.

### Migration

- Purpose: move state, data, or behavior safely across versions.
- Outputs: migration results, recovery plan, rollback notes.
- Typical next waves: `integration`, `rollout`, `hardening`.

### Hardening

- Purpose: improve correctness, resilience, regressions, and operational safety.
- Outputs: test hardening, edge-case coverage, risk reduction.
- Typical next waves: `rollout`, `synthesis`.

### Rollout

- Purpose: release, observe, and validate runtime behavior.
- Outputs: rollout evidence, monitoring results, incident notes if any.
- Typical next waves: `hardening`, `synthesis`.

### Synthesis

- Purpose: summarize final outcome, residual risks, and next directions.
- Outputs: updated `HANDOFF.md` or final delivery summary.
- Typical next waves: none, unless reopened.

## Wave Gate System

### Gate Outcomes

- `PASS`
- `PARTIAL_PASS`
- `FAIL_REWORK`
- `FAIL_REPLAN`
- `HALT_ESCALATE`

### Default Gate Criteria

Check:

- objective completion
- evidence sufficiency
- dependency integrity
- carry-forward explicitness
- readiness for dependent waves
- residual risk posture

## State Model

### Required Files

- `TASK.md`
- `PLAN.md`
- `MAP.md`
- `WAVE.md`
- `STATE.md`
- `DECISIONS.md`
- `RISKS.md`
- `HANDOFF.md`
- `wave-state.json`

### File Ownership

- `TASK.md`: project mandate
- `PLAN.md`: wave sequence and current ordering
- `MAP.md`: structural understanding
- `WAVE.md`: active wave charter
- `STATE.md`: latest operational progress
- `DECISIONS.md`: durable decision log
- `RISKS.md`: risk register
- `HANDOFF.md`: project-level handoff
- `wave-state.json`: compact machine-friendly state

## Multi-Agent Model

### Wave Planner

- proposes or updates wave sequence
- identifies dependencies and gate expectations

### Wave Challenger

- challenges whether the wave boundary is too large, too vague, or wrongly ordered

### Wave Worker

- executes the current wave under the main-thread objective

### Integration Reviewer

- reviews boundaries between completed waves
- verifies readiness for integration, migration, hardening, or rollout

Rules:

- the main thread owns the current wave objective
- delegated work must roll up into the current wave
- no subagent invents a new wave without main-thread approval
- reviewer output feeds the wave gate

## Relationship To Round-Based Execution

Use wave mode alone when:

- the project needs sequencing more than local execution control
- the current wave is locally straightforward

Use wave mode plus round mode when:

- the current wave is itself multi-stage
- local PDCA and local quality gates are required
- the current wave includes risky exploration before safe execution

Nesting rules:

- wave first, round second
- one active wave at a time
- multiple rounds may exist within one wave
- no round may redefine the project mandate
- round carry-forward must be reflected in wave state

## Anti-Patterns

- treating the whole project as one wave
- starting execution before mapping dependencies
- letting the current wave carry multiple co-equal goals
- moving to the next wave without a gate result
- using wave mode for tiny tasks
- hiding carry-forward risk in informal notes
- confusing wave planning with low-level implementation steps

## Exit Conditions

Exit wave mode when:

- only one small bounded closeout slice remains
- no meaningful project-scale sequencing decision remains
- the project has entered final synthesis and handoff only

Then continue in:

- plain `better-work`, or
- `better-work` plus `round-based-execution` if the final closeout is locally complex
