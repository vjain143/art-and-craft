# Post-Mortem Template

**Prompt to generate a post-mortem:**
```
Write a blameless post-mortem for this incident.
Use 5-whys for root cause analysis. Include all sections below.

Incident ID: [INC-NNN]
Service: [SERVICE]
Duration: [START] → [END] ([DURATION])
Severity: [P1/P2/P3]

Timeline:
[PASTE EVENTS IN CHRONOLOGICAL ORDER]

Impact:
[DESCRIBE]

Fix applied:
[DESCRIBE]
```

---

## Post-Mortem: [Incident Title]

**Incident ID:** INC-NNN  
**Severity:** P1 / P2 / P3  
**Date:** YYYY-MM-DD  
**Duration:** [HH:MM]  
**Author:** [Name]  
**Reviewers:** [Names]  
**Status:** Draft | Under Review | Final

---

### Executive Summary

[2-3 sentences: what happened, business impact, how it was resolved.]

---

### Timeline

| Time (UTC) | Event |
|-----------|-------|
| HH:MM | [Event] |
| HH:MM | [Detected] |
| HH:MM | [Escalated] |
| HH:MM | [Mitigated] |
| HH:MM | [Resolved] |

---

### Impact

- **Users / pipelines affected:**
- **Data loss / corruption:**
- **Downstream services affected:**
- **Duration of customer impact:**

---

### Root cause (5 Whys)

1. **Why did the incident occur?**
2. **Why did [cause 1] happen?**
3. **Why did [cause 2] happen?**
4. **Why did [cause 3] happen?**
5. **Root cause:**

---

### Contributing factors

- [Factor 1]
- [Factor 2]

---

### What went well

- [Detection was fast]
- [Rollback worked cleanly]

---

### What could be improved

- [Detection gap]
- [Runbook was missing]

---

### Action items

| Action | Owner | Due date | Status |
|--------|-------|----------|--------|
| [Add alert for X] | | | Open |
| [Update runbook] | | | Open |
| [Fix root cause] | | | Open |

---

### Lessons learned

[One paragraph on what the team is taking away from this incident.]
