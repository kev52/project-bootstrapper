## Clean Architecture

### Layer Rules (Dependency Points Inward Only)

```
Domain (entities, value objects, domain services, repository interfaces)
  ↑
Application (use cases, DTOs, port interfaces)
  ↑
Infrastructure (repo implementations, external APIs, persistence)
  ↑
Presentation (UI, controllers, API handlers)
```

- **Use cases orchestrate. They do not contain business rules.** Business rules live in domain entities and domain services.
- **Infrastructure depends on domain via interfaces, never the reverse.**
- **Presentation calls use cases only. Never touches domain directly.**
- **Use cases have exactly one public method** (execute/call/handle).

### Feature Structure

```
feature_name/
├── domain/           # Entities, value objects, repo interfaces, domain services, events
├── application/      # Use cases, DTOs, port interfaces
├── infrastructure/   # Repo implementations, external API clients, persistence
└── presentation/     # Controllers, API handlers, mappers
```

### Verification

- No domain import in infrastructure or presentation
- Every new service has an interface + at least one implementation
- No business logic in controllers or infrastructure
