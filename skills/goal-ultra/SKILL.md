---
name: goal-ultra
description: Design, critique, activate, and run durable Codex goals for persistent or long-running objectives, including Goal Define handoffs. Use when the user says "set a goal", "start a goal", "activate goal mode", "persistent goal", "durable goal", "long-running objective", "goal tree", or asks for verifiers, durable state, completion proof, approval gates, bounded delegation, or parent/child subagent goals. When detailed agent or thread orchestration is needed, hand off to the swarm skill after the durable parent goal is defined.
---

# Goal Ultra

Created by [Howdy Carter](https://howdycarter.com).

Use this skill when the user needs a goal that can survive retries, interruptions, waiting, and parallel work. A durable goal has an observable finish line, a verifier that can fail, and proof that completion happened outside the agent's own assertion.

If the immediately preceding work used Goal Define, Goal Ultra must pull in that Goal Define output before drafting its own packet. Do not make the user restate the Goal Define message.

## Modes

- `Design`: return a goal packet only. Do not activate a persistent goal.
- `Critique`: inspect an existing goal or draft and tighten its outcome, verifier, scope, and proof.
- `Activate`: design and critique the goal, then create the persistent goal when the platform supports it.
- `Run`: activate or resume the goal and keep working until complete or genuinely blocked.
- `Goal tree`: define durable parent/child boundaries when the user explicitly authorizes subagents or parallel work, or when an imported Goal Define handoff includes an authorized delegation map. Use Swarm for detailed child prompts, subagent spawning, separate Codex threads, and integration rules.

Default to `Activate` when the user explicitly invokes the skill for a concrete work objective and asks Codex to build, complete, run, pursue, or do the work. Stay in `Design` when the user asks to draft, discuss, or critique a goal without starting it.

Never set a token budget unless the user explicitly requests one.

## Goal Fit

Use a persistent goal when the work benefits from:

- Multiple attempts or an iteration loop.
- Waiting on external systems, deploys, reviews, or checks.
- Recovery after context loss, interruption, or resume.
- Parallel subgoals or agents.
- A verifier that can fail independently.

Prefer a normal task plan when the work is one-shot, mainly taste-based, blocked on a human choice, or has no credible external verifier.

## Grounding

Before writing the goal, gather just enough evidence to make the finish line real:

- Read the user's exact request, named files, linked artifacts, repo instructions, and existing goal files.
- Inspect the baseline: current state, failing behavior, empty starting point, prior attempt, benchmark, or existing artifact.
- Refresh volatile facts from primary or live sources when they affect the verifier or completion proof.
- Separate observed facts, user requirements, and inferred assumptions.

Ask only when the missing answer changes the finish line, grants consequential approval, or chooses between incompatible goals. Otherwise state the assumption and continue.

## Goal Define Handoff

At the start of every Goal Ultra run, scan the recent conversation for the latest `Goal Define Handoff For Goal Ultra` block or Goal Define goal packet generated immediately before the user invoked Goal Ultra.

If the user provides an existing Goal Ultra handoff or packet, import it as the durable parent goal before deciding whether to run it directly or hand detailed orchestration to Swarm.

When found:

- Treat it as the primary brief and import its goal, deliverable, acceptance criteria, baseline, constraints, non-goals, verifier, completion proof, approval gates, exact objective, and delegation map.
- Preserve the imported objective unless the user's new Goal Ultra request explicitly changes it.
- Carry forward the imported delegation map as authorization to spawn agents only when the original Goal Define request authorized agents, subagents, delegation, or parallel work.
- In `Grounding`, state that the Goal Define handoff was found and list any fields that were missing or superseded by the user's newer request.

When not found, continue from the user's current request and observed baseline. Do not ask the user to paste the prior Goal Define output unless the missing handoff changes the finish line or approval boundary. Do not substitute an informal summary when the labeled handoff is available in recent context.

## Goal Packet

Return or activate goals using this structure:

- `Fit`: persistent goal or ordinary task plan, with rationale.
- `Grounding`: observed state, user requirements, assumptions, and evidence gaps.
- `Outcome`: the specific result to produce.
- `Deliverable`: exact file, app, page, repo change, image, document, deploy, or other output.
- `Acceptance criteria`: concrete pass/fail requirements for the outcome.
- `Baseline`: what is true before the goal starts.
- `Constraints`: requested technologies, destinations, deadlines, safety boundaries, and style requirements.
- `Non-goals`: adjacent work that should not expand scope.
- `Implementation path`: the smallest practical sequence that reaches the outcome.
- `Verifier`: the strongest independent check that can fail.
- `Supporting checks`: commands, previews, screenshots, tests, readbacks, or inspections needed to prove quality.
- `Completion proof`: exact output, path, command result, screenshot, deploy check, readback, or external status required before completion.
- `Approval gates`: public, irreversible, costly, destructive, or shared-system actions that need separate user approval.
- `Blocker standard`: what must be true before declaring the goal blocked.
- `Iteration loop`: what to try next if the verifier fails.
- `Exact objective`: concise text suitable for the goal mechanism.
- `Activation state`: drafted, active, or not recommended.

Fill every field. If a field truly does not apply, write `None` or `Not applicable` with a short reason; do not collapse the packet into a partial summary.

Before activation, red-team the packet. Check whether success could be faked by weakening the verifier, whether the goal misses the user's real outcome, whether approval gates are explicit, and whether completion is observable.

## Durable State

Use durable files when the goal spans more than a short turn, when agents need shared context, or when recovery matters. Prefer existing project conventions. Otherwise use:

```text
GOAL.md    outcome, baseline, constraints, non-goals, success criteria, blocker criteria
WORKLOG.md attempts, evidence, current state, next action
RESULT.md  final change, verification evidence, remaining risks
```

Read existing durable files before editing them. Preserve unrelated dirty work. Keep durable files short and factual; do not copy long transcripts into them.

## Activation

When activating:

1. Create the persistent goal with the exact objective when the platform supports it. If a Goal Define handoff provided an exact objective, use that objective as the starting point and tighten it only for newer user requirements.
2. Keep detailed criteria in the working plan or durable files.
3. Start work immediately unless the user asked only to set the goal.
4. Inspect goal state after resumes, interruptions, or material steering.

Do not proceed under an unrelated existing goal. If a previous goal is active but does not match the request, create the requested goal when possible or state the mismatch and continue with the drafted objective in the working plan.

## Swarm Handoff

Goal Ultra owns durable parent goals, verifiers, approval gates, and completion proof. Swarm owns detailed child orchestration.

Use this section when the parent goal needs agents, subagents, delegation, parallel work, other chats, child goals, or coordinated workstreams:

- Preserve the parent objective, verifier, completion proof, constraints, and approval gates as the authority.
- Carry forward any Goal Define delegation map as authorization only when the original request authorized agents, subagents, delegation, parallel work, or separate threads.
- Keep durable child boundaries lightweight: candidate workstreams, ownership, verifier, stop condition, return value, and whether separate Codex threads were explicitly authorized.
- Use Swarm for detailed agent/thread mode decisions, child goal prompts, subagent spawning, separate Codex thread creation, waiting strategy, and integration rules.

Do not duplicate Swarm's orchestration policy inside the Goal Ultra packet. The parent goal remains responsible for final scope, conflict resolution, verification, and completion.

## Active Goal Discipline

When running a goal:

- Change one meaningful thing at a time when chasing a failing verifier.
- Record evidence from each attempt when durable state exists.
- Do not weaken tests, narrow scope, hide failures, replace real checks with mocks, or change benchmarks without user approval.
- Continue through implementation and verification when the user asked to run or complete the work.
- Mark complete only after the objective and completion proof are satisfied.
- Mark blocked only when the blocker standard is met and no meaningful safe next step remains.

## Reporting

Final responses should state:

- What goal was activated or updated.
- What changed or was produced.
- Which verifier and completion proof passed.
- Any assumptions, skipped checks, or remaining risks.
