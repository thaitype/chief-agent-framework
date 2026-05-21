# Directory structure reference

This page describes every file and folder Chief touches, and when each is created.

---

## After `/chief-install`

```
project/
├── AGENTS.md                          ← framework + project rules (highest authority)
├── CLAUDE.md → AGENTS.md             ← symlink (Claude Code only)
├── .agents/
│   └── agents/
│       ├── chief-agent.md
│       ├── builder-agent.md
│       ├── tester-agent.md
│       └── answer-verifier-agent.md
├── .claude/                           ← Claude Code integration
│   ├── agents/ → .agents/agents/*    ← symlinks
│   └── skills/ → .agents/skills/*    ← symlinks (populated by npx skills)
└── .github/
    └── agents/                        ← Copilot integration (symlinks or copies)
```

`.chief/` is **not** created at install time.

---

## After `/chief-init`

```
.chief/
└── project.md     ← tech stack, dev commands, architecture notes
```

---

## After `/chief-plan` (first milestone)

```
.chief/
├── project.md
└── milestone-1/
    ├── _goal/
    │   └── goal.md        ← what this milestone delivers
    ├── _contract/
    │   └── contract.md    ← API shapes, data models, constraints
    ├── _plan/
    │   ├── _todo.md       ← batch/task list
    │   └── task-1.md      ← individual task spec
    └── _report/           ← retro output, investigations
```

---

## After rules accumulate (via `/chief-rule` or `/chief-retro`)

```
.chief/
├── project.md
├── _rules/
│   ├── _standard/         ← coding standards, patterns
│   ├── _contract/         ← global API contracts, data models
│   ├── _goal/             ← long-term direction (spans milestones)
│   └── _verification/     ← test commands, definition of done
└── milestone-N/
```

`_rules/` is empty in small projects. It grows through retros and `/chief-rule` as patterns emerge.

---

## After `/chief-grill`

```
.chief/
└── _grill/
    ├── opened/
    │   └── NNNN-topic.md  ← active grill session (Q&A + verification results)
    └── coach/             ← closed sessions
```

---

## Key rules

- `.chief/` is created lazily — only the files you actually use appear.
- `AGENTS.md` is always present after install. It is the highest-authority file.
- Skills (`.agents/skills/`) are managed by `npx skills`, not by `/chief-install`.
- A canonical example layout lives at [`docs/example-chief/`](../example-chief/) for reference.

---

## File authority

| File / folder | Authority level | Created by |
|---|---|---|
| `AGENTS.md` | Highest | `/chief-install` |
| `.chief/_rules/` | Global | `/chief-rule`, `/chief-retro`, or manual |
| `.chief/milestone-N/_goal/` | Milestone | `/chief-plan` or `chief-agent` |
| `.chief/milestone-N/_contract/` | Milestone | `/chief-plan` or `chief-agent` |
| `.chief/project.md` | Context (not rules) | `/chief-init` or manual |
