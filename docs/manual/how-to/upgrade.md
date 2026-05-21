# How to upgrade Chief

Upgrading Chief is a two-step process: refresh skills, then upgrade framework files.

---

## Step 1 — Refresh skills

```bash
npx skills@latest add thaitype/chief
```

This pulls the latest version of every Chief skill. The picker shows what changed. Safe to re-run anytime — `npx skills add` is idempotent.

---

## Step 2 — Upgrade framework files

```
/chief-upgrade
```

The skill diffs your framework files (subagents, `AGENTS.md`) against the target version, builds a file-by-file upgrade plan, and waits for your sign-off before changing anything.

Files with your own content — such as your `AGENTS.md` Project Rules section — are flagged for review. Framework-only files default to overwrite.

---

## Pinning a specific version

Both steps support version pinning:

```bash
# Skills — any git ref (tag, branch, or commit)
npx skills@latest add thaitype/chief#v4.0.0
```

```
# Framework files
/chief-upgrade v4.0.0
/chief-upgrade canary
```

---

## What is safe to overwrite

| File | Safe to overwrite? |
|---|---|
| Subagent definitions (`.agents/agents/*.md`) | Yes — overwrite is recommended |
| Skill files (`.agents/skills/**`) | Yes — managed by `npx skills` |
| `AGENTS.md` framework sections | Yes |
| `AGENTS.md` Project Rules section | No — your content, review manually |
| `.chief/project.md` | No — your content |
| `.chief/milestone-*/` | No — your work |

---

## Upgrading from v3

v4 changes the skill install method. Instead of skills bundled inside `/chief-install`, skills are now installed independently via `npx skills`. After running the two steps above:

1. Your existing `.chief/` milestone files are untouched.
2. Your `AGENTS.md` Project Rules are preserved.
3. `review-plan-agent` is deprecated — `answer-verifier-agent` replaces it.

---

## Related

- [How to install Chief](install.md)
- [Directory structure reference](../reference/directory-structure.md)
