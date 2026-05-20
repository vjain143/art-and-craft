# Skill: DBT Model Health

**Domain:** DBT  
**Updated:** 2026-05-20

## When to use
Model failure diagnosis, test coverage audits, materialisation strategy, schema.yml reviews.

## Prompt templates

### Diagnose model failure
```
Diagnose this DBT model failure and suggest a fix.
DBT version: [VERSION]
Adapter: [trino/bigquery/snowflake]

[PASTE dbt run ERROR OUTPUT]
```

### Audit model and tests
```
Review this DBT model and schema.yml.
Flag: missing tests, undocumented columns, wrong materialisation, ref() vs source() misuse.
Suggest a complete schema.yml for all columns.

Model SQL:
[PASTE SQL]

schema.yml:
[PASTE SCHEMA.YML OR "none"]
```

### Advise materialisation strategy
```
Suggest the right materialisation (table/view/incremental/snapshot/ephemeral)
for this DBT model given its usage pattern and data volume.

Model SQL:
[PASTE SQL]

Context:
- Row count: [ESTIMATE]
- Query frequency: [N times/day]
- Data arrives: [append-only / full refresh / SCD]
```

### Incremental model debug
```
This DBT incremental model is not picking up new rows correctly.
Diagnose and fix the incremental logic.

[PASTE MODEL SQL WITH is_incremental() block]
[PASTE dbt run --full-refresh vs incremental output diff]
```

## Quick reference

### Minimum viable schema.yml
```yaml
models:
  - name: my_model
    description: "What this model does and who uses it"
    columns:
      - name: id
        description: "Primary key"
        tests: [unique, not_null]
      - name: status
        tests:
          - accepted_values:
              values: ['active', 'inactive', 'pending']
      - name: created_at
        tests: [not_null]
```

### Materialisation decision guide
| Pattern | Materialisation |
|---------|----------------|
| Small lookup / reference table | `view` |
| Large aggregation queried often | `table` |
| Append-only event data | `incremental` (unique_key on id) |
| Slowly changing dimensions | `snapshot` |
| Intermediate transformation | `ephemeral` |

### Useful dbt commands
```bash
dbt run --select my_model
dbt test --select my_model
dbt run --full-refresh --select my_model
dbt docs generate && dbt docs serve
dbt source freshness
dbt compile --select my_model   # debug compiled SQL
```
