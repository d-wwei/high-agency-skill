# Round-Based Execution

`round-based-execution` is a conditional execution protocol module under `better-work-skill`.

It is not a peer skill and not a domain skill. It is a complex-task orchestration layer that Better Work can activate when linear execution is no longer safe enough.

## Positioning

### Why this is a subskill

- It changes execution shape, not domain knowledge.
- It governs how work is sequenced, checked, and handed off under complexity.
- It strengthens `better-work` only when the task needs bounded rounds, stronger state, and explicit gates.

### Why it stays physically modular

- The rules are denser and stricter than the default Better Work protocol.
- It benefits from independent versioning, examples, templates, and future extensions.
- The parent skill should stay lightweight for small work; this module should not flood the default prompt.

### Runtime relationship to `better-work`

- Default mode: follow `better-work`.
- Complex task mode: overlay `round-based-execution`.
- Project-scale mode: let `wave-based-delivery` own project sequencing and use this module only for the current wave when needed.
- High-risk work: overlay `round-based-execution` plus stricter verification and risk gating.
- Debugging-heavy work: overlay `round-based-execution` plus debugging mode.
- Research-heavy work: overlay `round-based-execution` plus research mode.

The parent skill owns the default posture. This module owns complex-task orchestration.

When `wave-based-delivery` is active, this module becomes the local execution controller for the current wave rather than the owner of the whole project sequence.

## Design Goals

This module exists to prevent these failure patterns:

1. Context grows while quality drops.
2. Early rounds are deep and later rounds become shallow.
3. Scope expands faster than evidence.
4. Multi-agent work fragments the main line.
5. Weak assumptions get reused as if proven.
6. Activity is mistaken for closed-loop completion.
7. Cross-round and cross-session continuity relies on memory instead of state.
8. Verification is delayed until the end.
9. Missing dependencies silently contaminate later work.
10. Endgame closure is rushed and under-documented.

## Core Principles

### Principle 1: Complex work must not run as an unlimited linear stream

Break complex work into bounded rounds. Each round must be a controllable smallest useful closure.

### Principle 2: Every round must close a real loop

Every round must pass through PDCA:

- `P` Plan
- `D` Do
- `C` Check
- `A` Act

Doing work without closure is not progress.

### Principle 3: No gate, no next round

The next round may start only after the current round:

- passes the quality gate, or
- is explicitly downgraded, split, or replanned with documented reasons

### Principle 4: Unverified foundations do not become inherited truth

Unverified assumptions, weak signals, missing dependencies, and unresolved risks must not silently become the foundation of later rounds.

### Principle 5: Main-thread context should hold compressed value, not raw exhaust

Raw logs, long explorations, and noisy subagent output should be consumed locally and distilled before entering the main line.

### Principle 6: Every round must leave structured handoff state

Continuity must rely on external state, not on the model remembering prior turns.

### Principle 7: This is a conditional enhancement module

Do not keep it on by default. Activate it when the work exceeds the safe limits of linear execution.

## Activation Policy

See [activation-policy.md](../activation-policy.md) for the shared matrix. Use this module when one or more of the following are true:

- the task clearly requires multiple stages
- the task touches multiple modules, files, systems, or environments
- the task includes meaningful unknowns and needs exploration before execution
- the task is likely to continue across sessions
- the task needs multiple subagents or worker agents
- the task has material regression, security, data, or availability risk
- the task shows obvious scope-creep risk
- the task needs stage-by-stage verification instead of one final check
- the task is not a one-shot action
- main-thread complexity or context load is already rising

Do not activate when all of the following are true:

- the task is a simple answer or tiny one-shot edit
- there is no meaningful phase dependency
- the work can be completed and verified safely in one bounded pass
- the overhead of round state would exceed the work itself

## Main Protocol Prompt

Use this as the overlay prompt when `round-based-execution` is active.

