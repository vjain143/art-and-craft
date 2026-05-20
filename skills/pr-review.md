# Skill: PR Review

**Domain:** All  
**Updated:** 2026-05-20

## When to use
Reviewing any pull request — Trino SQL, DBT models, Kestra YAML, Kubernetes manifests, Python.

## Prompt templates

### Trino SQL PR
```
Review this Trino SQL PR. Give inline comments with severity labels (critical/warning/suggestion).
Include: partition pruning, join strategy, data type mismatches, missing CTEs, and performance traps.

[PASTE DIFF]
```

### DBT model PR
```
Review this DBT model PR. Check: naming conventions, ref() vs source() usage,
missing tests, missing documentation, and materialisation choice.
Severity labels: critical/warning/suggestion.

[PASTE DIFF]
```

### Kestra workflow YAML PR
```
Review this Kestra workflow YAML PR. Check: task ordering, error handlers,
retry config, timeout values, secret management, and idempotency.
Severity labels: critical/warning/suggestion.

[PASTE YAML]
```

### Kubernetes manifest PR
```
Review this Kubernetes manifest PR. Check: resource limits/requests,
liveness and readiness probes, RBAC least-privilege, security context,
image tag pinning, and HPA config.
Severity labels: critical/warning/suggestion.

[PASTE MANIFEST]
```

### Architecture / design PR
```
Review this design/architecture PR as a Principal Architect.
Check: alignment with data contracts, idempotency, observability hooks,
scalability assumptions, and runbook coverage.
Severity labels: critical/warning/suggestion.

[PASTE DIFF OR DESCRIPTION]
```

## Output format Claude returns
- Inline comments per block with severity label
- Summary section grouped by severity
- Before/after fix snippets
- Copy-paste ready GitHub comment block
