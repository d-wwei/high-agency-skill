# Wave-Based Delivery Design

This document defines the repository-level design for `wave-based-delivery`, a new conditional subskill under `better-work`.

It is a full-version design, not an MVP sketch.

## Purpose

`wave-based-delivery` is the project-scale orchestration layer for Better Work.

It exists to solve problems that are larger than a single bounded execution stream:

- multi-module or multi-system work
- tasks that naturally break into staged delivery
- work expected to continue across sessions
- work that needs project-level sequencing, not just local execution rigor
- work where the current objective must be selected from a larger program of work

It should recover the most valuable project-scale strengths from GSD without making normal Better Work heavy by default.

## Positioning

### What it is

`wave-based-delivery` is:

- a conditional subskill
- a project-level delivery orchestration layer
- a wave planner, selector, and gatekeeper
- the owner of project-scale state and sequencing

### What it is not

It is not:

- a replacement for `better-work`
- a replacement for `round-based-execution`
- a domain-specific skill
- a mandatory layer for small or medium tasks

### Runtime relationship

- `better-work` remains the default operating protocol.
- `wave-based-delivery` activates when the task must be managed as a sequence of waves.
- `round-based-execution` activates when the current wave itself is too complex for safe linear execution.

In other words:

- `better-work` decides whether to stay light
- `wave-based-delivery` decides what the current wave is
- `round-based-execution` decides how to execute the current wave safely

## Design Goals

This module exists to prevent these project-scale failure patterns:

1. Large tasks are treated as one continuous stream until context collapses.
2. The agent starts execution before the codebase or dependency surface is mapped.
3. The task keeps expanding because no wave boundary exists.
4. Partial results accumulate without a project-level sequencing model.
5. Session continuity relies on chat memory instead of durable state.
6. The next slice of work is chosen opportunistically instead of strategically.
7. Integration, migration, and hardening work arrive too late and all at once.
8. Multiple subagents work in parallel without being rolled up into one current wave objective.

## Repository Layout

Proposed additions:

```text
subskills/
  wave-based-delivery.md
  agents/
    wave-planner.md
    wave-challenger.md
    wave-worker.md
    integration-reviewer.md
commands/
  better-work-waves.md
  better-work-wave-plan.md
  better-work-wave-map.md
  better-work-wave-status.md
  better-work-wave-replan.md
  better-work-wave-handoff.md
templates/
  MAP.md
  WAVE.md
  DECISIONS.md
  RISKS.md
  wave-state.json
designs/
  wave-based-delivery-design.md
```

Existing files reused:

- `TASK.md`
- `PLAN.md`
- `STATE.md`
- `HANDOFF.md`
- `ROUND.md`
- `round-state.json`

## Full Structure For `subskills/wave-based-delivery.md`

The subskill document should follow this structure.

### 1. Title And Positioning

Headings:

- `# Wave-Based Delivery`
- `## Positioning`
- `### Why this is a subskill`
- `### Why it stays separate from round-based-execution`
- `### Runtime relationship to better-work`

Core points:

- this is a conditional orchestration layer
- it governs project-scale sequencing, not local execution details
- it stays modular to avoid bloating default Better Work behavior

### 2. Design Goals

Headings:

- `## Design Goals`
- `## Failure Patterns This Module Prevents`

Core points:

- project decomposition
- wave-level gates
- durable state
- explicit carry-forward handling
- cleaner multi-session continuity

### 3. Activation Policy

Headings:

- `## Activation Policy`
- `### Activate when`
- `### Do not activate when`
- `### Escalate into wave mode`
- `### Deactivate back to lighter modes`

Core points:

- trigger on project-scale complexity
- do not trigger for single bounded passes
- allow late escalation when hidden project shape emerges
- allow deactivation when only small closeout work remains

### 4. Main Protocol Prompt

Heading:

- `## Main Protocol Prompt`

This section should include an overlay prompt block.

