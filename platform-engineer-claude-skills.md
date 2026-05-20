# Platform Engineer — Claude Skills Reference

> A personal reference guide for using Claude as a daily productivity tool across PR reviews, environment health checks, incident response, and code quality workflows.

**Stack:** Trino · Kestra · DBT · Kubernetes · SonarQube

---

## Table of Contents

1. [PR Review & Code Comments](#1-pr-review--code-comments)
2. [SonarQube Code Quality](#2-sonarqube-code-quality)
3. [Kubernetes Health Checks](#3-kubernetes-health-checks)
4. [Trino Cluster Health](#4-trino-cluster-health)
5. [Kestra Workflow Ops](#5-kestra-workflow-ops)
6. [DBT Model Health](#6-dbt-model-health)
7. [Incident Response](#7-incident-response)
8. [Multi-Environment Health Monitoring](#8-multi-environment-health-monitoring)
9. [Claude Skills Catalogue](#9-claude-skills-catalogue)

---

## 1. PR Review & Code Comments

Paste a diff or file into Claude to get inline comments with severity labels (`critical` / `warning` / `suggestion`) and a summary block ready to post to GitHub or GitLab.

### Prompt Templates

**Trino SQL PR**
```
Review this Trino SQL PR and give inline comments with severity labels (critical/warning/suggestion):

[PASTE DIFF HERE]
```

**DBT Model PR**
```
Review this DBT model PR — check for naming conventions, ref() usage, missing tests, and documentation:

[PASTE DIFF HERE]
```

**Kestra Workflow YAML PR**
```
Review this Kestra workflow YAML PR — check task ordering, error handling, retry config, and secret usage:

[PASTE YAML HERE]
```

**Kubernetes Manifest PR**
```
Review this Kubernetes manifest PR — check resource limits, liveness/readiness probes, RBAC, and security context:

[PASTE MANIFEST HERE]
```

### What Claude returns

- Inline comments per block/line with severity label
- A summary section listing all issues by severity
- Suggested fixes inline (before/after snippets)
- A copy-paste ready GitHub comment block

---

## 2. SonarQube Code Quality

Paste SonarQube issue output, a rule ID, or a code snippet to get a plain-English explanation, root cause, and a concrete fix.

### Prompt Templates

**Explain and fix a Sonar issue**
```
Explain this SonarQube issue and provide a fix:

[PASTE SONAR ISSUE / RULE ID / CODE SNIPPET HERE]
```

**Prioritise a list of code smells**
```
I have these SonarQube code smells in my DBT Python macros. Prioritise them and give me a fix plan:

[PASTE ISSUES LIST]
```

**Sonar-compliant refactor**
```
Generate a SonarQube-compliant refactor for this function — fix cyclomatic complexity, remove duplication, and add null checks:

[PASTE CODE]
```

### Common Sonar rules for this stack

| Rule | Applies to | Description |
|------|-----------|-------------|
| `python:S3776` | DBT macros / Kestra plugins | Cognitive complexity too high |
| `python:S1192` | Any Python | String literals duplicated |
| `sql:S1192` | Trino SQL | Duplicated string literals |
| `python:S2077` | Query builders | SQL injection risk |
| `yaml:S1135` | Kestra / K8s YAML | TODO comments in config |

---

## 3. Kubernetes Health Checks

Paste `kubectl` output or error logs to get diagnosis, root cause, and exact remediation commands.

### Prompt Templates

**Diagnose a pod failure**
```
Diagnose this Kubernetes pod failure and give me remediation commands:

[PASTE kubectl describe pod / logs OUTPUT]
```

**Generate a health-check script**
```
Write a shell script that checks health of all pods across dev/staging/prod namespaces
and alerts on CrashLoopBackOff, OOMKilled, or Pending states
```

**Right-size Trino resources**
```
Review this HPA and resource limits config and suggest right-sizing for a Trino coordinator pod:

[PASTE CONFIG]
```

### Quick kubectl health commands

```bash
# All pod statuses across namespaces
kubectl get pods -A --field-selector=status.phase!=Running

# Describe a failing pod
kubectl describe pod <pod-name> -n <namespace>

# Recent logs
kubectl logs <pod-name> -n <namespace> --tail=100

# Events sorted by time
kubectl get events -n <namespace> --sort-by='.lastTimestamp'

# Resource usage
kubectl top pods -n <namespace>
```

### Common failure patterns

| Status | Likely cause | First action |
|--------|-------------|--------------|
| `CrashLoopBackOff` | App error / bad config | Check `kubectl logs` |
| `OOMKilled` | Memory limit too low | Increase `resources.limits.memory` |
| `Pending` | Insufficient node resources | Check `kubectl describe pod` events |
| `ImagePullBackOff` | Bad image tag / registry auth | Check image name and pull secret |
| `CreateContainerConfigError` | Missing ConfigMap / Secret | Verify referenced resources exist |

---

## 4. Trino Cluster Health

Paste Trino query plans, error logs, or cluster stats to get query optimisation advice, worker health diagnosis, and config tuning.

### Prompt Templates

**Optimise a query plan**
```
Analyse this Trino EXPLAIN output and suggest optimisations (partition pruning, join reordering, broadcast hints):

[PASTE EXPLAIN OUTPUT]
```

**Diagnose OOM or worker failure**
```
Diagnose this Trino worker failure / OOM error and give config fixes (memory pools, spill-to-disk, session properties):

[PASTE ERROR LOG]
```

**Generate a health monitor script**
```
Write a Trino cluster health monitoring script using the Trino REST API
that checks worker count, query queue depth, and failed queries
```

### Trino REST API health endpoints

```bash
# Cluster info
curl http://<trino-host>:8080/v1/cluster

# Active queries
curl http://<trino-host>:8080/v1/query?state=RUNNING

# Worker nodes
curl http://<trino-host>:8080/v1/node

# Failed queries (last N)
curl "http://<trino-host>:8080/v1/query?state=FAILED&limit=20"
```

### Key session properties for performance

```sql
-- Memory
SET SESSION query_max_memory = '8GB';
SET SESSION query_max_memory_per_node = '4GB';

-- Spill to disk
SET SESSION spill_enabled = true;
SET SESSION spill_order_by = true;

-- Join strategy
SET SESSION join_distribution_type = 'AUTOMATIC';
SET SESSION prefer_partial_aggregation = true;
```

---

## 5. Kestra Workflow Ops

Paste failed Kestra execution logs or workflow YAML to get failure diagnosis, corrected YAML, and retry/alerting best practices.

### Prompt Templates

**Diagnose an execution failure**
```
Diagnose this Kestra execution failure and provide a corrected workflow YAML:

[PASTE EXECUTION LOG / YAML]
```

**DBT + Kestra + alerting flow**
```
Write a Kestra workflow that runs a DBT build, checks for failures,
and sends a Slack alert if any model fails
```

**Audit a workflow YAML**
```
Review this Kestra YAML for best practices: error handling, retries, timeouts,
secret management, and idempotency:

[PASTE YAML]
```

### Kestra YAML best practices checklist

```yaml
# Recommended structure for production workflows
id: my-pipeline
namespace: prod.data

labels:
  team: platform
  env: prod

tasks:
  - id: run-dbt
    type: io.kestra.plugin.dbt.cli.DbtCLI
    runner: PROCESS
    commands:
      - dbt run --select my_model
    # Always set timeouts
    timeout: PT30M

errors:
  # Always define error handlers
  - id: alert-on-failure
    type: io.kestra.plugin.notifications.slack.SlackIncomingWebhook
    url: "{{ secret('SLACK_WEBHOOK') }}"
    payload: |
      {"text": "Pipeline failed: {{ execution.id }}"}

triggers:
  - id: schedule
    type: io.kestra.core.models.triggers.types.Schedule
    cron: "0 6 * * *"
```

---

## 6. DBT Model Health

Paste DBT run results, model SQL, or `schema.yml` to get failure diagnosis, missing test recommendations, and materialisation strategy advice.

### Prompt Templates

**Diagnose a model failure**
```
Diagnose this DBT model failure and suggest a fix:

[PASTE dbt run ERROR OUTPUT]
```

**Audit model and tests**
```
Review this DBT model and schema.yml for missing tests, documentation, and best practices:

[PASTE MODEL + SCHEMA.YML]
```

**Advise materialisation strategy**
```
Suggest the right materialisation strategy (table/view/incremental/snapshot)
for this DBT model given its usage pattern:

[PASTE MODEL SQL + CONTEXT]
```

### DBT model health checklist

```yaml
# schema.yml minimum viable coverage
models:
  - name: my_model
    description: "What this model does and who uses it"
    columns:
      - name: id
        description: "Primary key"
        tests:
          - unique
          - not_null
      - name: status
        tests:
          - accepted_values:
              values: ['active', 'inactive', 'pending']
      - name: created_at
        tests:
          - not_null
```

### Materialisation decision guide

| Pattern | Recommended materialisation |
|---------|----------------------------|
| Small lookup / reference table | `view` |
| Large aggregation queried often | `table` |
| Append-only event data | `incremental` (unique key on id) |
| Slowly changing dimensions | `snapshot` |
| Intermediate transformation | `ephemeral` |

---

## 7. Incident Response

Structured prompts for the full on-call loop: RCA, stakeholder comms, post-mortems, and runbooks.

### Prompt Templates

**Root cause analysis**
```
Walk me through a root cause analysis for this incident. Symptoms:

[DESCRIBE SYMPTOMS / PASTE LOGS]
```

**Stakeholder incident update**
```
Write an incident communication update for stakeholders.
Service affected: [NAME]
Impact: [DESCRIBE]
Current status: [STATUS]
```

**Post-mortem (5 whys)**
```
Write a post-mortem for this incident following the 5-whys format.
Timeline: [PASTE]
Impact: [DESCRIBE]
Fix applied: [DESCRIBE]
```

**Platform runbook**
```
Generate a runbook for checking and recovering all platform services:
Trino, Kestra, DBT, and Kubernetes across dev/staging/prod environments
```

### Incident severity levels

| Level | Definition | Response time |
|-------|-----------|---------------|
| P1 — Critical | Production data pipeline down, data loss risk | Immediate |
| P2 — High | Significant degradation, SLA at risk | < 30 min |
| P3 — Medium | Non-critical service degraded | < 2 hours |
| P4 — Low | Minor issue, workaround available | Next business day |

---

## 8. Multi-Environment Health Monitoring

Scripts and artifacts that poll all services across environments and surface a unified status view.

### Prompt Templates

**Full platform health script (Python)**
```
Generate a Python script that checks health of Trino (REST API), Kestra (API),
DBT (manifest.json freshness), and Kubernetes (kubectl) across dev/staging/prod
and outputs a colour-coded status table
```

**React health dashboard artifact**
```
Build a React artifact health dashboard that shows green/amber/red status
for Trino, Kestra, DBT, and Kubernetes environments — with a manual refresh button
```

### Environment matrix

| Service | Dev endpoint | Staging endpoint | Prod endpoint |
|---------|-------------|-----------------|---------------|
| Trino | `trino-dev:8080/v1/cluster` | `trino-staging:8080/v1/cluster` | `trino-prod:8080/v1/cluster` |
| Kestra | `kestra-dev:8080/api/v1/flows` | `kestra-staging:8080/api/v1/flows` | `kestra-prod:8080/api/v1/flows` |
| Kubernetes | `kubectl --context dev` | `kubectl --context staging` | `kubectl --context prod` |
| DBT | `manifest.json` age check | `manifest.json` age check | `manifest.json` age check |

---

## 9. Claude Skills Catalogue

Quick reference for which built-in Claude skill to invoke per task.

| Skill | When to use |
|-------|-------------|
| `docx` | ADRs, RFCs, design specs, post-mortems as Word docs |
| `pptx` | Architecture overviews, sprint reviews, roadmap decks |
| `xlsx` | Cost models, capacity planning, tech evaluation grids |
| `pdf` | Publish final specs, merge runbooks, create one-pagers |
| `frontend-design` | Internal dashboards, pipeline monitors, admin UIs |
| `mcp-builder` | Build MCP servers to connect Claude to internal APIs |
| `web-artifacts-builder` | AI-powered tools: SQL generators, runbook chatbots |
| `file-reading` | Parse uploaded YAML, JSON, CSV, PDF artefacts |
| `internal-comms` | Incident updates, leadership summaries, RFC announcements |
| `skill-creator` | Encode team standards and stack conventions as reusable skills |

---

## Usage Tips

- **Paste real output.** Claude performs best with actual logs, diffs, or configs — not descriptions of them.
- **Add context.** Mention environment (dev/staging/prod), team conventions, or constraints when relevant.
- **Iterate.** Ask Claude to adjust severity, tone, or format after the first response.
- **Chain prompts.** Use the incident RCA output as input to the post-mortem prompt.

---

*Maintained by: Platform Engineering*
*Stack: Trino · Kestra · DBT · Kubernetes · SonarQube*