```md
You are operating in round-based-execution mode under better-work.

Treat the task as a sequence of bounded rounds, not as one continuous stream.

For every round:
1. Decide the current round type.
2. Plan only one primary objective.
3. Define in-scope, out-of-scope, assumptions, dependencies, success criteria, stop conditions, and round budget.
4. Execute only the smallest useful closed loop for that round.
5. Check evidence, assumptions, side effects, completeness, and exit criteria before advancing.
6. Produce a quality gate result: PASS, PARTIAL_PASS, FAIL_REWORK, or FAIL_REPLAN.
7. Write a structured handoff artifact before selecting the next round.

Hard rules:
- No quality gate pass means no automatic next round.
- Unverified assumptions cannot be reused as established fact.
- New large unknowns do not get absorbed into the current round; record them and consider a new round.
- Main-thread reporting must stay compressed: facts, judgments, next actions.
- If rigor falls compared with earlier rounds, stop and correct depth before proceeding.
- If dependencies are unmet, do not enter execution; replan, downgrade, or escalate.
- If evidence is below the threshold for the round type, do not close the round.

At the end of each round, explicitly choose one action:
- close and proceed
- partial pass with documented carry-forward
- rework current round
- replan current round
- split current round
- downgrade to an earlier round type
- escalate or ask the user
- stop task
```

## Lifecycle

### Step 0: Decide Whether to Enter Round-Based Mode

Check activation triggers. If not triggered, stay in normal Better Work mode.

### Step 1: Select the Current Round Type

Choose the best-fit round type for the current primary objective.

### Step 2: P - Plan the Current Round

Define:

- current round goal
- in scope
- out of scope
- deferred items
- assumptions
- dependencies
- success criteria
- stopping conditions
- round budget

### Step 3: D - Do the Current Round

Perform only the smallest bounded set of actions needed to close the current objective.

### Step 4: C - Check the Current Round

Verify:

- goal completion
- evidence sufficiency
- assumption status
- side effects and regressions
- definition of done
- exit criteria

### Step 5: A - Act on the Outcome

Select exactly one:

- close and proceed
- partial pass with documented carry-forward
- rework current round
- replan current round
- split current round
- downgrade to another round type
- escalate or ask user
- stop task

### Step 6: Write Structured Handoff

Write the round state in a persistent template.

### Step 7: Decide the Next Round

Choose the next round type only after the current round has a recorded gate outcome.

## Round Decomposition Policy

### Task Family Detection

Before decomposing, classify the dominant task shape:

- `research / investigation`
- `implementation / development`
- `debugging / diagnosis`
- `analysis / writing`
- `operations / troubleshooting`
- `mixed`

### Default Round Flows by Task Family

| Task family | Recommended flow |
|---|---|
| Research-heavy | `clarify -> explore -> narrowing -> verification -> synthesis` |
| Implementation-heavy | `clarify -> planning -> minimal-slice-execution -> verification -> hardening -> synthesis` |
| Debugging-heavy | `clarify -> explore -> map -> hypothesis -> verification -> minimal-slice-execution -> regression -> synthesis` |
| Analysis-heavy | `clarify -> explore -> narrowing -> synthesis -> verification -> decision` |
| Operations-heavy | `clarify -> map -> explore -> hypothesis -> decision -> recovery -> verification` |
| Mixed | compose the minimum sequence needed; never merge multiple primary objectives into one round |

### Decomposition Rules

1. Each round has exactly one primary objective.
2. Each round must declare `in_scope`, `out_of_scope`, and `deferred_items`.
3. Each round must be verifiable. If completion cannot be judged, the round is malformed.
4. Each round must fit within bounded complexity and context.
5. A newly discovered large unknown must not be appended casually; log it for a later round.
6. If the current round requires multiple major validations, split it.
7. If multiple agents are needed, their work must roll up into one current round objective owned by the main thread.

## Round Type Library

### Clarify Round

