# Skill: Stakeholder Communication

**Domain:** Leadership / Architecture  
**Updated:** 2026-05-20

## When to use
Translating technical work into business language for non-technical audiences:
exec summaries, roadmap narratives, leadership updates, board-level summaries,
cross-team alignment, and presenting architecture decisions upward.

## Prompt templates

### Translate a technical decision for leadership
```
Translate this technical decision into an executive summary for [CTO / VP / Head of Data / Board].
Audience: non-technical. They care about: business impact, risk, cost, and timeline.
Avoid: acronyms, implementation details, jargon.
Length: half a page maximum.

Decision: [PASTE ADR OR DESIGN SUMMARY]
Business context: [WHY THIS MATTERS TO THE BUSINESS]
```

### Write a roadmap narrative
```
Write a roadmap narrative for this quarter's platform work.
Audience: [engineering leadership / product / business stakeholders]
Tone: confident, clear, no jargon.

Initiatives:
- [INITIATIVE 1]: [ONE LINE DESCRIPTION]
- [INITIATIVE 2]: [ONE LINE DESCRIPTION]

Frame each initiative as: problem it solves → what we're doing → business outcome.
```

### Write a status update (written)
```
Write a concise status update for [PROJECT/INITIATIVE].
Audience: [TEAM / MANAGEMENT / LEADERSHIP]
Format: RAG status + 3 bullets (progress, risks, next steps).

Current state:
[PASTE NOTES OR CONTEXT]
```

### Prepare talking points for a meeting
```
Prepare talking points for a [architecture review / leadership sync / cross-team alignment] meeting.
My goal: [WHAT I NEED THE AUDIENCE TO AGREE TO OR UNDERSTAND]
Audience: [WHO WILL BE IN THE ROOM]
Time available: [N minutes]

Context:
[PASTE RELEVANT BACKGROUND]

Output: opening statement, 3 key points with supporting evidence, expected objections + responses,
and a clear ask/call to action.
```

### Write a business case for a technical investment
```
Write a business case for this technical investment.
Audience: [VP Engineering / CTO / Finance]
Frame in terms of: cost reduction, risk mitigation, velocity improvement, or revenue enablement.
Avoid: technical implementation details.

Investment: [DESCRIBE — e.g. migrate to Iceberg, add observability layer, upgrade Trino]
Current pain: [QUANTIFY IF POSSIBLE — e.g. X hours/week lost, Y incidents/month]
Proposed solution: [BRIEF]
Cost: [ESTIMATE]
Timeline: [ESTIMATE]
```

### Explain an incident to leadership
```
Write a leadership-level incident summary.
Audience: non-technical. Tone: transparent, factual, no panic.
Length: 1 paragraph + action items.

Incident: [PASTE POST-MORTEM SUMMARY OR DESCRIBE]
```

### Write a cross-team alignment email
```
Write an alignment email to [TEAM / STAKEHOLDER].
Goal: [GET SIGN-OFF / SHARE DECISION / REQUEST INPUT / ANNOUNCE CHANGE]
Tone: collaborative, not demanding.

Background: [DESCRIBE]
What I need from them: [BE SPECIFIC]
Deadline: [DATE]
```

### Prepare a slide deck outline
```
Create a slide deck outline for [TOPIC].
Audience: [TECHNICAL LEADERSHIP / EXECUTIVES / CROSS-FUNCTIONAL]
Time: [N minutes]
Goal: [INFORM / GET APPROVAL / ALIGN / PRESENT OPTIONS]

Key message I want them to leave with: [ONE SENTENCE]

Context:
[PASTE RELEVANT BACKGROUND]

Output: slide titles, one-line description per slide, speaker notes structure.
```

### Simplify a technical explanation
```
Explain [TECHNICAL CONCEPT/DECISION] to a non-technical stakeholder.
Use an analogy if it helps. No jargon. Maximum 3 sentences.

Concept: [DESCRIBE]
Why it matters to the business: [DESCRIBE]
```

## Communication principles for a Principal Architect

### Writing for non-technical audiences
- **Lead with impact**, not implementation. "This reduces pipeline failures by 80%" before "we're adding a circuit breaker."
- **One message per communication.** State it in the first sentence.
- **Quantify where possible.** "3 incidents per week" beats "frequent incidents."
- **Translate risk into business terms.** "Data delivered 4 hours late" not "SLA breach on the gold layer."
- **State the ask clearly.** Every communication should end with: approve / note / respond by [date].

### RAG status format
```
[GREEN / AMBER / RED] — [Project Name] — [Date]

Progress: [What was completed this period]
Risk: [What could go wrong, if anything]
Next: [What happens next / what's needed]
```

### Objection handling (common patterns)
| Objection | Response frame |
|-----------|---------------|
| "Why does this take so long?" | Complexity + risk of going faster: "We could cut 4 weeks by skipping X, but that increases incident risk by Y." |
| "Can't we just use what we have?" | Cost of inaction: "The current system costs us X hours/week and caused Y incidents last quarter." |
| "This is too expensive." | Cost of not doing it: "The alternative is [RISK/COST]. This investment pays back in [TIMEFRAME]." |
| "Another team does it differently." | Context matters: "Their use case is [X]. Ours is [Y]. The difference is [Z]." |

### Audience calibration
| Audience | What they care about | What to avoid |
|----------|---------------------|---------------|
| CTO / VP Eng | Risk, velocity, team health | Implementation details |
| Head of Data / Product | Delivery timeline, data quality, features | Architecture diagrams |
| Finance | Cost, ROI, timeline | Technical justification |
| Cross-team engineers | Interface contracts, timelines, dependencies | Your internal design choices |
| On-call / ops | Exact commands, clear steps | Business context |
