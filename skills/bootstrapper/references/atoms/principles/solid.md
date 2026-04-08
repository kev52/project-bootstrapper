## SOLID Principles

- **S: One reason to change.** If a class handles both validation and persistence, split it.
- **O: Extend, don't modify.** New behavior = new class implementing an interface. No if/else branches growing in existing classes.
- **L: Subtypes must be substitutable.** If overriding a method changes the contract, the inheritance is wrong. Prefer composition.
- **I: Thin interfaces.** If a class implements an interface but throws NotImplemented on some methods, the interface is too fat. Split it.
- **D: Depend on abstractions.** Every service boundary gets an interface. Constructor injection only. No service locators, no static access.
