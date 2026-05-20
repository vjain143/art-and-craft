# Skill: Tech Evaluation & Build vs Buy

**Domain:** Architecture  
**Updated:** 2026-05-20

## When to use
Evaluating new tools, frameworks, or platforms; structured build vs buy decisions;
vendor assessments; tech radar updates; deprecating existing technology.

## Prompt templates

### Structured build vs buy decision
```
Help me make a build vs buy decision for [CAPABILITY].
Stack context: Trino · Kestra · DBT · Kubernetes

Score each option against:
- Fit with existing stack (integration complexity)
- Operational burden (who runs it, at what cost)
- Team familiarity (ramp-up time)
- Scalability (headroom for 3–10x growth)
- Vendor risk (lock-in, pricing trajectory, community health)
- Total cost (build cost + 2-year run cost)
- Time to value

Options to evaluate:
- Build: [DESCRIBE WHAT WE'D BUILD]
- Buy/OSS option A: [NAME]
- Buy/OSS option B: [NAME]

Constraints: [DEADLINE, BUDGET, TEAM SIZE]
```

### Evaluate a specific tool
```
Evaluate [TOOL NAME] for [USE CASE].
We currently use: Trino · Kestra · DBT · Kubernetes

Assess:
- What problem it solves and how well
- Integration complexity with our stack
- Operational overhead (deployment, upgrades, observability)
- Community / vendor health (activity, support, roadmap)
- Licensing and cost model
- Known failure modes and limitations
- Alternatives worth considering

Be direct. If it's the wrong tool for this use case, say so.
```

### Compare two specific tools
```
Compare [TOOL A] vs [TOOL B] for [USE CASE].
Stack context: Trino · Kestra · DBT · Kubernetes

Decision criteria (ranked by priority):
1. [CRITERION 1 — e.g. operational simplicity]
2. [CRITERION 2 — e.g. query performance at our scale]
3. [CRITERION 3 — e.g. cost]

Give a clear recommendation with rationale. Note any deal-breakers.
Scale: [DATA VOLUME, TEAM SIZE, QUERY RATE]
```

### Run a proof-of-concept design
```
Design a time-boxed PoC to evaluate [TOOL] for [USE CASE].
Time budget: [N days]

Output:
- PoC scope (what to test, what to skip)
- Success/failure criteria with measurable thresholds
- Test scenarios to run
- Risks to validate
- Decision checklist at the end of the PoC
```

### Vendor assessment
```
Help me assess this vendor/product for procurement.
Product: [NAME]
Use case: [DESCRIBE]

Evaluate:
- Functional fit (does it do what we need?)
- Technical fit (integrates with our stack?)
- Vendor health (funding, customers, support model)
- Pricing model and total cost of ownership
- Data residency and security posture
- Contract risks (lock-in, auto-renewal, exit clauses)
- Reference checks — what to ask existing customers

[PASTE VENDOR MATERIALS / DEMO NOTES IF AVAILABLE]
```

### Tech radar update
```
Help me update our tech radar for [CATEGORY: Languages / Frameworks / Platforms / Tools].
Current radar entries: [LIST EXISTING ENTRIES AND RINGS — Adopt/Trial/Assess/Hold]

New technology to place: [NAME]
Context: [WHY IT'S ON OUR RADAR NOW]

Recommend a ring placement with rationale. Note what would move it to the next ring.
```

### Deprecation plan
```
Create a deprecation plan for [TOOL/SYSTEM].
Replacement: [WHAT REPLACES IT]
Users/teams affected: [LIST]

Output:
- Deprecation timeline with milestones
- Migration path for each user group
- Communication plan
- Risk of moving too fast / too slow
- Definition of "done" (when is the old system fully decommissioned?)
```

## Evaluation scoring template

Use this when you need a structured score to present to stakeholders.

| Criterion | Weight | Option A | Option B | Build |
|-----------|--------|----------|----------|-------|
| Stack fit | 20% | /10 | /10 | /10 |
| Operational burden | 20% | /10 | /10 | /10 |
| Team familiarity | 15% | /10 | /10 | /10 |
| Scalability | 15% | /10 | /10 | /10 |
| Vendor/project risk | 15% | /10 | /10 | /10 |
| Total cost (2yr) | 15% | /10 | /10 | /10 |
| **Weighted total** | 100% | | | |

## Build vs buy decision heuristics

| Signal | Lean build | Lean buy |
|--------|-----------|---------|
| Core differentiator? | Yes → build | No → buy |
| Team has deep expertise? | Yes → build | No → buy |
| Market has mature solutions? | No → build | Yes → buy |
| Integration complexity? | Simple → either | Complex → buy carefully |
| Operational burden | Can absorb → build | Can't → buy managed |
| Time to value | Have runway → build | Need fast → buy |
| Data/IP sensitivity | High → build or self-host | Low → SaaS OK |

## Vendor health red flags
- Pricing opaque or consumption-based with no cap
- No published SLA or SLA < 99.9% for production use cases
- Vendor lock-in via proprietary APIs with no export path
- Thin community (< 1k GitHub stars, low recent commit activity for OSS)
- No enterprise support tier or support via community only
- Recent funding issues, layoffs, or acquisition rumours
- Auto-renewing annual contracts with 90-day notice to cancel
