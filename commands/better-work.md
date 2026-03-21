---
description: "Better Work command surface. Use /better-work for the core protocol, or add rounds, waves, verify, unstick, handoff, review, plan, execute, or wave-specific subcommands."
argument-hint: "[rounds|waves|verify|unstick|handoff|review|plan|execute|wave-plan|wave-map|wave-status|wave-replan|wave-handoff] [task description]"
---

Load the `better-work` core skill and apply it to the current task.

## Command Routing

- no subcommand -> activate the full Better Work protocol
- `rounds` -> explicitly activate `round-based-execution` for complex multi-round work
- `waves` -> explicitly activate `wave-based-delivery` for project-scale work
- `verify` -> focus on verification, evidence, and close-the-loop checks
- `unstick` -> focus on recovery method, alternate hypotheses, and changing direction
- `handoff` -> produce a structured boundary report with verified facts and next steps
- `review` -> inspect sibling issues, edge cases, and upstream/downstream impact after a fix
- `plan` -> clarify the task, define success criteria, and write a lightweight execution plan if needed
- `execute` -> continue from the current task, plan, or state and keep written progress current when those files exist
- `wave-plan` -> build or refresh the project wave sequence and gate logic
- `wave-map` -> build or refresh project mapping before delivery
- `wave-status` -> summarize current wave, gate posture, and next-wave readiness
- `wave-replan` -> replan wave order or boundaries after new information
- `wave-handoff` -> write a project-level handoff with completed, current, and pending waves

## Execution Rules

1. Load the `better-work` skill
2. Follow the behavior protocol in `SKILL.md`
3. If `$ARGUMENTS` contains text beyond the subcommand, treat it as the task description
4. Use the subcommand to bias the output:
   - `rounds`: prioritize activation of `round-based-execution`, explicit round selection, PDCA, quality gates, and structured round handoff
   - `waves`: prioritize activation of `wave-based-delivery`, wave selection, project sequencing, and durable project state
   - `verify`: prioritize evidence, tests, build output, runtime checks, and completion criteria
   - `unstick`: prioritize diagnosis, search, assumption checks, and a new approach
   - `handoff`: prioritize verified facts, eliminated possibilities, and problem boundaries
   - `review`: prioritize related-pattern checks, edge cases, and follow-on risks
   - `plan`: prioritize intake, task decomposition, verification steps, and whether workflow files are warranted
   - `execute`: prioritize the next highest-leverage slice, evidence capture, and `STATE.md` hygiene when a structured workflow is in use
   - `wave-plan`: prioritize mandate lock, dependency ordering, wave sequence, and gate definitions
   - `wave-map`: prioritize modules, interfaces, services, environments, and risk surface
   - `wave-status`: prioritize current wave state, gate posture, carry-forward, and next-wave choice
   - `wave-replan`: prioritize splitting, reordering, retiring, or downgrading waves while preserving what remains true
   - `wave-handoff`: prioritize project-level continuity, pending sequence, and restartability

## Lightweight Workflow Rule

Do not create project files for tiny one-shot tasks.

If the task is multi-step, likely to span sessions, or already showing context drift, use these lightweight files:

- `TASK.md`
- `PLAN.md`
- `STATE.md`
- `HANDOFF.md`

If `rounds` mode is active, also use:

- `ROUND.md`
- `round-state.json`

If `waves` mode is active, also use:

- `MAP.md`
- `WAVE.md`
- `DECISIONS.md`
- `RISKS.md`
- `wave-state.json`

Keep them short and execution-focused.

## Example Invocations

### `/better-work`

- `/better-work Fix this failing CI test end to end.`
- `/better-work Investigate why staging works but production fails.`
- `/better-work Implement this API change and verify the result.`

### `/better-work verify`

- `/better-work verify I think the auth fix is done.`
- `/better-work verify this config change before we merge it.`
- `/better-work verify the smallest end-to-end path for this bugfix.`

### `/better-work rounds`

- `/better-work rounds Fix this multi-service production bug with bounded rounds.`
- `/better-work rounds Research this unfamiliar codebase and leave resumable round state.`
- `/better-work rounds Implement this feature in verifiable slices with quality gates.`

### `/better-work waves`

- `/better-work waves Plan and execute this migration as staged delivery waves.`
- `/better-work waves Coordinate this multi-service feature without losing project state.`
- `/better-work waves Break this project into safe waves and decide whether any wave needs rounds.`

### `/better-work unstick`

- `/better-work unstick We have tried two fixes and still get ECONNREFUSED.`
- `/better-work unstick The migration keeps failing in the same way.`
- `/better-work unstick Stop tweaking and find a fundamentally different angle.`

### `/better-work handoff`

- `/better-work handoff If this still cannot be reproduced locally, write a clean boundary report.`
- `/better-work handoff Summarize verified facts and eliminated possibilities.`
- `/better-work handoff Prepare the next engineer to continue from here.`

### `/better-work review`

- `/better-work review Check this module for sibling issues after the fix.`
- `/better-work review Look for upstream/downstream impact before we call this done.`
- `/better-work review Inspect edge cases and similar patterns in nearby files.`

### `/better-work plan`

- `/better-work plan Break this migration into safe, verifiable steps.`
- `/better-work plan Set up a lightweight task plan for this feature.`
- `/better-work plan Decide whether this task needs written state.`

### `/better-work execute`

- `/better-work execute Continue from the current plan and update the state.`
- `/better-work execute Finish the next slice and leave evidence.`
- `/better-work execute Resume this task without losing context.`

### `/better-work wave-plan`

- `/better-work wave-plan Break this project into staged delivery waves.`
- `/better-work wave-plan Refresh wave order after these new constraints.`

### `/better-work wave-map`

- `/better-work wave-map Map the modules, services, and interfaces before wave planning.`
- `/better-work wave-map Identify the dependency surface for this migration.`

### `/better-work wave-status`

- `/better-work wave-status Summarize current wave, gate posture, and next-wave readiness.`
- `/better-work wave-status Resume this project with a clean state summary.`

### `/better-work wave-replan`

- `/better-work wave-replan Split or reorder waves after these new findings.`
- `/better-work wave-replan Re-sequence this project to reduce rework.`

### `/better-work wave-handoff`

- `/better-work wave-handoff Prepare a project-level handoff for the next session.`
- `/better-work wave-handoff Summarize completed, current, and pending waves.`