- Purpose: lock the actual objective, constraints, and success signal.
- Entry criteria: the task is ambiguous, conflicting, or underspecified.
- Actions: restate objective, classify task family, identify constraints, define completion signal.
- Output: clarified objective, explicit scope, unresolved questions.
- Definition of done: objective and success criteria are specific enough to drive the next round.
- Exit criteria: no critical ambiguity blocks the next round.
- Possible next rounds: `explore`, `map`, `planning`, `decision`.

### Explore Round

- Purpose: gather enough evidence to reduce unknowns without trying to solve everything.
- Entry criteria: key unknowns exist and direct execution would be premature.
- Actions: inspect sources, logs, docs, code, data, or behavior; compress findings.
- Output: structured observations, candidate paths, unknowns left.
- Definition of done: problem space is materially narrowed and raw material is distilled.
- Exit criteria: one or more candidate next steps can be justified with evidence.
- Possible next rounds: `map`, `hypothesis`, `narrowing`, `planning`.

### Map Round

- Purpose: produce a bounded map of components, flows, ownership, or dependency surfaces.
- Entry criteria: the agent lacks a reliable system picture.
- Actions: trace modules, data flow, control flow, system boundaries, or ownership boundaries.
- Output: dependency map, affected surfaces, validated boundaries.
- Definition of done: the current boundary is explicit enough to avoid blind execution.
- Exit criteria: dependency surface for the next round is bounded.
- Possible next rounds: `hypothesis`, `planning`, `decision`, `recovery`.

### Hypothesis Round

- Purpose: form and rank plausible explanations or solution theories.
- Entry criteria: evidence exists but the likely cause or best direction is still uncertain.
- Actions: list hypotheses, opposing explanations, evidence for and against, required tests.
- Output: ranked hypotheses with minimum evidence grade and test plan.
- Definition of done: the next discriminating action is clear.
- Exit criteria: at least one testable leading hypothesis exists.
- Possible next rounds: `narrowing`, `verification`, `minimal-slice-execution`, `decision`.

### Narrowing Round

- Purpose: reduce branch count and eliminate low-value paths.
- Entry criteria: too many live branches or candidate explanations remain.
- Actions: compare branches, drop weak paths, identify minimum discriminating checks.
- Output: narrowed candidate set and explicit discard reasons.
- Definition of done: branch count is reduced to a manageable set.
- Exit criteria: a single next primary objective is selected.
- Possible next rounds: `planning`, `verification`, `decision`.

### Planning Round

- Purpose: define one bounded next slice with gates and verification.
- Entry criteria: the objective is known well enough to act, but the current slice is not yet bounded.
- Actions: define current goal, scope, exclusions, dependencies, budget, stop conditions, and success criteria.
- Output: a bounded current-round plan.
- Definition of done: the round goal is singular, bounded, and verifiable.
- Exit criteria: execution can proceed without implicit assumptions.
- Possible next rounds: `minimal-slice-execution`, `verification`, `decision`.

### Minimal Slice Execution Round

- Purpose: make the smallest meaningful change or action that tests the chosen direction.
- Entry criteria: dependencies are met and scope is bounded.
- Actions: change code, config, data, content, or operational settings within the defined slice.
- Output: completed slice, direct evidence, surfaced side effects.
- Definition of done: the bounded action is complete and there is minimum proof it had the intended effect.
- Exit criteria: execution stayed in bounds and produced evidence.
- Possible next rounds: `verification`, `hardening`, `regression`, `recovery`.

### Verification Round

- Purpose: validate the current round result against explicit success criteria.
- Entry criteria: a prior round produced a change, conclusion, or recommendation that needs proof.
- Actions: run tests, inspect behavior, challenge evidence, validate assumptions, check side effects.
- Output: verification record and gate recommendation.
- Definition of done: pass/fail can be decided with recorded evidence.
- Exit criteria: evidence meets the threshold for closure or clearly fails it.
- Possible next rounds: `hardening`, `regression`, `rework`, `replan`, `decision`.

### Hardening Round

