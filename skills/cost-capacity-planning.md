# Skill: Cost & Capacity Planning

**Domain:** Architecture / FinOps  
**Updated:** 2026-05-20

## When to use
Cloud cost analysis, right-sizing Trino/Kubernetes workloads, storage cost modelling,
capacity forecasting for growth, FinOps reviews, and budget estimation for new initiatives.

## Prompt templates

### Analyse and reduce cloud costs
```
Analyse these cloud costs and identify the top reduction opportunities.
Cloud: [AWS / GCP / Azure]
Services: [e.g. EC2, S3, GKE, Trino on EKS]
Monthly spend: [PASTE COST REPORT OR SUMMARISE]

Focus on: idle resources, over-provisioned instances, storage lifecycle gaps,
reserved instance / committed use discount opportunities, and egress costs.
Prioritise by savings potential vs effort.
```

### Right-size Trino cluster
```
Recommend the right Trino cluster sizing for this workload.
Current config:
- Coordinator: [INSTANCE TYPE, MEMORY]
- Workers: [COUNT × INSTANCE TYPE, MEMORY]
- Concurrent queries: [TYPICAL / PEAK]
- Largest query memory: [GB]
- Data scanned per query: [GB/TB]

Usage pattern: [batch-heavy / interactive / mixed]
Growth target: [3x / 10x in N months]

Output: recommended sizing, estimated cost at current and target scale,
and config parameters to set (memory pools, spill thresholds).
```

### Right-size Kubernetes workloads
```
Recommend right-sizing for these Kubernetes workloads.
Provide: recommended requests/limits, HPA min/max replicas, and estimated cost change.

Current config and observed usage:
[PASTE kubectl top output + current resource config]

Workloads to review: [LIST — e.g. kestra-worker, dbt-runner, trino-coordinator]
```

### Model storage costs
```
Model storage costs for this data platform over 12 months.
Cloud: [AWS / GCP / Azure]
Storage tier: [S3 / GCS / ADLS]

Current state:
- Raw / Bronze: [TB], growth: [GB/day]
- Processed / Silver: [TB], growth: [GB/day]
- Aggregated / Gold: [TB], growth: [GB/day]

Include: storage cost, request costs, cross-region replication if applicable,
lifecycle policy recommendations (transition to cold/archive tiers).
```

### Capacity forecast for growth
```
Forecast infrastructure capacity needs for [3x / 10x / N] growth over [TIMEFRAME].
Current baseline:
- Data volume: [TB/day ingested]
- Query load: [N queries/day, peak concurrency N]
- Pipeline count: [N Kestra workflows]
- Kubernetes nodes: [COUNT × INSTANCE]

Bottlenecks to size: Trino compute, storage I/O, Kubernetes node pools, Kestra workers.
Output: phased capacity plan with trigger thresholds for each expansion step.
```

### Estimate cost of a new initiative
```
Estimate the infrastructure cost for this new initiative.
Cloud: [AWS / GCP / Azure]

Initiative: [DESCRIBE — e.g. add real-time ingestion layer, new Trino cluster for team X]
Expected load:
- Data volume: [GB/day]
- Query rate: [N/hour]
- Pipeline frequency: [N/day]

Output: monthly cost estimate (low / mid / high), key cost drivers,
and cost optimisation options to consider at design time.
```

### FinOps review — find waste
```
Review our infrastructure config for FinOps waste.
Focus: unused resources, idle nodes, over-provisioned storage classes,
unattached volumes, orphaned snapshots, and missing auto-shutdown on non-prod.

[PASTE kubectl get nodes, cloud cost breakdown, or describe your setup]
```

### Build a cost model for a proposal
```
Build a cost model for this architectural proposal.
Format it for a leadership audience — business numbers, not raw pricing.

Proposal: [DESCRIBE]
Cloud: [AWS / GCP / Azure]
Scale assumptions: [DATA VOLUME, QUERY RATE, TEAM USAGE]

Output:
- Year 1 cost (build + run)
- Year 2 run cost
- Cost per unit of value (cost per query / per pipeline run / per GB processed)
- Break-even vs current approach (if replacing something)
```

## Quick reference

### Trino cost levers
| Lever | Action | Typical saving |
|-------|--------|---------------|
| Spot / preemptible workers | Use for workers, not coordinator | 60–80% on worker compute |
| Reserved instances (coordinator) | 1-year RI on coordinator | 30–40% |
| Query result caching | Enable result cache for repeated queries | Reduce worker hours |
| Partition pruning | Ensure queries filter on partition key | Reduce data scanned = less I/O cost |
| Autoscaling workers | Scale to 0 in off-hours | Eliminate idle worker cost |
| Storage format | Use Parquet/ORC + compression | 3–5× storage reduction vs raw |

### Kubernetes cost levers
| Lever | Action |
|-------|--------|
| Requests = actual usage | Don't over-request CPU/memory — wastes node capacity |
| HPA + Cluster Autoscaler | Scale workloads and nodes with demand |
| Spot node pools for batch | Kestra workers, DBT runners on spot |
| On-demand for stateful | Trino coordinator, databases on on-demand |
| Resource quotas per namespace | Prevent runaway costs from dev/test |
| Non-prod auto-shutdown | Schedule dev/staging clusters off overnight |

### Storage lifecycle rules (S3/GCS)
```
Raw / Bronze:   30 days Standard → 90 days Infrequent Access → 365 days Glacier
Silver:         90 days Standard → 180 days Infrequent Access
Gold / Serving: Standard (no transition — hot data)
Logs:           7 days Standard → delete
```

### Cost allocation tagging (minimum set)
```yaml
tags:
  team: platform
  env: dev | staging | prod
  service: trino | kestra | dbt | ingestion
  cost-centre: [CODE]
```

### Capacity trigger thresholds (suggested)
| Resource | Review trigger | Action |
|----------|---------------|--------|
| Trino worker CPU | > 70% sustained 15 min | Add workers |
| Trino coordinator heap | > 80% | Scale up or tune memory config |
| K8s node CPU | > 65% average | Add nodes |
| Storage growth | > 80% of budget | Review lifecycle / compression |
| Kestra worker queue | > 50 queued executions | Add workers |
