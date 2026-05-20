# Skill: Technical Writing

**Domain:** Architecture  
**Updated:** 2026-05-20

## When to use
Writing or improving any technical document: ADRs, RFCs, design specs, one-pagers,
technical proposals, runbooks, API docs, or meeting summaries.

## Prompt templates

### Draft an ADR
```
Draft an Architecture Decision Record for this decision.
Be concise — one page maximum. Use the ADR format from templates/adr.md.

Decision: [WHAT WAS DECIDED]
Context: [WHY THIS DECISION WAS NEEDED — constraints, drivers]
Alternatives considered: [LIST]
Outcome: [WHAT WAS CHOSEN AND WHY]
```

### Draft an RFC
```
Draft an RFC for this proposal.
Audience: [engineering team / leadership / cross-team]
Use the RFC format from templates/rfc.md.

Proposal: [WHAT YOU WANT TO CHANGE OR BUILD]
Problem: [WHAT IS BROKEN OR MISSING]
Constraints: [DEADLINE, TEAM SIZE, BUDGET, TECH STACK]
```

### Write a design spec (one-pager)
```
Write a one-page technical design spec for this component/feature.
Audience: [engineers / tech leads]
Stack: Trino · Kestra · DBT · Kubernetes

Requirements:
- [FUNCTIONAL REQ]
- SLA: [LATENCY / FRESHNESS / AVAILABILITY]

Output: problem statement, proposed design, key decisions, open questions, success criteria.
```

### Improve existing technical writing
```
Improve this technical document.
Fix: clarity, structure, passive voice, missing context, and assumed knowledge.
Keep the technical depth — don't dumb it down.
Audience: [engineers / tech leads / leadership]

[PASTE DOCUMENT]
```

### Write a technical proposal
```
Write a technical proposal for [INITIATIVE].
Audience: [engineering leadership / CTO / Head of Data]
Include: problem statement, proposed solution, business value, risks, timeline, and ask.
Keep it under 2 pages. Lead with the business impact, not the technology.

Context:
[DESCRIBE]
```

### Summarise a long technical document
```
Summarise this technical document.
Output two versions:
1. TL;DR (3 bullet points) for a busy reader
2. Executive summary (1 paragraph) for non-technical leadership

[PASTE DOCUMENT]
```

### Write a runbook
```
Write an operational runbook for [SERVICE/SCENARIO].
Structure: pre-requisites → detection → diagnosis steps → remediation commands → verification → rollback.
Assume the on-call engineer is competent but does not know this service deeply.
Stack: [TRINO / KESTRA / DBT / KUBERNETES]
```

### Write API documentation
```
Write API documentation for this endpoint/service.
Include: description, request/response schema, example curl call, error codes, and rate limits.
Format: OpenAPI-compatible markdown.

[PASTE CODE OR ENDPOINT SPEC]
```

### Write a meeting summary / decision log
```
Write a structured meeting summary and decision log from these notes.
Include: decisions made (with rationale), action items (owner + due date), open questions, and next steps.

Raw notes:
[PASTE]
```

## Writing quality checklist
- [ ] Lead with the "so what" — problem or decision first, details second
- [ ] One idea per paragraph
- [ ] Active voice: "We decided X" not "It was decided that X"
- [ ] No jargon without definition for the target audience
- [ ] All acronyms spelled out on first use
- [ ] Open questions explicitly listed — don't bury ambiguity
- [ ] Action items have an owner and a due date

## Document type → template mapping
| What you need | Template | Skill prompt |
|---------------|----------|-------------|
| Record a decision | `templates/adr.md` | "Draft an ADR" |
| Propose a change | `templates/rfc.md` | "Draft an RFC" |
| Explain a component | Design spec (above) | "Write a design spec" |
| Incident post-mortem | `templates/post-mortem.md` | `skills/incident-response.md` |
| Stakeholder update | `templates/incident-update.md` | `skills/stakeholder-comms.md` |
