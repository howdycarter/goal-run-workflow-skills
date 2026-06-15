# X Launch Thread

## Main Post

I built a tiny OS for Codex agents.

Most agent runs fail because nobody owns:

- what done means
- how to prove it
- what can be delegated
- when to stop

So I open-sourced `goal-run`:

Goal Define -> Goal Ultra -> Swarm -> Run + Verify

Attach: `goal-run-workflow-teaser.mp4`

## Reply 1

Repo:

https://github.com/howdycarter/goal-run-workflow-skills

Install:

```sh
mkdir -p ~/.codex/skills
cp -R skills/goal-run skills/goal-define skills/goal-ultra skills/swarm ~/.codex/skills/
```

Then run:

```text
$goal-run Objective: [what you want done]
```

## Reply 2

The idea is simple:

Prompts are not enough.

Agents need an operating loop:

1. define the goal
2. activate durable state
3. split bounded work
4. verify with proof

That is the whole stack.

Attach: `goal-run-workflow-excalidraw.png`

## Reply 3

The important part:

Swarm does not replace the parent goal.

The parent goal stays the source of truth.

Agents get bounded ownership.

Completion only happens after the verifier and proof pass.

That is what makes it useful for real work.

## Alternative Hooks

1. I built a tiny OS for Codex agents.
2. Vibe coding breaks when nobody owns the finish line.
3. Prompts are not enough. Agents need operating loops.
4. The missing primitive in Codex is not more autonomy. It is proof of done.
5. I open-sourced the workflow I use to keep Codex agents from wandering.
