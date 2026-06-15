---
name: goal-define
description: Convert vague or templated build requests into a concrete execution goal, acceptance criteria, verifier, completion proof, reusable Goal Ultra handoff, and lightweight delegation readiness. Use when the user asks to "define goal", "write yourself a new goal", use "/goal", set a durable or persistent goal, turn a build prompt into an actionable goal, or explicitly asks Codex to spawn agents/subagents in parallel for a build, design, app, game, site, automation, document, or deliverable. When detailed agent or thread orchestration is needed, hand off to the swarm skill after the parent goal is defined.
---

# Goal Define

Created by [Howdy Carter](https://howdycarter.com).

Use this skill to turn an open-ended build prompt into an executable objective, then carry straightforward work through implementation and verification when the user asks for execution. For long-running or interruption-prone work, produce the Goal Ultra handoff so Goal Ultra can own durable state, activation, retries, and completion proof.

Every Goal Define output must leave behind a reusable handoff message that Goal Ultra can import later. Do not rely on an informal summary when a structured handoff is possible.

## Intake Template

Recognize prompts shaped like:

```text
Build [THING] in [TECH/FRAMEWORK]. It should include [MAIN FEATURES], with [INTERACTION/ANIMATION/BEHAVIOR DETAILS]. Make it feel [MOOD/QUALITY], using [VISUAL DETAILS], [ENVIRONMENT DETAILS], and [EXTRA EFFECTS]. Output as [FORMAT/FILE TYPE].
```

Treat every filled placeholder as a requirement. If a placeholder is missing, infer a conservative default from the surrounding request unless the missing value changes the deliverable, safety profile, or destination.

## Define The Goal

Before broad work begins, write a single goal that is specific enough to guide execution and hard to satisfy accidentally:

```text
Goal: Build [thing] in [tech/framework] as [format], with [main features], [required interactions/behaviors], and a [mood/quality] visual treatment using [visual/environment/effects constraints].
Done when: [deliverable exists], [core interactions work], [visual/format constraints are met], and [primary verifier plus completion proof have passed].
```

When the platform exposes a persistent goal mechanism and the user asks for a new goal or `/goal`, create the short parent objective before implementation. If the goal needs durability, retries, external waits, or resumability, hand it to Goal Ultra instead of managing durable state here.

Do not proceed under an unrelated existing goal. If a previous goal is active but does not match the request, create the new requested goal when the platform allows it, or state the mismatch and continue with the defined goal in the working plan.

## Goal Fit

Recommend Goal Ultra when the work benefits from retries, waiting, recovery after interruption, parallel subgoals, or a verifier that can fail independently. Prefer an ordinary task plan when the work is one-shot, mainly taste-based, blocked on human choices, or lacks a credible external check.

If the user asks only to design or critique a goal, return a goal packet and do not activate it. If the user asks to build, complete, run, or do the work, activate the goal before continuing when the platform supports it.

## Plan The Work

Turn the prompt into:

- `Deliverable`: exact file, app, page, repo change, image, document, or other output.
- `Acceptance criteria`: concrete checks derived from the user's features, behavior, mood, visuals, environment, effects, and format.
- `Baseline`: current state, empty starting point, failing behavior, existing artifact, or metric being improved.
- `Implementation path`: the smallest practical sequence that reaches the deliverable.
- `Primary verifier`: strongest independent check that can fail.
- `Supporting checks`: commands, previews, screenshots, tests, linting, readbacks, or manual inspection needed to prove quality.
- `Completion proof`: exact output, path, command result, screenshot, deploy check, or readback required before marking the goal complete.
- `Approval gates`: public, irreversible, costly, destructive, or shared-system actions that need separate user approval.

For frontend, visual, game, or interactive requests, include visual validation in the plan. Use browser or screenshot checks when a local preview is available.

## Goal Ultra Handoff

Goal Define does not own durable state. When the work needs persistence, retries, waits, reviews, deploys, resumability, or proof outside the agent's own claim, emit the reusable `Goal Define Handoff For Goal Ultra` block and let Goal Ultra decide durable files, activation, worklog, and completion proof.

## Goal Packet

When returning or activating a goal, include:

- `Fit`: persistent goal or ordinary task plan, with a short rationale.
- `Grounding`: observed state, user requirements, assumptions, and evidence gaps.
- `Outcome`: the specific result to produce.
- `Deliverable`: exact file, app, page, repo change, image, document, or other output.
- `Acceptance criteria`: concrete checks derived from the user's features, behavior, mood, visuals, environment, effects, and format.
- `Baseline`: current state, empty starting point, failing behavior, existing artifact, or metric being improved.
- `Constraints`: user requirements, technology, format, style, safety boundaries, and destinations.
- `Non-goals`: adjacent work that should not expand scope.
- `Implementation path`: the smallest practical sequence that reaches the deliverable.
- `Primary verifier`: strongest independent check that can fail.
- `Supporting checks`: commands, previews, screenshots, tests, linting, readbacks, or manual inspection needed to prove quality.
- `Completion proof`: exact output, path, command result, screenshot, deploy check, or readback required before marking the goal complete.
- `Approval gates`: public, irreversible, costly, destructive, or shared-system actions that need separate user approval.
- `Blocker standard`: what must be true before declaring the goal blocked.
- `Iteration loop`: what to try next if the verifier fails.
- `Delegation map`: lightweight authorized workstreams only; use Swarm for detailed child prompts, agent/thread decisions, and orchestration.
- `Exact objective`: concise text suitable for the goal mechanism.
- `Activation state`: drafted, active, or not recommended.

Before activation, red-team the packet: check whether success could be faked by weakening the verifier, whether the wording misses the user's real outcome, whether approval gates are explicit, and whether completion is observable outside the agent's own claim.

Fill every field. If a field truly does not apply, write `None` or `Not applicable` with a short reason; do not collapse the packet into a partial outline.

## Goal Define Handoff For Goal Ultra

Whenever Goal Define returns or activates a goal, emit a top-level reusable message block with the exact heading `Goal Define Handoff For Goal Ultra`. Goal Ultra must be able to import this block from the recent conversation without asking the user to restate it. Do not substitute a summary, file path, or informal plan reference.

The handoff block must contain:

- `Goal`: the concise executable objective.
- `Done when`: acceptance criteria plus verifier and completion proof.
- `Deliverable`: exact output, path, app, page, repo change, document, or artifact.
- `Acceptance criteria`: concrete pass/fail requirements for the deliverable.
- `Baseline`: current state or starting point.
- `Constraints`: user requirements, technology, format, style, safety boundaries, and destinations.
- `Non-goals`: adjacent work excluded from scope.
- `Primary verifier`: the strongest check that can fail.
- `Supporting checks`: secondary commands, previews, screenshots, tests, readbacks, or inspections.
- `Completion proof`: exact evidence required before completion.
- `Approval gates`: actions that need separate user approval.
- `Delegation map`: authorized workstreams, ownership, verifier, stop condition, and return value; write `None` when agents are not authorized or not useful. Keep this lightweight and hand detailed orchestration to Swarm.
- `Exact objective`: concise text suitable for the persistent goal mechanism.

If agent work is authorized by explicit user wording or by a build-template/parallel-agent instruction, the `Delegation map` must identify independent candidate workstreams whenever the work naturally splits into safe parallel tracks. If detailed agent prompts, subagent spawning, separate threads, or child goals are needed, use Swarm after this handoff instead of expanding the orchestration here.

## Swarm Handoff

Goal Define owns the parent objective. Swarm owns detailed orchestration.

When agent work is authorized, keep the Goal Define `Delegation map` lightweight:

- Candidate workstreams.
- Ownership boundaries.
- Verifier or evidence expected from each workstream.
- Stop condition.
- Return value needed by the parent.
- Whether separate Codex threads were explicitly authorized.

Then use Swarm when the next step requires child goal packets, subagent spawning, separate Codex threads, or integration rules. Do not duplicate Swarm's detailed agent/thread decision policy here.

## Result Synthesis

For ordinary non-delegated execution, summarize the implemented result, verifier, and remaining risk. If delegated work is involved, use Swarm for child-result integration, conflict resolution, waiting strategy, and agent cleanup.

## Active Goal Discipline

When working under an active goal:

- Inspect goal state after resumes, interruptions, or material steering.
- Change one meaningful thing at a time when chasing a failing verifier.
- Record evidence from each attempt when Goal Ultra has established durable state.
- Do not weaken tests, narrow scope, hide failures, replace real checks with mocks, or change benchmarks without user approval.
- Mark complete only after the goal objective and completion proof are satisfied.
- Mark blocked only when the blocker standard is met and no meaningful safe next step remains.

## Execution Rules

- Preserve the user's requested technology, format, mood, and interaction details unless impossible.
- Ask only for missing information that cannot be safely inferred.
- Do not stop at a plan when the user asked to build or update; implement, verify, and report the result.
- Keep generated artifacts in the requested destination or, for projectless work, in the workspace outputs folder when the artifact is user-facing.
- State any unverified assumptions or checks that could not be run.
