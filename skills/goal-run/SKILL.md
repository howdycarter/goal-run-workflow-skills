---
name: goal-run
description: One-shot durable execution wrapper for Codex goals. Use this skill whenever the user asks to run a goal end-to-end, run Goal Define plus Goal Ultra plus Swarm, create or activate a Codex /goal and execute it, use a "goal-run" command, or turn a substantial objective into a persistent goal with verifier, proof, and authorized agents. This skill sequences Goal Define, Goal Ultra, and Swarm; it does not replace their source-of-truth responsibilities.
---

# Goal Run

Created by [Howdy Carter](https://howdycarter.com).

Use this skill as the one-shot wrapper for substantial objectives that should become active Codex goals and then be executed with the right amount of durability and delegation.

Goal Run is a coordinator:

```text
Goal Define = clarify the objective and acceptance criteria.
Goal Ultra = activate or resume the durable parent goal with verifier and proof.
Swarm = orchestrate authorized agents or threads after the parent goal exists.
Goal Run = run that sequence without making the user manually invoke each layer.
```

## Fit

Use Goal Run when the user asks for any of these:

- `goal-run`
- "run this as a goal"
- "set the Codex /goal and run it"
- "use Goal Define, Goal Ultra, and Swarm"
- "make this durable and use agents"
- "take this objective end to end"
- "create a persistent goal and finish it"

Do not use Goal Run for quick one-shot debugging, tiny edits, ordinary Q&A, or tasks with no credible verifier unless the user explicitly asks to create or run a goal.

## Required Sequence

1. Use Goal Define first.
   - Convert the request into a concrete objective.
   - Fill acceptance criteria, deliverable, constraints, non-goals, verifier, completion proof, approval gates, and exact objective.
   - Emit or preserve a `Goal Define Handoff For Goal Ultra` block.

2. Use Goal Ultra second.
   - Import the latest Goal Define handoff or any provided Goal Ultra packet.
   - Create or activate the persistent Codex goal when the platform supports it.
   - Keep durable verifier, completion proof, blocker standard, and iteration loop as the parent authority.
   - Never set a token budget unless the user explicitly requested one.

3. Use Swarm third, only when authorized and useful.
   - Carry forward the parent objective, verifier, proof, constraints, and approval gates.
   - Use in-chat subagents when the user authorizes agents, delegation, parallel work, or a swarm and there are safe independent workstreams.
   - Create separate Codex threads only when the user explicitly asks for other chats, separate threads, child chats, durable child threads, or follow-up work outside the current thread.
   - If the user forbids threads, use in-chat subagents only or choose no swarm.

4. Run the work.
   - Do not stop at the packet when the user asked to run, build, fix, finish, or execute.
   - Keep the parent agent on the critical path.
   - Verify with the strongest available check that can fail.
   - Mark the persistent goal complete only after the objective and completion proof pass.

## Parent Goal Packet

Before broad work begins, produce a compact parent goal packet with:

- `Fit`
- `Grounding`
- `Outcome`
- `Deliverable`
- `Acceptance criteria`
- `Baseline`
- `Constraints`
- `Non-goals`
- `Implementation path`
- `Verifier`
- `Supporting checks`
- `Completion proof`
- `Approval gates`
- `Blocker standard`
- `Iteration loop`
- `Delegation map`
- `Exact objective`
- `Activation state`

If a field does not apply, write `None` or `Not applicable` with a short reason. Do not collapse the packet into an informal summary.

## Delegation Rules

Goal Run may authorize Swarm only from the user's wording or from an imported handoff that already authorized agents.

Good delegation candidates:

- Requirements/spec review.
- Research for volatile docs, APIs, or external services.
- Design critique or accessibility review.
- Implementation in disjoint files or modules.
- QA preparation or independent verification.
- Review of a completed change.

Do not delegate the immediate critical-path blocker. Do not create overlapping write scopes without a merge plan. Every child prompt created through Swarm must start with:

```text
Create a dedicated goal for this subtask first.
```

## Approval Gates

Pause for explicit approval before:

- Public or production deploys.
- Irreversible or destructive actions.
- Costly external actions.
- Shared-system changes.
- Creating separate Codex threads when not already explicitly requested.
- Weakening the verifier, narrowing scope, or replacing a real check with a mock.

## Reporting

Final reports should include:

- The persistent goal objective that was activated or resumed.
- Which layers ran: Goal Define, Goal Ultra, Swarm.
- Which agents or threads were used, if any.
- What changed or was produced.
- Which verifier and completion proof passed.
- Any skipped checks, assumptions, remaining risks, or active child threads.
