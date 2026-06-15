---
name: swarm
description: Orchestrate authorized multi-agent Codex work after a parent goal exists. Use this skill whenever the user says "swarm", "multiple agents", "parallel agents", "delegate this", "agent team", "subagents", "other chats", "child goals", "split this into workstreams", or asks Codex to coordinate in-chat agents and/or separate Codex threads. This skill decides when to use in-chat subagents, separate threads, both, or no swarm, while preserving Goal Define and Goal Ultra as the source of truth for objective, verifier, and completion proof.
---

# Swarm

Created by [Howdy Carter](https://howdycarter.com).

Use this skill to turn an authorized parent goal into coordinated child work. Swarm is an orchestration layer, not the source of truth for success.

The sequence is:

```text
Goal Define = what exactly are we doing?
Goal Ultra = how do we make it durable and verifiable?
Swarm = who owns which bounded workstreams?
```

## Fit

Use Swarm when the user authorizes agents, subagents, delegation, parallel work, other chats, child goals, or coordinated workstreams.

Do not use Swarm for ordinary depth, thoroughness, investigation, or "be careful" requests unless the user explicitly authorizes agent or thread delegation. Do not create parallel work just because a task is large; create it only when the split improves speed, quality, or durability without confusing ownership.

Swarm can produce:

- An in-chat subagent plan and active subagents.
- A separate Codex thread plan and created/steered threads when explicitly authorized.
- A hybrid plan with immediate subagents plus durable child threads.
- A no-swarm decision when the work is too small, sequential, tightly coupled, unauthorized, or blocked on a single critical-path decision.

## Parent Goal Requirement

Before child work starts, establish the parent goal.

1. If the recent conversation includes a Goal Define handoff or Goal Ultra packet, import it as the parent goal.
2. If the objective is vague, run the Goal Define pattern first: define outcome, deliverable, acceptance criteria, verifier, completion proof, constraints, non-goals, approval gates, and exact objective.
3. If the work needs retries, waits, reviews, deploys, resumability, durable state, or completion proof outside the agent's claim, run the Goal Ultra pattern before swarm execution.
4. If the parent goal can be defined immediately from the user's request and does not need a full durable packet, write a compact parent goal before decomposing.

Do not redefine success criteria that Goal Define or Goal Ultra already established. If the parent goal is incomplete, fill only the missing orchestration-relevant details or ask only for missing approvals that change delegation boundaries.

## Decision Policy

Choose the lightest useful orchestration mode.

### No Swarm

Choose no swarm when:

- The task is one-shot or clearly sequential.
- The immediate next step is a blocker that the parent should do locally.
- Safe parallel splits would overlap write ownership.
- The user did not authorize agents, delegation, parallel work, or threads.
- A child could not return independently useful evidence.

State the no-swarm reason briefly and continue the parent goal normally.

### In-Chat Subagents

Use in-chat subagents by default when:

- The user authorized agents, subagents, delegation, parallel work, or a swarm.
- There are two or more independent sidecar tracks.
- Each child can return bounded evidence while the parent continues critical-path work.
- Write scopes are disjoint or the child work is read-only.

Prefer in-chat subagents for requirements extraction, design critique, research, QA preparation, focused code patches with disjoint ownership, and independent review.

### Separate Codex Threads

Use separate Codex threads only when the user explicitly asks for other chats, separate threads, child chats, durable child workstreams, or follow-up work outside the current thread.

Separate threads are appropriate when:

- The child work should be visible and resumable as its own user-owned thread.
- The child may wait on external systems or continue after the parent finishes.
- The work needs its own project/worktree boundary.
- The user asked to create or hand off work to another chat.

Do not create threads for routine sidecar tasks. When thread tools are available, use them only after explicit authorization and include the created thread directive in the final response when required by the platform.

### Hybrid

Use both modes when:

- Immediate parallel subagents can advance the current goal now.
- One or more child workstreams also need durable thread ownership.
- The parent can integrate fast-returning child results without waiting for every durable thread.

In hybrid mode, separate immediate subagent work from durable child-thread work so ownership and expectations remain clear.

## Swarm Plan

Before spawning or creating anything, write a compact swarm plan:

- `Parent goal`: imported or newly defined exact objective.
- `Mode`: no swarm, in-chat subagents, separate threads, or hybrid.
- `Reason`: why this mode fits.
- `Parent critical path`: what the parent will keep doing locally.
- `Children`: each child role, subtask, ownership, verifier, stop condition, and return value.
- `Approvals`: any public, irreversible, costly, destructive, shared-system, or thread-creation actions that need explicit user approval.
- `Integration`: how the parent will consume child results and decide completion.

If execution is requested and approval already exists, do not stop at the plan. Start the first useful set of child work, then continue parent critical-path work while children run.

## Child Goal Packet

Every child prompt must start with this exact sentence:

```text
Create a dedicated goal for this subtask first.
```

Then include the complete child goal requirements:

```text
Create a dedicated goal for this subtask first. If the platform supports persistent goals, activate it before doing the subtask; otherwise write the complete child goal packet in your first response.
Child goal packet fields: Fit, Grounding, Outcome, Deliverable, Acceptance criteria, Baseline, Constraints, Non-goals, Implementation path, Verifier, Supporting checks, Completion proof, Approval gates, Blocker standard, Iteration loop, Exact objective, Activation state.
Parent goal: [imported exact objective].
Role: [requirements, research, design, implementation, QA, review, release, or other specific role].
Subtask: [bounded responsibility].
Ownership: [files/modules/systems, or read-only scope].
Verifier: [check or evidence this child must return].
Stop condition: [when to stop instead of expanding scope].
Return: [exact output needed by the parent].
Coordination: You are not alone in the codebase. Do not revert unrelated edits or other agents' work. If you discover overlapping ownership, stop and report it.
```

For separate Codex threads, adapt the same packet into the initial thread prompt and add the thread's expected independence, target project or projectless destination, and whether the parent expects a final artifact, status report, or continuing owner.

## Splitting Work

Use the smallest useful number of children. Two or three children are often enough. More is justified only when workstreams are clearly independent.

Good splits:

- Requirements/spec child: extracts acceptance criteria, edge cases, and risks from the parent goal.
- Research child: checks volatile docs, APIs, assets, external services, or prior art.
- Design child: proposes visual/UX direction, accessibility risks, and validation criteria.
- Implementation child: owns a disjoint file/module/service patch.
- QA child: prepares or runs checks that can proceed while implementation continues.
- Review child: inspects a completed change for correctness, regressions, and missing tests.

Avoid splits where:

- Multiple children edit the same files without a clear merge strategy.
- A child owns the immediate blocker the parent needs before doing anything else.
- The subtask is too vague to verify.
- The return value is a broad opinion instead of usable evidence.

## Execution Discipline

The parent agent owns the swarm.

- Keep the parent on the critical path instead of waiting idly.
- Spawn multiple independent subagents in the same round when useful.
- Wait only when the next parent step depends on child results.
- Do not redo delegated work; inspect, integrate, or reject it.
- Close completed agents when their output has been consumed.
- Prefer user requirements and parent-goal acceptance criteria over child suggestions.
- Do not mark the parent goal complete until the parent verifier and completion proof pass.

When child results conflict, decide locally using the parent goal as the authority. If a conflict changes scope, approval gates, or safety boundaries, pause and ask the user.

## Reporting

Final reports should include:

- The parent goal used.
- Which swarm mode was chosen.
- Which children ran or were created.
- What each child returned that affected the outcome.
- What the parent integrated or rejected.
- Which verifier and completion proof passed.
- Remaining risks, skipped checks, or active child threads.
