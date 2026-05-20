# Incident Update Template

**Prompt to generate an update:**
```
Write an incident communication update for stakeholders.
Tone: factual, calm, no technical jargon.

Service: [NAME]
Severity: [P1/P2/P3]
Status: [Investigating / Mitigating / Monitoring / Resolved]
Impact: [WHO IS AFFECTED AND WHAT THEY CANNOT DO]
What we know: [BRIEF TECHNICAL SUMMARY IN PLAIN ENGLISH]
What we are doing: [CURRENT ACTIONS]
Next update: [TIME]
```

---

## Templates by status

### Initial declare (P1/P2)
```
[INCIDENT DECLARED] [Service Name] — [Severity]

We are currently investigating an issue affecting [SERVICE/FEATURE].

Impact: [WHO, WHAT THEY CANNOT DO]
Status: Investigating
Started: [TIME UTC]

We will provide an update by [TIME UTC].
```

### Ongoing update
```
[UPDATE] [Service Name] — [HH:MM UTC]

We have identified the cause: [PLAIN ENGLISH, NO JARGON].
We are currently [MITIGATION STEPS IN PLAIN ENGLISH].

Impact remains: [STATUS]
Next update: [TIME UTC]
```

### Resolution
```
[RESOLVED] [Service Name] — [HH:MM UTC]

The issue affecting [SERVICE] has been resolved as of [TIME UTC].

Duration: [HH:MM]
Impact: [BRIEF SUMMARY]
Root cause: [ONE SENTENCE, PLAIN ENGLISH]

A post-mortem will be published within 48 hours.
We apologise for the disruption.
```