The prompt should instruct the model to:

- treat the task as a sequence of waves
- select one current wave
- define wave goal, scope, dependencies, gate, and carry-forward rules
- write durable state before advancing
- never let unverified assumptions become wave foundations
- decide whether round mode is also needed within the current wave

### 5. Lifecycle

Headings:

- `## Lifecycle`
- `### Step 0: Decide Whether To Enter Wave Mode`
- `### Step 1: Lock The Mandate`
- `### Step 2: Map The System`
- `### Step 3: Build The Wave Plan`
- `### Step 4: Select The Current Wave`
- `### Step 5: Execute Or Delegate The Current Wave`
- `### Step 6: Review The Current Wave`
- `### Step 7: Advance, Replan, Split, Or Halt`
- `### Step 8: Final Synthesis And Delivery`

Expected behavior per step:

- Step 1 writes `TASK.md`
- Step 2 writes `MAP.md`
- Step 3 updates `PLAN.md`
- Step 4 writes `WAVE.md` and `wave-state.json`
- Step 5 may activate `round-based-execution`
- Step 6 updates `STATE.md`, `RISKS.md`, and `DECISIONS.md`
- Step 7 chooses next wave action
- Step 8 closes with `HANDOFF.md` or verified completion

### 6. Wave Decomposition Policy

Headings:

- `## Wave Decomposition Policy`
- `### Wave design rules`
- `### Carry-forward rules`
- `### Split rules`
- `### Merge restrictions`

Rules:

1. Each wave has one primary delivery objective.
2. Each wave must be independently gateable.
3. A wave may contain multiple rounds, but not multiple co-equal project goals.
4. New large unknowns discovered mid-wave are recorded and explicitly triaged.
5. A wave must have declared upstream dependencies and downstream consumers.
6. If a wave would require multiple major integration points, split it.

### 7. Wave Type Library

Heading:

- `## Wave Type Library`

Recommended wave types:

- `mandate`
  Purpose: lock scope, success criteria, and non-goals.
- `mapping`
  Purpose: map modules, interfaces, services, and risk surface.
- `foundation`
  Purpose: prepare prerequisites and unblock later waves.
- `slice-delivery`
  Purpose: ship one bounded user-visible or system-visible increment.
- `integration`
  Purpose: join previously delivered slices and verify boundaries.
- `migration`
  Purpose: move data, behavior, or configuration safely between states.
- `hardening`
  Purpose: tighten resilience, correctness, tests, and regressions.
- `rollout`
  Purpose: release, observe, and validate real runtime behavior.
- `synthesis`
  Purpose: summarize outcomes, residual risks, and next directions.

Each type should define:

- purpose
- entry criteria
- typical outputs
- gate expectations
- likely next wave types

### 8. Wave Gate System

Headings:

- `## Wave Gate System`
- `### Gate outcomes`
- `### Default gate criteria`

Standard outcomes:

- `PASS`
- `PARTIAL_PASS`
- `FAIL_REWORK`
- `FAIL_REPLAN`
- `HALT_ESCALATE`

Default gate checks:

- objective completion
- evidence sufficiency
- dependency integrity
- risk posture
- carry-forward explicitness
- readiness for next wave

### 9. State Model

Headings:

- `## State Model`
- `### Required files`
- `### File ownership`
- `### What must stay current`

State contract:

- `TASK.md` holds project mandate
- `PLAN.md` holds overall wave sequence
- `MAP.md` holds codebase and dependency understanding
- `WAVE.md` holds the active wave charter
- `STATE.md` holds latest progress
- `DECISIONS.md` holds durable decision log
- `RISKS.md` holds risk register
- `HANDOFF.md` holds project-level handoff
- `wave-state.json` holds compact machine-friendly wave state

### 10. Multi-Agent Model

Headings:

- `## Multi-Agent Model`
- `### Wave planner`
- `### Wave challenger`
- `### Wave worker`
- `### Integration reviewer`

