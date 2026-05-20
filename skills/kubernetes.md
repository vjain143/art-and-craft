# Skill: Kubernetes Health & Operations

**Domain:** Kubernetes  
**Updated:** 2026-05-20

## When to use
Pod failure diagnosis, resource right-sizing, manifest review, HPA tuning, multi-env health checks.

## Prompt templates

### Diagnose pod failure
```
Diagnose this Kubernetes pod failure and give me exact remediation commands.
Environment: [dev/staging/prod]
Namespace: [NAMESPACE]

[PASTE kubectl describe pod / kubectl logs OUTPUT]
```

### Right-size resources
```
Review this Kubernetes resource config and suggest right-sizing.
Current usage metrics:
[PASTE kubectl top output or HPA stats]

Config:
[PASTE deployment YAML]
```

### Generate health check script
```
Write a shell script that checks health of all pods across dev/staging/prod namespaces
using contexts: kubectl --context dev|staging|prod.
Alert on: CrashLoopBackOff, OOMKilled, Pending > 5 min, ImagePullBackOff.
Output a colour-coded summary table.
```

### Review RBAC / security context
```
Review this Kubernetes RBAC and security context config.
Flag: over-permissive roles, missing securityContext, privileged containers,
hostNetwork/hostPID usage, missing resource quotas.

[PASTE CONFIG]
```

## Quick reference

```bash
# All non-running pods across namespaces
kubectl get pods -A --field-selector=status.phase!=Running

# Describe a failing pod
kubectl describe pod <pod-name> -n <namespace>

# Recent logs (last 100 lines)
kubectl logs <pod-name> -n <namespace> --tail=100

# Events sorted by time
kubectl get events -n <namespace> --sort-by='.lastTimestamp'

# Resource usage
kubectl top pods -n <namespace>

# Rollout history
kubectl rollout history deployment/<name> -n <namespace>

# Quick rollback
kubectl rollout undo deployment/<name> -n <namespace>
```

### Common failure patterns
| Status | Likely cause | First action |
|--------|-------------|--------------|
| `CrashLoopBackOff` | App error / bad config | `kubectl logs` |
| `OOMKilled` | Memory limit too low | Raise `resources.limits.memory` |
| `Pending` | Insufficient node resources | `kubectl describe pod` → events |
| `ImagePullBackOff` | Bad image tag / registry auth | Check image name + pull secret |
| `CreateContainerConfigError` | Missing ConfigMap / Secret | Verify referenced resources exist |
| `Terminating` stuck | Finalizer not cleared | Check finalizers, force delete |
