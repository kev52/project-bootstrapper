## Service-Oriented Architecture

- **Services own their data.** No shared databases between service boundaries.
- **Communication between bounded contexts via DTOs or events.** Never pass domain entities across context boundaries.
- **Anti-corruption layers at integration points.** External API responses get mapped to domain objects at the boundary, not passed through.
- **Each service is independently deployable.** No compile-time dependencies between services.
- **Failures are expected.** Circuit breakers, retries with backoff, graceful degradation. No service assumes another is always available.
