# Rules hierarchy reference

Chief enforces a three-level priority order. When rules conflict, higher authority always wins.

---

## Priority order

```
┌─────────────────────────────────┐
│ 1. AGENTS.md  (Project Rules)   │  ← Highest authority
├─────────────────────────────────┤
│ 2. .chief/_rules/               │  ← Global rules
├─────────────────────────────────┤
│ 3. .chief/milestone-N/_goal/    │  ← Lowest authority
└─────────────────────────────────┘
```

No exceptions. Higher priority always wins.

---

## What lives at each level

### Level 1 — `AGENTS.md` Project Rules

Hard constraints. Non-negotiable. Override everything.

```md
## Project Rules

- NEVER use ORM in this project
- All APIs MUST return JSON:API format
- MUST use pnpm, not npm
- Database access ONLY through repository pattern
```

**Put something here when:**
- It's a hard constraint that must never be violated
- It applies across all milestones, all time
- Violating it would cause real damage

---

### Level 2 — `.chief/_rules/`

Detailed standards. Apply across all milestones. Can include examples.

```
.chief/_rules/
├── _standard/       ← HOW to write code
├── _contract/       ← WHAT the interfaces look like
├── _goal/           ← WHERE we're heading (long-term)
└── _verification/   ← HOW to verify correctness
```

**Put something here when:**
- It needs detail, examples, or code snippets
- It applies to all milestones but isn't a hard constraint
- It may evolve over time

---

### Level 3 — `.chief/milestone-N/_goal/`

Scoped to one milestone. Can narrow global rules but must not contradict them.

**Put something here when:**
- It applies only to this milestone
- It's a tactical decision, not a permanent rule

---

## Conflict resolution examples

**Direct override — higher wins:**
```
AGENTS.md:      "NEVER use MongoDB ObjectId in service layer"
_rules/:        "MongoDB ObjectId may be used in some cases"
Result:         ObjectId is never used in service layer. AGENTS.md wins.
```

**Specificity without conflict — all apply:**
```
AGENTS.md:      "All APIs MUST return JSON:API format"
_rules/:        "Pagination MUST use cursor-based approach"
milestone-1:    "Implement user listing endpoint"
Result:         User listing uses JSON:API format with cursor-based pagination.
```

**Milestone narrows scope (valid):**
```
_rules/_goal:   "Support PostgreSQL and MySQL"
milestone-1:    "Focus only on PostgreSQL for now"
Result:         Valid. Milestone narrows scope without contradicting global goal.
```

**Milestone contradicts global (invalid):**
```
_rules/_standard: "All functions MUST have unit tests"
milestone-1:      "Skip tests for prototyping speed"
Result:           Global rule wins. Tests are still required.
```

---

## How agents use the hierarchy

1. `chief-agent` reads all three levels before planning.
2. If ambiguity exists, `chief-agent` escalates to the human.
3. `builder-agent` follows task specs, which already resolve conflicts.
4. No agent may silently ignore a higher-priority rule.