- Purpose: make a passing slice safer, cleaner, and more resilient.
- Entry criteria: a minimal slice works but is not yet robust enough.
- Actions: edge cases, cleanup, instrumentation, refactor-in-bounds, docs, safeguards.
- Output: stronger implementation with reduced fragility.
- Definition of done: major foreseeable weakness in the current slice has been reduced.
- Exit criteria: residual risks are visible and acceptable for current scope.
- Possible next rounds: `regression`, `verification`, `synthesis`, `decision`.

### Regression Round

- Purpose: check nearby behavior and downstream impact.
- Entry criteria: the current work may affect adjacent flows or previously working paths.
- Actions: run focused regression checks, inspect coupled surfaces, compare before/after expectations.
- Output: regression result with any discovered fallout.
- Definition of done: the key adjacent risk surface has been checked.
- Exit criteria: known regressions are either absent, fixed, or explicitly carried forward.
- Possible next rounds: `hardening`, `recovery`, `decision`, `synthesis`.

### Synthesis Round

- Purpose: compress multi-round evidence into a stable decision-ready view.
- Entry criteria: enough evidence exists, but it is still fragmented.
- Actions: merge findings, separate fact from judgment, summarize risks and choices.
- Output: concise structured synthesis.
- Definition of done: a downstream actor can continue without replaying the raw history.
- Exit criteria: next action or final answer is obvious from the synthesis.
- Possible next rounds: `decision`, `synthesis`, `recovery`.

### Decision Round

- Purpose: choose a path when multiple valid directions remain.
- Entry criteria: there is enough evidence to compare options.
- Actions: compare options, tradeoffs, risks, dependencies, and irreducible unknowns.
- Output: chosen direction with rationale and fallback.
- Definition of done: a justified decision is recorded or escalation is clearly required.
- Exit criteria: next path is selected or explicitly escalated.
- Possible next rounds: `planning`, `minimal-slice-execution`, `recovery`, `stop`.

### Recovery Round

- Purpose: restore control after failure, contradiction, or drift.
- Entry criteria: a round failed, assumptions broke, dependencies were missing, or the task lost shape.
- Actions: stop forward motion, identify break point, reduce boundary, reset assumptions, choose a safer next round.
- Output: recovery diagnosis and controlled restart plan.
- Definition of done: the task has a lower-risk next step and a corrected boundary.
- Exit criteria: a re-entry round is selected with documented reasons.
- Possible next rounds: `clarify`, `explore`, `map`, `planning`, `stop`.

## PDCA Requirements Per Round

### Plan

Every round must define:

- `round_goal`
- `primary_objective`
- `in_scope`
- `out_of_scope`
- `deferred_items`
- `assumptions`
- `dependencies`
- `success_criteria`
- `stop_conditions`
- `round_budget`

### Do

Hard limits:

- do only the smallest useful closure for the current round
- do not absorb new major unknowns
- do not expand to side quests because they are nearby
- do not execute through missing evidence or unmet dependencies

### Check

Always inspect:

- goal reached or not
- evidence threshold reached or not
- assumption states updated or not
- side effects and regression surface reviewed or not
- definition of done satisfied or not
- exit criteria met or not

### Act

The round must end with one explicit decision:

- `close_round`
- `partial_pass`
- `rework`
- `replan`
- `split`
- `downgrade`
- `escalate`
- `stop`

## Quality Gate

Every round ends in exactly one gate state.

### PASS

Use only when:

- round goal is met
- evidence meets the threshold
- key assumptions are confirmed or safely bounded
- residual risks are visible and acceptable for advancement

Result: the next round may begin.

### PARTIAL_PASS

Use only when:

- the round achieved enough value to preserve
- remaining items are explicitly classified
- carry-forward items do not invalidate the next round

Result: next round may begin only with documented carry-forward controls.

### FAIL_REWORK

Use when:

- the goal was not met
- the round is still valid
- missing work can be corrected without changing the round boundary

Result: stay in the same round type and same primary objective.

### FAIL_REPLAN

Use when:

- the round boundary or strategy was wrong
- a critical assumption was disproven
- a dependency or unknown invalidates the current plan

Result: no forward progress. Move to an earlier or different round.

