## Domain-Driven Design

- **Entities have behavior, not just data.** If a class has only getters/setters, it's anemic. Add the business method.
- **Value Objects for domain concepts.** Email, Money, DateRange, PhoneNumber — wrap them. No raw primitives.
- **Aggregates enforce invariants.** Validate inside the aggregate, not in the use case or controller.
- **Domain Events over direct coupling.** When Aggregate A triggers something in Aggregate B, emit an event. Don't call B from A.
- **Ubiquitous Language is mandatory.** Name classes/methods using domain language. No generic CRUD names (DataManager, ServiceHelper, BaseProcessor).
- **Domain layer has ZERO external imports.** No framework, no ORM, no HTTP. Ever.
