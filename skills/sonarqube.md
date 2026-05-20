# Skill: SonarQube Code Quality

**Domain:** SonarQube  
**Updated:** 2026-05-20

## When to use
Explaining SonarQube issues, prioritising code smell backlogs, generating compliant refactors.

## Prompt templates

### Explain and fix a Sonar issue
```
Explain this SonarQube issue in plain English and provide a concrete fix.
Language: [Python/SQL/YAML/Java]
Rule ID: [e.g. python:S3776]

[PASTE SONAR ISSUE / CODE SNIPPET]
```

### Prioritise a backlog of issues
```
I have these SonarQube issues in [DBT macros / Kestra plugins / Trino SQL].
Prioritise them by business risk and give me a fix plan with effort estimates.

[PASTE ISSUES LIST]
```

### Generate a compliant refactor
```
Refactor this function to be SonarQube-compliant.
Fix: cognitive complexity, string literal duplication, null safety, and dead code.
Keep the same public interface.

[PASTE CODE]
```

### SQL injection audit
```
Audit this Python/SQL code for SQL injection risk (SonarQube rule S2077).
Show exactly which lines are vulnerable and provide parameterised query fixes.

[PASTE CODE]
```

## Quick reference

### Common rules for this stack
| Rule | Language | Description |
|------|----------|-------------|
| `python:S3776` | Python | Cognitive complexity > 15 |
| `python:S1192` | Python | String literals duplicated ≥ 3 times |
| `sql:S1192` | SQL | Duplicated string literals |
| `python:S2077` | Python | SQL injection risk |
| `yaml:S1135` | YAML | TODO/FIXME comments in config |
| `python:S107` | Python | Too many function parameters |
| `python:S1481` | Python | Unused local variables |
| `python:S3457` | Python | Printf-style format strings |

### Quality gate thresholds (team defaults)
| Metric | Gate |
|--------|------|
| Coverage | ≥ 80% on new code |
| Duplicated lines | < 3% |
| Cognitive complexity | < 15 per function |
| Critical issues | 0 |
| Blocker issues | 0 |