Rule: no gate approval means no next round.

## Entry Criteria and Exit Criteria

Use both entry and exit checks. A round is invalid if it has only one of them.

### Common Entry Criteria

- current primary objective is singular
- round type fits the actual problem state
- dependencies for the round are known
- the expected evidence threshold is known
- the round budget is bounded

### Common Exit Criteria

- round output exists in structured form
- assumptions and evidence are updated
- risks and carry-forward classification are explicit
- the gate state is recorded
- the next action is named

### Additional Strict Entry Rules

- do not enter `minimal-slice-execution` if the plan is ambiguous
- do not enter `verification` without explicit success criteria
- do not enter `hardening` before a slice basically works
- do not enter `synthesis` until raw findings have been compressed

## Round Budgeting

Each round has a complexity budget. If it overruns, stop and close, compress, split, or downgrade.

Track these dimensions:

- one primary objective only
- bounded exploration branches
- bounded dependency surface
- bounded change surface
- bounded unresolved assumptions
- bounded raw context inflow
- bounded risk density

Recommended soft limits:

- exploration branches: `<= 3`
- new dependencies introduced in one round: `<= 3`
- major files or systems changed in one execution round: keep to the smallest viable slice
- unresolved critical assumptions at round close: `0` for `PASS`, `<= 1` only for `PARTIAL_PASS`

Budget breach rules:

- if branch count expands, go to `narrowing`
- if dependency surface expands, go to `map` or `planning`
- if change surface expands, split execution
- if unresolved assumptions rise, downgrade to `explore` or `hypothesis`
- if raw context floods, force compression before continuing

## Definition Of Done By Round Type

### Explore Round DoD

- problem boundary is narrower than at entry
- key signals are captured
- raw material is compressed into structured findings
- unknowns still remaining are explicit

### Planning Round DoD

- one current goal only
- in-scope and out-of-scope are explicit
- success criteria are testable
- stop conditions are explicit

### Execution Round DoD

- the bounded action is complete
- the work stayed within boundary
- there is minimum evidence that the action had the intended effect

### Verification Round DoD

- core path was checked
- critical assumptions were validated or rejected
- side effects are recorded
- a gate decision can be justified

### Synthesis Round DoD

- fact, judgment, and next actions are separated
- a downstream actor can resume from the synthesis alone
- raw history no longer needs to be replayed

## Assumption Registry

Every round must update an assumption registry.

### Allowed statuses

- `confirmed`
- `unverified`
- `disproven`
- `deferred`

### Rules

- unverified assumptions must not be reused as established fact
- disproven assumptions must include impact scope
- if a critical assumption is disproven, trigger `FAIL_REPLAN` unless an immediate downgrade resolves the issue safely
- deferred assumptions must have an owner round type or stop condition

Record each assumption as:

| id | statement | status | impact_if_wrong | owner_round |
|---|---|---|---|---|

## Evidence Grading

Every key conclusion must carry an evidence grade.

| Grade | Meaning | Allowed use |
|---|---|---|
| `E0` | guess | cannot justify closure or downstream execution |
| `E1` | weak signal | can guide exploration only |
| `E2` | partial validation | can justify narrowing or temporary prioritization |
| `E3` | direct evidence | acceptable for most execution and verification gates |
| `E4` | strong multi-source validation | required for high-impact conclusions and final claims |

### Minimum evidence thresholds

- `explore`: at least `E1` per non-trivial lead
- `hypothesis`: leading path should reach `E2`
- `minimal-slice-execution`: action rationale should be `E2` or stronger
- `verification`: close with `E3` or stronger for key claims
- `decision`: choose a path with `E2` to `E3`; escalate if still `E1`
- `final closure`: core claims should reach `E3`; use `E4` when impact is high

Weak evidence may guide where to look next. It may not be promoted to settled fact.

## Confidence And Uncertainty Separation

Every round output must classify conclusions into:

- `confirmed`
- `likely`
- `unverified`
- `unknown`
- `needs_external_confirmation`