Rules:

- the main thread owns the current wave objective
- delegated work must roll up into the current wave
- no agent invents a new wave without main-thread approval
- reviewer output feeds the current wave gate

### 11. Relationship To `round-based-execution`

Headings:

- `## Relationship To Round-Based Execution`
- `### When wave mode is enough`
- `### When wave mode must also activate round mode`
- `### Nesting rules`

Nesting rules:

- wave first, round second
- one active wave at a time
- multiple rounds may exist within one wave
- no round may redefine the project mandate
- round carry-forward must flow into wave state

### 12. Anti-Patterns

Headings:

- `## Anti-Patterns`

Examples:

- treating the whole project as one wave
- starting execution before mapping dependencies
- letting wave goals include multiple co-equal deliverables
- entering next wave without gate outcome
- hiding carry-forward assumptions
- using wave mode for tiny tasks

### 13. Exit Conditions

Headings:

- `## Exit Conditions`
- `### Exit to better-work only`
- `### Exit to better-work plus round-based-execution`
- `### Exit with handoff`

## Proposed Command Files

### `commands/better-work-waves.md`

Purpose:

- explicit manual activation of `wave-based-delivery`

Behavior bias:

- create or refresh project-scale orchestration
- decide whether round mode is also needed

### `commands/better-work-wave-plan.md`

Purpose:

- build or update the project wave plan

Behavior bias:

- define wave sequence
- order dependencies
- assign gate expectations
- update `PLAN.md`, `WAVE.md`, and `wave-state.json` as needed

### `commands/better-work-wave-map.md`

Purpose:

- build or refresh project mapping before delivery begins

Behavior bias:

- map modules, interfaces, services, environments, and dependencies
- write `MAP.md`
- capture unknowns and risk surface

### `commands/better-work-wave-status.md`

Purpose:

- summarize current project state with wave focus

Behavior bias:

- show current wave, gate status, carry-forward items, next action, and blockers
- refresh `STATE.md` and `wave-state.json`

### `commands/better-work-wave-replan.md`

Purpose:

- replan wave sequence after new information, failure, or scope change

Behavior bias:

- split, reorder, merge, or retire waves
- update risk and decision logs
- preserve what remains true

### `commands/better-work-wave-handoff.md`

Purpose:

- write a project-level handoff, not just a local status note

Behavior bias:

- summarize completed waves
- summarize current wave state
- capture pending waves, risks, and unresolved dependencies
- produce a clean restart point

## Proposed Template Files

### `templates/MAP.md`

Sections:

- `# Map`
- `## System Boundary`
- `## Modules And Services`
- `## Key Interfaces`
- `## Data Or State Boundaries`
- `## Environments`
- `## Unknowns`
- `## Risks`

### `templates/WAVE.md`

Sections:

- `# Current Wave`
- `## Wave ID`
- `## Wave Type`
- `## Objective`
- `## In Scope`
- `## Out Of Scope`
- `## Dependencies`
- `## Inputs`
- `## Expected Outputs`
- `## Gate Criteria`
- `## Carry-Forward Rules`
- `## Does This Wave Need Round Mode`
- `## Next Candidate Waves`

### `templates/DECISIONS.md`

Sections:

- `# Decisions`
- repeated entries with:
  - date
  - decision
  - reason
  - rejected alternatives
  - impact on later waves

### `templates/RISKS.md`

Sections:

- `# Risks`
- repeated entries with:
  - risk
  - affected wave
  - likelihood
  - impact
  - mitigation
  - trigger signal
  - owner

### `templates/wave-state.json`

Suggested keys:

```json
{
  "project_phase": "wave-delivery",
  "current_wave_id": "wave-03",
  "current_wave_type": "slice-delivery",
  "current_wave_gate": "PARTIAL_PASS",
  "round_mode_active": false,
  "completed_waves": [],
  "pending_waves": [],
  "blocked_by": [],
  "carry_forward": [],
  "next_action": "",
  "last_updated": ""
}
```

