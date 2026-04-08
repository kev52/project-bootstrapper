## Observability

- **Structured logging only.** JSON format with consistent fields: `timestamp`, `level`, `message`, `service`, `correlation_id`. No `print()` or unstructured `console.log()`.
- **Log levels mean something.** `ERROR`: something broke that needs attention. `WARN`: something unexpected but handled. `INFO`: key business events (user created, order placed). `DEBUG`: developer detail, off in production.
- **Correlation IDs on every request.** Generate at the edge, propagate through all service calls. Every log line includes it.
- **No PII in logs.** Never log passwords, tokens, full credit card numbers, SSNs, or email addresses. Mask or redact at the logging layer.
- **Log at boundaries.** Log when entering/exiting services, on external API calls, on retries, and on errors. Don't log inside tight loops.
- **Errors include context.** Every error log includes: what failed, what input caused it, what the expected behavior was. Stack traces for unexpected errors only.
- **Health checks are mandatory.** Every service exposes `/health` (or equivalent) returning `{ "status": "ok", "version": "..." }`. Used by load balancers and monitoring.
