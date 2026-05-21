# Subagents reference

Chief ships four subagents. Each has a defined role and clear boundaries — they don't cross into each other's territory.

---

## chief-agent

**Role:** Planner and orchestrator.

Reads `AGENTS.md`, `.chief/_rules/`, and the current milestone's goals and contracts. Creates plans, breaks work into tasks, delegates to `builder-agent`, and decides what to do next.

Does **not** write code.

**When to call:**
- Give it a goal: `"Plan milestone 3"`
- Ask it for status: `"What's left in milestone 2?"`
- Change direction mid-milestone: `"Descope the export feature from milestone 1"`

---

## builder-agent

**Role:** Implementer.

Receives a task spec and implements it. Runs unit tests, fixes type and lint errors, and commits. Handles all fast, local, deterministic verification.

Does **not** make architecture decisions. Does **not** run integration tests or UI flows.

**When to call:**
```
builder-agent: implement task-1 from milestone-1
```

Or via `/chief-autopilot` (called automatically).

---

## tester-agent

**Role:** Integration and environment verifier.

Runs integration tests, validates API responses, tests UI flows, checks environment-level behavior. Handles slow, non-deterministic, real-world verification.

Does **not** write code. Does **not** run unit tests, lint, or build.

**Only triggered when you explicitly request it** — chief-agent does not call tester-agent automatically.

**When to call:**
```
tester-agent: validate milestone-1
```

Use when unit tests aren't enough and you need real-world coverage.

---

## answer-verifier-agent

**Role:** Background answer verifier.

Spawned by `/chief-grill` — one instance per question, running in the background while the main grill session continues. Receives only the most recent question and answer as context (not the full session) to stay focused.

Checks whether an answer is grounded in the actual codebase, not just plausible in general. Writes its findings to the grill session file.

**Not called manually.** Triggered automatically by `/chief-grill`.

> This agent replaces `review-plan-agent`, which is deprecated as of v4.

---

## Compatibility

| Coding agent | How subagents are wired |
|---|---|
| Claude Code | `.claude/agents/` symlinks → `.agents/agents/` |
| GitHub Copilot | `.github/agents/` symlinks or copies → `.agents/agents/` |
| Other agents | Read `.agents/agents/*.md` directly (if supported) |

`/chief-install` handles this wiring automatically for Claude Code and GitHub Copilot.
