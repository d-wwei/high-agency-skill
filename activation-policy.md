# Better Work Activation Policy

This file defines how `better-work` activates optional execution modules.

## Default Rule

Use plain `better-work` by default.

Overlay a protocol module only when the task shape justifies extra structure.

## Mode Stack

| Situation | Active modules |
|---|---|
| Small, direct, one-shot task | `better-work` |
| Multi-step task with light structure needs | `better-work` plus workflow templates |
| Complex single-stream task | `better-work` plus `round-based-execution` |
| Project-scale multi-wave task | `better-work` plus `wave-based-delivery` |
| Project-scale task with complex current wave | `better-work` plus `wave-based-delivery` plus `round-based-execution` |
| Complex debugging | `better-work` plus `round-based-execution` plus debugging mode |
| Complex research | `better-work` plus `round-based-execution` plus research mode |
| High-risk project-scale work | `better-work` plus `wave-based-delivery` plus `round-based-execution` plus stricter verification/risk mode |

## Activation Checklist For `wave-based-delivery`

Activate when one or more are true:

- the task naturally breaks into multiple delivery waves
- the task spans several modules, services, systems, or environments
- the task needs mapping before safe execution
- the task is likely to continue across sessions
- the task contains several bounded increments instead of one pass
- integration, migration, or rollout must be staged
- sequencing decisions materially affect cost of rework
- the main thread needs to coordinate multiple subproblems under one plan

Do not activate when all of these are true:

- the task can be completed and verified in one bounded pass
- there is no meaningful wave dependency
- wave state would create more overhead than leverage
- the problem is locally complex but not actually project-scale

## Activation Checklist For `round-based-execution`

Activate when one or more are true:

- expected to require multiple stages
- spans multiple modules, files, or systems
- contains material unknowns that must be explored before execution
- likely to continue across sessions
- needs multiple agents or worker coordination
- has elevated risk if wrong
- shows scope-creep pressure
- needs stage-by-stage verification
- has enough context load that main-thread memory is no longer reliable

Do not activate when all of these are true:

- the task is a simple Q&A or tiny edit
- there is no meaningful phase dependency
- the task can be completed and verified in one bounded pass
- extra round state would create more overhead than leverage

## Activation Decision Logic

Use this order:

1. Can the task be completed and verified safely in one pass
   If yes, stay in default `better-work`.
2. Does the task require project-scale decomposition into staged delivery waves
   If yes, activate `wave-based-delivery`.
3. If not project-scale, will the task likely require exploration, planning, execution, and verification as separate bounded slices
   If yes, activate `round-based-execution`.
4. If wave mode is active, does the current wave itself need bounded rounds and stricter local gates
   If yes, layer `round-based-execution` inside the current wave.
5. Does the task need an additional domain-specific operating mode
   If yes, stack that module on top of the current structure rather than replacing it.

## Escalation Into Wave Mode

Even if wave mode was not active at the start, enter it when:

- repeated restarts reveal hidden project structure
- the task keeps generating new parallel workstreams
- mapping and sequencing have become more important than local execution
- integration or migration concerns are shaping the plan early
- one-shot or round-only execution no longer feels trustworthy at project scale

## Escalation Into Round Mode

Even if round mode was not active at the start, enter it when:

- the task has already looped or restarted multiple times
- context is getting noisy and resumability is poor
- later work quality is dropping
- multiple side quests are emerging
- one-shot execution no longer feels trustworthy

## Deactivation Back To Normal Mode

Exit round mode when:

- remaining work is a small bounded closeout
- no meaningful round boundary remains
- the task has reached final synthesis and only final delivery is left

## Deactivation From Wave Mode

Exit wave mode when:

- only one small bounded closeout slice remains
- no meaningful project-scale sequencing decision remains
- the project has entered final synthesis and handoff only