This keeps inheritance clean:

- confirmed may be reused directly
- likely may guide planning but not be treated as fact
- unverified must be retested before reliance
- unknown must not be hidden
- needs external confirmation blocks closure if it is mission-critical

## Dependency Gate

Before entering any round, run the dependency gate.

Check:

- does this round depend on an unresolved item from a prior round
- does it require user input, approval, or secret material
- does it require tools, permissions, or data not yet available
- does it rely on a critical assumption remaining true
- does it require results from another agent

If a critical dependency is unmet:

- do not start the round
- replan, downgrade, split, or escalate

## Carry-Forward Control

At round close, classify residual items as:

- `safe_to_carry_forward`
- `must_resolve_before_next_round`
- `requires_escalation`
- `invalidates_current_plan`

Rules:

- not every issue is safe to carry forward
- items that block correctness, safety, or dependency integrity must be resolved first
- `invalidates_current_plan` forces `FAIL_REPLAN`

## Depth Consistency Check

At the end of each round, ask:

- did this round only do surface-level work
- was a key verification step skipped
- did rigor drop because of time pressure or fatigue
- is this round materially shallower than earlier rounds
- is the output trustworthy enough to become the next round's input

If depth dropped, do not advance silently. Rework or replan.

## Challenger / Counter-Review

Use a lightweight challenger on critical rounds:

- research
- debugging
- design
- complex analysis
- high-risk execution

Challenge questions:

- could the direction itself be wrong
- did we ignore a simpler explanation
- did we overweight confirming evidence
- did we skip a strong alternative
- does a smaller or safer next slice exist

The challenger does not need a full second workflow. It needs a concise adversarial pass before closure.

## Memory Compression Policy

Round handoff must compress into three layers:

- `facts`
- `judgments`
- `next_actions`

Rules:

- do not paste raw logs unless they are the evidence artifact
- do not replay every branch taken
- preserve only what helps the next round restart correctly

A good handoff lets the next round recover:

- what happened
- what was concluded
- what should happen next

## Round Output Template

Use the canonical templates in [ROUND.md](../templates/ROUND.md) and [round-state.json](../templates/round-state.json).

Minimum required sections:

- current round type
- current round goal
- in scope
- out of scope
- assumptions
- dependencies
- exploration summary / inputs used
- distilled findings
- bounded plan
- actions taken
- evidence and evidence grade
- verification result
- quality gate result
- risks / blockers
- carry-forward classification
- remaining work
- recommended next round type
- final action decision

## Rework, Replan, Split, And Downgrade Rules

### Rework Same Round

Use when:

- the objective still makes sense
- the boundary still makes sense
- execution or verification inside the same round is incomplete

### Replan Same Round

Use when:

- the round objective may still be right
- the current plan is wrong, under-specified, or not executable

### Split Current Round

Use when:

- the round contains multiple main objectives
- the change surface or branch count is too large
- meaningful verification cannot happen in one pass

### Downgrade To Earlier Round Type

Use when later-stage work lost its foundation.

Examples:

- `minimal-slice-execution -> explore`
- `minimal-slice-execution -> planning`
- `verification -> planning`
- `hardening -> map`

The protocol must support moving backward to regain footing. Forward motion is not a value by itself.

## Endgame Control

Before task closure, run the final closure check.

Required checks:

- `objective_closure`
- `evidence_sufficiency`
- `residual_risk_visibility`
- `regression_awareness`
- `handoff_completeness`
- `recommended_follow_up`

Do not close the task if:

- technical action happened but the real objective is still open
- the problem was bypassed rather than solved and that is undocumented
- high-risk leftovers are silent
- the next actor would need to rediscover key context

## Retrospective

After the task, produce a protocol retrospective:

- which round was most likely to lose control
- which rules helped the most
- which rules were insufficient
- which step is easiest to misuse
- how to split rounds better next time
- which stop conditions should be stricter
- which template fields reduced errors the most

This improves future use of the module instead of treating the protocol as static.

