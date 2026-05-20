# Skill: Architecture Review & Design Patterns

**Domain:** Architecture  
**Updated:** 2026-05-20

## When to use
Reviewing system designs, proposing or evaluating architecture patterns, producing design feedback,
validating scalability/reliability assumptions, and selecting the right pattern for a given problem.

## Prompt templates

### Review a system design
```
Review this system design as a Principal Architect.
Stack context: Trino · Kestra · DBT · Kubernetes · SonarQube

Check:
- Data contracts and schema ownership
- Idempotency and re-runnability
- Observability (alerting, freshness checks, lineage)
- Scalability assumptions and bottlenecks
- Failure modes and recovery paths
- Security boundaries and secret management
- Runbook coverage

Severity labels: critical / warning / suggestion

Design:
[PASTE DIAGRAM DESCRIPTION / ARCHITECTURE DOC / RFC EXCERPT]
```

### Evaluate a proposed pattern
```
Evaluate this architectural pattern for our use case.
Be direct about trade-offs and failure modes.

Pattern: [e.g. Lambda architecture / Event sourcing / CQRS / Medallion / Saga]
Use case: [DESCRIBE]
Scale: [DATA VOLUME, QUERY RATE, TEAM SIZE]
Constraints: [DEADLINES, EXISTING STACK, BUDGET]
```

### Compare two approaches
```
Compare these two architectural approaches for [PROBLEM].
Give a clear recommendation with rationale.
Frame trade-offs as: simplicity, operational burden, scalability, cost, and reversibility.

Option A: [DESCRIBE]
Option B: [DESCRIBE]

Context: [TEAM SIZE, SCALE, TIMELINE]
```

### Design a new component
```
Design a [COMPONENT] for our data platform.
Requirements:
- [FUNCTIONAL REQ 1]
- [FUNCTIONAL REQ 2]
- SLA: [LATENCY / FRESHNESS / AVAILABILITY]

Constraints:
- Stack: Trino · Kestra · DBT · Kubernetes
- Team: [SIZE]
- Must integrate with: [EXISTING SYSTEMS]

Output: component design with interface definitions, failure modes, and open questions.
```

### Identify design smells
```
Review this design/code for architectural anti-patterns.
Flag: tight coupling, missing abstraction layers, hidden data contracts,
monolithic flows, missing observability, and scalability ceilings.

[PASTE DESIGN DESCRIPTION / CODE / YAML / DIAGRAM]
```

### Scalability review
```
Review this architecture for scalability limits.
Identify: bottlenecks, single points of failure, fan-out problems,
hotspot partitions, and state management issues.
Suggest specific changes with effort estimates.

Current scale: [ROWS/DAY, QUERIES/MIN, PIPELINE COUNT]
Target scale: [3x / 10x / 100x in [TIMEFRAME]]

Architecture:
[PASTE]
```

### Technology selection
```
Help me select the right technology for [PROBLEM].
Evaluate against: operational simplicity, fit with existing stack (Trino/Kestra/DBT/K8s),
team familiarity, scalability, and long-term maintenance cost.

Candidates: [LIST OPTIONS OR ASK CLAUDE TO GENERATE THEM]
Constraints: [BUDGET, TIMELINE, TEAM SIZE]
```

## Design patterns quick reference

### Data pipeline patterns

| Pattern | When to use | Watch out for |
|---------|-------------|---------------|
| **Medallion (Bronze/Silver/Gold)** | General-purpose lakehouse | Over-engineering for simple pipelines |
| **Lambda (batch + stream)** | Low-latency + historical accuracy | Two code paths to maintain |
| **Kappa (stream only)** | Unified real-time + historical | Replay cost at scale |
| **Event sourcing** | Audit trail, time-travel queries | Storage growth, replay time |
| **CDC (Change Data Capture)** | Sync source systems to lake | Ordering guarantees, schema evolution |

### Orchestration patterns

| Pattern | When to use | Watch out for |
|---------|-------------|---------------|
| **DAG-based (Kestra/Airflow)** | Batch pipelines with dependencies | Fan-out explosion |
| **Saga (compensating transactions)** | Distributed multi-step workflows | Complex rollback logic |
| **Dead-letter queue** | Handling poison messages | Unbounded queue growth |
| **Circuit breaker** | Calling unreliable downstream APIs | False positives blocking recovery |
| **Idempotent consumer** | Safe re-processing of events | Key design complexity |

### Query & compute patterns

| Pattern | When to use | Watch out for |
|---------|-------------|---------------|
| **Pushdown predicate** | Reduce data scanned in Trino | Connector support varies |
| **Pre-aggregation (DBT table)** | Frequently queried aggregates | Staleness, storage cost |
| **Incremental materialisation** | Append-only large models | Late-arriving data |
| **CQRS** | Separate read/write models | Eventual consistency lag |
| **Broadcast join** | Small-large table joins in Trino | Memory pressure on coordinator |

### Reliability patterns

| Pattern | Use case |
|---------|---------|
| **Bulkhead** | Isolate critical from non-critical pipelines |
| **Retry with backoff** | Transient failures in Kestra tasks |
| **Timeout + fallback** | External API calls in pipelines |
| **Health endpoint** | Every service exposes `/health` or Trino `/v1/cluster` |
| **Canary deployment** | Gradual rollout of pipeline changes |

## Architecture review checklist

### Data contract
- [ ] Schema defined and versioned before implementation
- [ ] SLA (freshness, latency, availability) agreed with consumers
- [ ] Breaking-change process documented

### Reliability
- [ ] All failure modes identified with recovery paths
- [ ] No single point of failure for P1 pipelines
- [ ] Retry and timeout on every external call
- [ ] Rollback procedure exists

### Observability
- [ ] Alert on pipeline failure (Kestra errors block)
- [ ] Freshness check on all production models
- [ ] Lineage documented (dbt DAG or equivalent)
- [ ] Runbook exists for every alert

### Security
- [ ] Secrets via Kubernetes secrets / Kestra secret store — never hardcoded
- [ ] Least-privilege RBAC on all K8s service accounts
- [ ] No PII in logs

### Operability
- [ ] Runbook written before deploy
- [ ] Deployment is reversible (rollback < 15 min)
- [ ] On-call can diagnose without author present
