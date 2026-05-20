# Skill: Kestra Workflow Operations

**Domain:** Kestra  
**Updated:** 2026-05-20

## When to use
Execution failure diagnosis, workflow YAML review, designing new pipelines, alerting/retry patterns.

## Prompt templates

### Diagnose execution failure
```
Diagnose this Kestra execution failure and provide a corrected workflow YAML.
Kestra version: [VERSION]
Namespace: [NAMESPACE]

[PASTE EXECUTION LOG / ERROR OUTPUT]

Current YAML:
[PASTE YAML]
```

### Audit workflow YAML
```
Review this Kestra YAML for production readiness.
Check: error handlers, retry config, timeouts, secret management,
task idempotency, input validation, and trigger schedule.

[PASTE YAML]
```

### Design a new pipeline
```
Design a Kestra workflow that:
- [DESCRIBE STEPS]
- Runs on schedule: [CRON]
- Sends Slack alert on failure
- Is fully idempotent

Output namespace: [NAMESPACE]
Stack context: DBT + Trino + Kubernetes
```

### DBT + Kestra + alerting flow
```
Write a Kestra workflow that:
1. Runs dbt build for [MODEL/TAG]
2. Checks dbt run results for failures
3. Sends a Slack alert with model name and error if any model fails
4. Marks the execution as FAILED if dbt fails

Use Slack secret name: SLACK_WEBHOOK
```

## Quick reference

### Production YAML skeleton
```yaml
id: my-pipeline
namespace: prod.data

labels:
  team: platform
  env: prod

inputs:
  - id: target_date
    type: STRING
    defaults: "{{ now() | dateAdd(-1, 'DAYS') | date('yyyy-MM-dd') }}"

tasks:
  - id: run-dbt
    type: io.kestra.plugin.dbt.cli.DbtCLI
    runner: PROCESS
    commands:
      - dbt run --select my_model --vars '{"target_date": "{{ inputs.target_date }}"}'
    timeout: PT30M

errors:
  - id: alert-on-failure
    type: io.kestra.plugin.notifications.slack.SlackIncomingWebhook
    url: "{{ secret('SLACK_WEBHOOK') }}"
    payload: |
      {"text": "Pipeline *{{ flow.id }}* failed on {{ execution.startDate }}\nExecution: {{ execution.id }}"}

triggers:
  - id: daily-schedule
    type: io.kestra.core.models.triggers.types.Schedule
    cron: "0 6 * * *"
```

### Kestra API quick checks
```bash
# List recent executions
curl http://<kestra-host>:8080/api/v1/executions?namespace=prod.data&pageSize=20

# Execution detail
curl http://<kestra-host>:8080/api/v1/executions/<execution-id>

# Trigger a flow manually
curl -X POST http://<kestra-host>:8080/api/v1/executions/prod.data/my-pipeline
```

### Best practices checklist
- [ ] `errors:` block defined with Slack alert
- [ ] All tasks have `timeout:` set
- [ ] Secrets via `secret()`, never hardcoded
- [ ] Idempotent: safe to re-run without duplicating data
- [ ] `inputs:` with defaults for date parameters
- [ ] `labels:` include `team` and `env`
