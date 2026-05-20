# Principal Architect — Claude Workspace

**Owner:** Vivek Jain (vjain143@gmail.com)  
**Role:** Principal Architect  
**Stack:** Trino · Kestra · DBT · Kubernetes · SonarQube · Python · YAML

---

## What this workspace is

A living productivity toolkit. Skills, templates, and runbooks are added here over time.
Each session, Claude should treat this directory as the source of truth for context, conventions, and reusable prompts.

---

## Directory structure

```
art-and-craft/
├── CLAUDE.md                        ← You are here. Claude reads this first.
├── skills/                          ← One file per domain skill
│   ├── SKILLS.md                    ← Index of all skills
│   ├── pr-review.md
│   ├── sonarqube.md
│   ├── kubernetes.md
│   ├── trino.md
│   ├── kestra.md
│   ├── dbt.md
│   ├── incident-response.md
│   └── health-monitoring.md
├── templates/                       ← Reusable document templates
│   ├── adr.md                       ← Architecture Decision Record
│   ├── rfc.md                       ← Request for Comments
│   ├── post-mortem.md
│   └── incident-update.md
├── runbooks/                        ← Operational runbooks (add as needed)
└── platform-engineer-claude-skills.md  ← Original consolidated reference
```

---

## How to add a new skill

1. Create `skills/<topic>.md` using the template below.
2. Add a one-line entry to `skills/SKILLS.md`.
3. Commit with message: `skill: add <topic>`.

### Skill file template

```markdown
# Skill: <Name>

**Domain:** <Trino | Kestra | DBT | K8s | SonarQube | Architecture | ...>  
**Updated:** YYYY-MM-DD

## When to use
<One sentence.>

## Prompt templates
### <Scenario>
\```
<Paste prompt here. Use [BRACKETS] for variable inputs.>
\```

## Quick reference
<Commands, tables, checklists specific to this skill.>
```

---

## Conventions

- **Paste real output.** Logs, diffs, configs — not descriptions of them.
- **Add context.** Always mention environment (dev/staging/prod) and team constraints.
- **Chain prompts.** RCA output → post-mortem input. Sonar issue → refactor prompt.
- **Severity labels:** `critical` / `warning` / `suggestion` on all review tasks.
- **Environments:** dev · staging · prod (Kubernetes contexts match these names).

---

## Architecture principles (load into every design/review session)

1. **Data contracts first** — schema and SLA agreed before implementation.
2. **Idempotency by default** — all Kestra workflows and DBT runs must be re-runnable safely.
3. **Observability built-in** — every pipeline has a failure alert and a freshness check.
4. **Right-size, don't over-engineer** — choose the simplest materialisation and deployment pattern that meets the SLA.
5. **Runbook before deploy** — no production change without a runbook entry.

---

## Frequently used Claude skills (built-in)

| `/skill` | Use for |
|----------|---------|
| `review` | PR and architecture review |
| `security-review` | Security audit of pending changes |
| `run` | Start and verify the app |
| `verify` | Confirm a fix works end-to-end |
| `simplify` | Clean up changed code |

---

*Update this file whenever the stack, team conventions, or architecture principles change.*