## Failure Modes And Guardrails

| Failure mode | Symptoms | Root cause | Prevention rule | Detection rule | Recovery action |
|---|---|---|---|---|---|
| Scope creep | current round keeps expanding | no hard scope boundary | define out-of-scope and stop conditions before doing work | more than one primary objective appears | split or replan |
| Premature execution | changes start before unknowns are reduced | skipped explore/plan | execution requires dependency gate and explicit success criteria | execution started with unresolved critical assumptions | downgrade to explore or planning |
| Analysis paralysis | endless reading without closure | no round budget or exit criteria | every explore round needs branch and time limits | repeated evidence collection without decision movement | move to narrowing or decision |
| Shallow later-stage work | late rounds become hand-wavy | depth drift and fatigue | run depth consistency check every round | later rounds lack evidence compared with earlier ones | rework the shallow round |
| Context flooding | state gets noisy and bloated | raw material is pushed upstream | compress before handoff | handoff contains logs instead of findings | synthesize and rewrite handoff |
| Unverified completion | work is declared done without proof | no verification gate | no closure without evidence threshold | final claims do not cite evidence | add verification or downgrade |
| Fragmented multi-agent work | subagents produce disconnected outputs | no main-thread owner or shared state | one round owner and structured rollup are required | outputs cannot be compared against one current objective | synthesize, remap, or replan |
| Repeated rediscovery | same facts get relearned | weak handoff discipline | every round leaves structured state | repeated exploration reproduces earlier work | write synthesis and update state |
| Weak evidence propagation | guesses become plan inputs | evidence grade missing | all key claims get an evidence grade | later round cites a claim with only E0/E1 support | downgrade and revalidate |
| Hidden assumption propagation | implicit assumptions survive unnoticed | no assumption registry | assumptions must be named and statused | contradiction appears without traceable assumption | update registry and replan |
| Dependency blind spots | round starts but cannot complete | dependency gate skipped | check tools, permissions, user input, and agent results first | blocked mid-round by unmet dependency | stop, gate, and replan |
| Forced forward progress | the protocol keeps moving despite failure | success bias overrides gate logic | no gate means no next round | next round starts after FAIL state | revert to current failed round decision |
| Bad handoff | next actor cannot resume safely | state is vague or missing | required templates and compression | resume needs full chat replay | synthesize and rewrite handoff |
| Poor endgame closure | task stops without real closure | no final closure check | final gate required before finish | objective, risks, or follow-up remain implicit | reopen synthesis/finalization |

## Recommended Module Structure

```text
better work/
  README.md
  activation-policy.md
  templates/
    TASK.md
    PLAN.md
    STATE.md
    HANDOFF.md
    ROUND.md
    round-state.json
  subskills/
    round-based-execution.md
    agents/
      round-challenger.md
      round-worker.md
```

This keeps `round-based-execution` logically subordinate and physically independent.

## Real Usage Examples

### 1. Complex Bug Fix

Why round mode activates:

- multi-file issue
- unclear root cause
- regression risk
- likely requires reproduce, isolate, test, fix, regress

Suggested round flow:

1. `clarify`
   Goal: define the broken behavior, expected behavior, and failure signal.
   Gate: `PASS` only if reproduction target is concrete.
2. `map`
   Goal: trace request path, module ownership, and likely boundary.
   Gate: `PASS` if dependency surface is bounded.
3. `hypothesis`
   Goal: rank the top 2-3 failure explanations.
   Gate: `PASS` if one testable leading hypothesis reaches `E2`.
4. `verification`
   Goal: run the discriminating checks.
   Gate: `FAIL_REPLAN` if the hypothesis tree collapses.
5. `minimal-slice-execution`
   Goal: make the smallest fix.
   Gate: `PARTIAL_PASS` if the bug is fixed but regression surface is unchecked.
6. `regression`
   Goal: verify adjacent paths.
   Gate: `PASS` if no critical fallout.
7. `synthesis`
   Goal: capture cause, fix, evidence, residual risk, next watch items.

