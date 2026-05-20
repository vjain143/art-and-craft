# Skill: Multi-Environment Health Monitoring

**Domain:** Ops  
**Updated:** 2026-05-20

## When to use
Generating health check scripts, dashboards, or unified status views across all platform services.

## Prompt templates

### Full platform health script (Python)
```
Generate a Python script that checks health of all platform services
across dev, staging, and prod environments.

Services to check:
- Trino: REST API /v1/cluster (worker count, active queries, failed queries)
- Kestra: API /api/v1/executions (recent failures)
- Kubernetes: kubectl --context [dev|staging|prod] pod statuses
- DBT: manifest.json last modified age check

Output: colour-coded terminal status table (rich library preferred).
Alert threshold: any service down or DBT manifest > 24 hours old.

Endpoints:
- Trino dev: [HOST:PORT]
- Kestra dev: [HOST:PORT]
[ADD STAGING/PROD AS NEEDED]
```

### React health dashboard
```
Build a React health dashboard artifact.
Show green/amber/red status for Trino, Kestra, DBT, and Kubernetes
across dev/staging/prod environments.
Include: last check time, error message on hover, manual refresh button.
Use inline styles only (no external CSS framework).
```

### Slack health digest (cron)
```
Write a Kestra workflow that runs every morning at 07:00 UTC.
It checks all platform services and posts a health digest to Slack.
Format: one emoji per service (green_circle/yellow_circle/red_circle) + status.
Secret: SLACK_WEBHOOK
```

### Alerting rules audit
```
Review these Prometheus/Alertmanager alerting rules for our platform stack.
Check: missing alerts, alert fatigue (too many non-actionable alerts),
missing runbook links, and incorrect severity labels.

[PASTE ALERT RULES YAML]
```

## Environment matrix

| Service | Dev | Staging | Prod |
|---------|-----|---------|------|
| Trino | `trino-dev:8080` | `trino-staging:8080` | `trino-prod:8080` |
| Kestra | `kestra-dev:8080` | `kestra-staging:8080` | `kestra-prod:8080` |
| Kubernetes | `kubectl --context dev` | `kubectl --context staging` | `kubectl --context prod` |
| DBT | manifest.json age | manifest.json age | manifest.json age |

## Health check endpoints

```bash
# Trino
curl http://<host>:8080/v1/cluster

# Kestra recent failures
curl "http://<host>:8080/api/v1/executions?state=FAILED&pageSize=5"

# K8s pod health across namespaces
kubectl get pods -A --field-selector=status.phase!=Running --context <env>
```
