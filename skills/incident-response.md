# Skill: Incident Response

**Domain:** Ops  
**Updated:** 2026-05-20

## When to use
On-call incidents: RCA, stakeholder comms, post-mortems, runbook generation.

## Prompt templates

### Root cause analysis
```
Walk me through a root cause analysis for this incident using the 5-whys method.
Service: [SERVICE]
Environment: [prod/staging]
Symptoms:

[DESCRIBE SYMPTOMS / PASTE LOGS / PASTE ALERTS]
```

### Stakeholder incident update
```
Write an incident communication update for stakeholders.
Tone: factual, calm, no technical jargon.

Service affected: [NAME]
Impact: [DESCRIBE — who is affected, what they can't do]
Current status: [Investigating / Mitigating / Resolved]
Next update: [TIME]
```

### Post-mortem document
```
Write a post-mortem following the 5-whys format.
Include: executive summary, timeline, impact, root cause, contributing factors,
action items with owners and due dates.

Timeline:
[PASTE]

Impact:
[DESCRIBE]

Fix applied:
[DESCRIBE]
```

### Generate a platform runbook
```
Generate an operational runbook for [SERVICE/SCENARIO].
Include: pre-requisites, step-by-step recovery steps, verification commands,
escalation path, and rollback procedure.
Stack: Trino · Kestra · DBT · Kubernetes
```

### Blameless RCA facilitation
```
Facilitate a blameless RCA session for this incident.
Ask me clarifying questions one at a time to build the full picture,
then produce the post-mortem document.

Starting context:
[BRIEF DESCRIPTION]
```

## Severity levels

| Level | Definition | Response SLA |
|-------|-----------|--------------|
| P1 — Critical | Production data pipeline down, data loss risk | Immediate |
| P2 — High | Significant degradation, SLA at risk | < 30 min |
| P3 — Medium | Non-critical service degraded | < 2 hours |
| P4 — Low | Minor issue, workaround available | Next business day |

## Incident communication checklist
- [ ] P1/P2 acknowledged within SLA
- [ ] Stakeholder update sent within 15 min of P1 declare
- [ ] Status page / channel updated
- [ ] Engineering lead notified
- [ ] Post-mortem scheduled within 48 hours of resolution
- [ ] Action items logged with owners and due dates
