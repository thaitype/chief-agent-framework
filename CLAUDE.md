# CLAUDE.md

## Overview

This project uses a structured **Chief Agent Framework** to enable goal-driven autonomous development with minimal human intervention.

The system is designed so that:

* Humans define **direction, rules, and constraints**
* The **chief-agent** plans and orchestrates work
* Builder and tester agents execute and verify tasks
* The system progresses milestone by milestone with clear contracts and verification

The primary objective is to reduce human involvement in execution while maintaining correctness, safety, and alignment with project goals.

---

# `.chief` Directory Structure

The `.chief` directory contains all structured planning, rules, goals, and execution state.

```
.chief
├── _rules
│   ├── _contract
│   ├── _goal
│   ├── _verification
│   └── _standard
├── _template
└── milestone-1
    ├── _contract
    ├── _goal
    ├── _plan
    ├── task-1
    └── task-2
```

Multiple milestones may exist.

A milestone can be:

* a simple numeric milestone (milestone-1, milestone-2)
* or a real ticket reference (e.g. `milestone-PROJ-1234`)

---

# Rules Hierarchy Priority

Rules must always be resolved using the following priority:

1. **CLAUDE.md** (highest authority)
2. `.chief/_rules`
3. `.chief/milestone-X/_goal` (lowest authority)

Example:
If CLAUDE.md states:

> "Do not use MongoDB ObjectId in service layer"

But `.chief/_rules` states:

> "MongoDB ObjectId may be used in some cases"

Then **CLAUDE.md always overrides**.

---

# Human vs AI Responsibilities

## Human Responsibilities

Humans focus primarily on:

* Writing and refining `CLAUDE.md`
* Maintaining `_rules`
* Defining goals clearly

Humans should not micromanage implementation details.

Clear rules and goals allow agents to work autonomously and safely.

## AI Responsibilities

AI agents must:

* Follow CLAUDE.md strictly
* Follow `.chief/_rules`
* Follow milestone goals and contracts
* Execute tasks safely and correctly
* Ask for clarification only when multiple valid paths exist

---

# `.chief/_rules` Directory

This directory defines global rules that apply to all milestones.

It contains four subfolders:

### `_standard`

General rules shared across all milestones:

* coding standards
* security policies
* database access rules
* architectural constraints

### `_goal`

General high-level goals shared across milestones.

### `_contract`

Shared system contracts:

* data models
* API contracts
* schema definitions
* system conventions

### `_verification`

Defines how work must be verified:

* test commands
* build requirements
* lint/type requirements
* definition of done

### Writing style rules for all rule files

All markdown inside `_rules` must be:

* concise
* structural
* clear
* not overly verbose
* include small code examples when useful
* eliminate ambiguity

Anything unclear may lead to incorrect autonomous decisions.

---

# `.chief/milestone-X` Directory

Each milestone has its own directory.

```
milestone-X
├── _contract
├── _goal
├── _plan
├── task-1
└── task-2
```

## `_contract`

Milestone-specific contracts.
May be more detailed than global contracts, but must never conflict with them.

Examples:

* API schema for this milestone
* DB schema
* service boundaries

## `_goal`

Milestone-specific goals.
More detailed than global goals but must not conflict.

## `_plan`

Execution plan and task list for this milestone.

## task folders

Each task folder contains implementation details and outputs related to that task.

---

# `.chief/milestone-X/_plan` Directory

Contains planning and execution tracking.

### Files

* `_todo.md` → main checklist for milestone
* `task-1.md`, `task-2.md`, etc → detailed task specs

### `_todo.md` Example

```md
# TODO List for Milestone X

- [ ] task-1: implement authentication module
- [ ] task-2: set up database schema
- [ ] task-3: write unit tests for user service
```

Chief-agent must update `_todo.md` by marking completed tasks:

```
[x]
```

Tasks should be kept small and clear.

## Task Output 

each task can have file output when needed, the output should be placed at `.chief/milestone-X/task-Y/`

---

# CLAUDE.md Purpose

CLAUDE.md is the highest authority file.

It should NOT contain excessive detail.

It should contain:

* system overview
* architecture overview
* important rules
* tech stack
* directory structure
* how to run/test

Detailed rules belong in `.chief/_rules`.

---

# Agents

## 1. chief-agent (Planner / Orchestrator)

Recommended model: most capable available (e.g. Opus)

Responsibilities:

* Read CLAUDE.md and all rules
* Analyze milestone goals
* Create `_plan`
* Break work into small tasks (3–5 at a time)
* Assign work to builder-agent and tester-agent
* Review completed work
* Update `_todo.md`
* Decide next steps

Chief-agent is the communication bridge between humans and other agents.

### Critical responsibility

Chief-agent must always evaluate:

> Are there multiple valid implementation paths?

If yes:

* present options to human
* explain pros/cons
* suggest rule clarification if needed
* propose updates to `_rules` or milestone goals

Chief-agent must actively reduce ambiguity.

---

## 2. builder-agent (Implementer)

Recommended model: fast + capable (e.g. Sonnet)

Responsibilities:

* Implement tasks assigned by chief-agent
* Follow CLAUDE.md
* Follow global rules
* Follow milestone contracts and goals
* Write correct and maintainable code

After completion:
→ return results to chief-agent

# Core Design Philosophy

This system is designed so that:

Human defines direction →
Chief-agent plans →
Builder builds →
Tester verifies →
Chief decides →
Repeat

Minimal human intervention.
Maximum clarity and safety.

## Development Commands

> Command to run during development and testing.
> Adjust as needed for your environment.
> TODO: Add Your specific commands here.

## Architecture Overview

> A brief overview of the architecture, key patterns, and important rules.
> TODO: Fill in with your project's architecture details.

### Tech Stack

> List of major technologies used in the project.

### Key Architectural Patterns

> Description of important architectural patterns (e.g., Repository Pattern, Service Layer, etc.)
> TODO: Fill in with your project's architectural patterns.

### Directory Structure

> The main directory structure of the project
> TODO: Fill in with your project's directory structure.

### Important Development Rules

> Key rules that developers must follow.
> TODO: Fill in with your project's important development rules.