## Activation Policy Changes

`activation-policy.md` should be expanded, not replaced.

### 1. Replace the mode stack table

Proposed table:

| Situation | Active modules |
|---|---|
| Small, direct, one-shot task | `better-work` |
| Multi-step task with light structure needs | `better-work` plus workflow templates |
| Complex single-stream task | `better-work` plus `round-based-execution` |
| Project-scale multi-wave task | `better-work` plus `wave-based-delivery` |
| Project-scale task with complex current wave | `better-work` plus `wave-based-delivery` plus `round-based-execution` |
| Complex debugging within project-scale work | `better-work` plus `wave-based-delivery` plus `round-based-execution` plus debugging mode |
| High-risk project-scale work | `better-work` plus `wave-based-delivery` plus `round-based-execution` plus stricter verification or risk mode |

### 2. Add activation checklist for `wave-based-delivery`

Add a new section:

- `## Activation Checklist For wave-based-delivery`

Activate when one or more are true:

- the work naturally breaks into multiple delivery waves
- the task spans several modules, services, or environments
- a map of the system or codebase is needed before safe execution
- the task will likely continue across sessions
- the task contains multiple bounded increments instead of one pass
- integration or migration must be staged
- sequencing decisions materially affect safety or cost of rework
- main-thread coordination across subproblems is required

Do not activate when all are true:

- the task can be completed and verified in one bounded pass
- there is no meaningful wave dependency
- wave state would be heavier than the work
- the problem is complex locally but not project-scale

### 3. Change activation decision order

Replace the current order with:

1. Can the task be completed and verified safely in one pass
   If yes, stay in default `better-work`.
2. Does the task require project-scale decomposition into staged delivery waves
   If yes, activate `wave-based-delivery`.
3. If not project-scale, does the current work still need bounded rounds and quality gates
   If yes, activate `round-based-execution`.
4. If wave mode is active, does the current wave itself need bounded rounds
   If yes, layer `round-based-execution` inside the active wave.
5. Does the task also need a domain-specific operating mode
   If yes, stack it on top of the current structure rather than replacing it.

### 4. Add escalation into wave mode

Add a new section:

- `## Escalation Into Wave Mode`

Enter wave mode when:

- repeated restarts reveal hidden project structure
- the task keeps generating new parallel workstreams
- mapping and sequencing are becoming more important than local execution
- integration or migration concerns are arriving early enough that they must shape the plan now

### 5. Add deactivation from wave mode

Add a new section:

- `## Deactivation From Wave Mode`

Exit wave mode when:

- only one small bounded closeout slice remains
- no meaningful project-scale sequencing decision remains
- the project has entered final synthesis and handoff only

## Agent File Proposals

These are not required for the first implementation pass, but they belong in the full design.

### `subskills/agents/wave-planner.md`

Role:

- propose or update wave sequence
- identify dependencies, order, and gate logic

### `subskills/agents/wave-challenger.md`

Role:

- challenge whether wave boundaries are too large, too vague, or wrongly ordered

### `subskills/agents/wave-worker.md`

Role:

- execute the current wave under the main-thread objective

### `subskills/agents/integration-reviewer.md`

Role:

- review boundaries between completed waves
- verify readiness for integration, migration, or hardening waves

## Implementation Notes

To keep Better Work coherent:

- `wave-based-delivery` must be described in `README.md`
- `round-based-execution` should be updated to reference wave mode as its project-scale parent when both are active
- `better-work` core `SKILL.md` should mention the new decision order
- `commands/` should expose explicit wave commands without forcing their use

## Recommended Build Order

1. Add `subskills/wave-based-delivery.md`
2. Add the new wave templates
3. Update `activation-policy.md`
4. Add explicit wave commands
5. Update `README.md`
6. Update `SKILL.md` references
7. Add evals for activation and wave-vs-round selection