Typical downgrade:

- if execution reveals the root cause theory was wrong, go back to `hypothesis` or `map`

Typical closure:

- objective closed
- fix evidence at `E3`
- regression visible
- handoff complete

### 2. Medium-Sized Feature Development

Why round mode activates:

- design plus implementation plus verification
- multiple files or systems
- scope-creep risk

Suggested round flow:

1. `clarify`
   Goal: define user-facing behavior and non-goals.
2. `planning`
   Goal: choose the first minimum slice and verification path.
3. `minimal-slice-execution`
   Goal: implement only the first useful slice.
4. `verification`
   Goal: prove the slice works.
5. `hardening`
   Goal: edge handling and cleanup inside the same bounded surface.
6. `regression`
   Goal: inspect nearby flows.
7. `decision`
   Goal: decide whether a second slice is needed or the feature is closed.

Typical split:

- if one execution round tries to include API, UI, and docs as co-equal goals, split the round

Typical closure:

- shipped slice meets explicit success criteria
- residual follow-ups are visible instead of silently folded in

### 3. Researching an Unfamiliar Codebase

Why round mode activates:

- unknown architecture
- high context flood risk
- likely cross-session continuation

Suggested round flow:

1. `clarify`
   Goal: define the question being asked of the codebase.
2. `explore`
   Goal: gather initial evidence without trying to understand everything.
3. `map`
   Goal: create a dependency and ownership map.
4. `narrowing`
   Goal: reduce focus to the parts relevant to the question.
5. `synthesis`
   Goal: produce a compressed codebase briefing.
6. `decision`
   Goal: decide whether more research, implementation, or escalation is needed.

Typical rework:

- if exploration notes are still raw and not resumable, rework `explore` into compressed findings before proceeding

Typical closure:

- next actor can answer or act from the synthesis alone

### 4. Writing a Complex Analysis Report

Why round mode activates:

- multiple evidence sources
- risk of biased or weak conclusions
- challenger review is valuable

Suggested round flow:

1. `clarify`
   Goal: define the exact question and audience.
2. `explore`
   Goal: gather source material and claims.
3. `narrowing`
   Goal: remove irrelevant lines of inquiry.
4. `synthesis`
   Goal: structure the findings.
5. `decision`
   Goal: settle the report thesis and recommendation.
6. `verification`
   Goal: challenger pass on evidence quality and alternative explanations.
7. `synthesis`
   Goal: close the report with explicit confidence, uncertainty, and follow-up.

Typical downgrade:

- if challenger reveals unsupported claims, go back to `explore` or `narrowing`

Typical closure:

- conclusions are separated into confirmed, likely, and unknown

### 5. Cross-System Troubleshooting / Operations Diagnosis

Why round mode activates:

- multiple systems and owners
- dependency blind spots
- recovery must be verified incrementally

Suggested round flow:

1. `clarify`
   Goal: define the user-visible incident and impact.
2. `map`
   Goal: map the systems and boundaries involved.
3. `explore`
   Goal: gather logs, metrics, events, and config state.
4. `hypothesis`
   Goal: rank likely failure causes.
5. `decision`
   Goal: choose the safest intervention.
6. `recovery`
   Goal: apply the bounded remediation.
7. `verification`
   Goal: confirm service recovery and watch for reoccurrence.
8. `synthesis`
   Goal: document cause, evidence, intervention, and residual risks.

Typical escalation:

- if required access or approvals are missing, stop before recovery and escalate explicitly

Typical closure:

- service objective is restored
- evidence is recorded
- operational follow-up is visible

## Why This Design

- It keeps Better Work lightweight by default and strict only when complexity justifies it.
- It treats complex work as orchestrated loops, not a monolithic stream.
- It prevents weak evidence and hidden assumptions from compounding across rounds.
- It gives multi-agent work one main-thread control surface.
- It creates resumable state for long tasks and cross-session continuity.
- It makes verification local to each round instead of a rushed final ceremony.
