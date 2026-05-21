# The three-agent model

Chief uses three distinct subagent roles: Chief, Builder, and Tester. The separation is intentional — each role has a narrow responsibility and clear boundaries.

## Why separate agents at all?

A single general-purpose agent that plans, builds, tests, and decides is appealing in theory. In practice, it creates feedback loops that don't converge. An agent that writes code, then tests it, then decides whether the tests are good enough, and then plans what to fix — all in one context — is doing too much. Each role bleeds into the next. The agent loses the ability to be honest about quality, because it's emotionally (context-wise) invested in the code it just wrote.

Separate agents avoid this. Each agent has a defined scope. It can give an honest, unbiased assessment of work it didn't produce.

## The three roles

### Chief-agent — planner and orchestrator

Chief reads rules, reads goals, and makes plans. It breaks work into tasks and delegates to Builder. When something is ambiguous, it escalates to the human rather than guessing.

Chief does **not** write code. If it starts implementing, it's drifting out of role. The discipline of "Chief plans, Builder builds" keeps each role honest.

### Builder-agent — implementer

Builder receives a task spec and implements it. It runs unit tests, fixes type and lint errors, and commits. All fast, local, deterministic verification is Builder's responsibility.

Builder does **not** make architecture decisions. If it encounters a decision (which library? which pattern?), it stops and asks Chief, who escalates to the human if needed. Builder builds what it was told to build, exactly.

### Tester-agent — real-world verifier

Tester runs integration tests, validates API responses, tests UI flows — anything that requires a real environment. It handles slow, non-deterministic, environment-level checks that can't be run locally in a unit test.

Tester does **not** write code. It does not fix bugs it finds — it reports them. It does not run unit tests (those are Builder's job). And crucially: **Tester is only triggered when you explicitly ask for it.** Chief does not auto-delegate to Tester, because real-world test runs are slow and expensive.

## The fourth agent: answer-verifier-agent

`answer-verifier-agent` is a special-purpose agent spawned by `/chief-grill`. It's not part of the main planning/building/testing cycle. Its only job is to verify, per-question, whether an answer given during a grill session is actually supported by the codebase — not just plausible in general.

It runs in the background, one instance per question, with a narrow context window (just the current question, not the full session). This prevents it from becoming biased by prior answers.

## The boundary that matters most

The most important boundary is between Tester and everyone else: **unit tests, lint, type checks, and build are Builder's job; integration and real-world validation are Tester's job**.

This is violated most often when people add integration test steps to Builder's task specs. That creates slow feedback loops and non-deterministic behavior inside what should be a fast, local verification step. Keep them separate.

## When to involve Tester

- After a milestone with user-facing behavior or API surface
- When the team needs confidence before a deploy
- When something that should work doesn't, and unit tests pass

Never automatically. Always by explicit request.

---

See also: [Why Chief exists](why-chief.md), [Pre-coding first](pre-coding-first.md), [Subagents reference](../reference/agents.md)
