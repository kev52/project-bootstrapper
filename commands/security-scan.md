---
description: Scan code for security vulnerabilities and hardcoded secrets
allowed-tools: Read, Glob, Grep, Bash(git diff:*, git log:*)
---

Scan the codebase for common security issues. Checks against the security atom rules.

## What It Checks

1. **Hardcoded secrets** — API keys, tokens, passwords, connection strings in source files. Check for patterns: `password=`, `api_key=`, `secret`, `token`, `Bearer`, base64-encoded strings that look like keys.
2. **SQL/NoSQL injection** — String interpolation in queries. Look for template literals, f-strings, or concatenation in anything that touches a database.
3. **Input validation gaps** — Endpoints/handlers that use request body or query params without schema validation (no Zod, Pydantic, or equivalent).
4. **Missing auth declarations** — Routes/endpoints without explicit auth middleware or decorator.
5. **Unescaped output** — User input rendered in HTML without sanitization (XSS vectors).
6. **Overprivileged configs** — Database users with admin roles, wildcard CORS, `*` in permissions.
7. **Dependency issues** — Check lock file exists. If npm: `npm audit --json`. If pip: check for known CVE packages.
8. **Fail-open patterns** — Catch blocks that fall through to "allow" on auth/permission checks.

## Output

```
SECURITY SCAN REPORT
════════════════════

🔴 CRITICAL ({count})
  {file}:{line} — {issue}
  Risk: {what could happen}
  Fix: {how to fix}

🟡 WARNING ({count})
  {file}:{line} — {issue}
  Fix: {how to fix}

✅ CLEAN AREAS
  {what looks good}

SUMMARY: {critical} critical, {warning} warnings across {files} files scanned
```

Offer to auto-fix issues where safe (e.g., replacing string interpolation with parameterized queries).
