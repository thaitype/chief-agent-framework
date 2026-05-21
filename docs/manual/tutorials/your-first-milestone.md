# Your first milestone with Chief

In this tutorial, we will install Chief into an existing project, set up project context, plan a milestone, and hand it off to an AI agent to build — start to finish.

By the end, you will have:

- Chief installed and wired into your coding agent
- A `.chief/project.md` with your project's context
- A planned milestone with goals, contracts, and tasks
- Code committed by `builder-agent`

This tutorial uses **Claude Code** as the coding agent. The steps are the same for other agents; the agent-specific setup happens automatically during install.

---

## Step 1 — Install the Chief skills

In a terminal inside your project:

```bash
npx skills@latest add thaitype/chief
```

The picker opens. Select all skills (press Space on each, or press `a` to select all). Make sure `chief-install` is included — it's the skill that wires Chief into your agent.

> If you only want specific skills, select `chief-install` at minimum, plus any skills you plan to use (e.g. `chief-plan`, `chief-grill`).

---

## Step 2 — Run /chief-install

Open your coding agent (Claude Code, Copilot, Cursor, etc.) in this project and run:

```
/chief-install
```

The skill will ask three questions:

1. **Which coding agent?** — Select yours. Claude Code sets up a `CLAUDE.md → AGENTS.md` symlink and wires up agent/skill symlinks automatically.
2. **Install mode?** — Choose `symlink` if supported, `copy` otherwise.
3. **Install subagents?** — Yes. This installs `chief-agent`, `builder-agent`, `tester-agent`, and `answer-verifier-agent`.

When it finishes, your project has `AGENTS.md` and the subagent definitions — but no `.chief/` folder yet. That's intentional: Chief creates it only when you need it.

---

## Step 3 — Bootstrap project context

Run:

```
/chief-init
```

The skill interviews you about your project. It asks things like:

- What language and framework are you using?
- What does `npm run dev` look like?
- What are the key architectural constraints?

Answer in plain sentences — the skill writes the answers to `.chief/project.md`. Subagents read this file before doing anything, so they start every session already knowing your project.

> You can skip this step and write `.chief/project.md` by hand later. But a 2-minute interview now saves repeating yourself every chat.

---

## Step 4 — Plan a milestone

Now plan what you're building. If you know exactly what you want, go straight to planning:

```
/chief-plan
```

If your goal is still fuzzy — or there are design decisions you haven't made yet — run a grill session first:

```
/chief-grill
```

`/chief-grill` walks the decision tree one question at a time, critiques its own suggested answers, and verifies them against your codebase via a background `answer-verifier-agent`. When the session ends, the outcome feeds directly into `/chief-plan`.

`/chief-plan` then guides you through:

1. **Goals** — What this milestone delivers. You review and approve.
2. **Contracts** — API shapes, data models, constraints. You review and approve.
3. **TODO** — Optional. Batch breakdown of the work.
4. **Tasks** — Individual units of work for `builder-agent` to implement.

At each step, the skill waits for your sign-off before proceeding.

---

## Step 5 — Build

With the plan approved, delegate to `builder-agent`:

```
builder-agent: implement task-1 from milestone-1
```

Or, if you want the agent to run through all tasks autonomously:

```
/chief-autopilot
```

`/chief-autopilot` reads the goals and contracts, creates tasks if they don't exist, and runs them in sequence. Use this when the plan is clear and you don't need to approve each step.

---

## Step 6 — Reflect

After the milestone is done, run a retrospective:

```
/chief-retro
```

The skill compares what was delivered to what was planned, surfaces lessons learned, and proposes any new rules that should be added to `.chief/_rules/`. You choose which ones to keep.

Rules added here apply to every future milestone — so the same mistake doesn't happen twice.

---

## What you built

Your project now has:

```
project/
├── AGENTS.md               ← framework rules
├── .chief/
│   ├── project.md          ← your project context (written by /chief-init)
│   └── milestone-1/
│       ├── _goal/          ← what this milestone delivers
│       ├── _contract/      ← agreed API shapes and constraints
│       ├── _plan/
│       │   └── _todo.md    ← task list
│       └── _report/        ← retro output
```

From this point, every future milestone follows the same shape. Your agents already know where to read, where to write, and what the rules are.

---

## Next steps

- [How to pick the right skill for your situation](../how-to/pick-the-right-skill.md)
- [How to capture a decision as a permanent rule](../how-to/capture-a-rule.md)
- [Why Chief exists](../explanation/why-chief.md)
