# SOLID Principles (Object-Oriented Design)

SOLID is a widely used mnemonic for five design principles that keep object-oriented systems **maintainable** and **extensible**. They appear frequently in interviews and code reviews.

## S — Single Responsibility Principle (SRP)
A class should have **one reason to change**—one primary responsibility.

**Java smell**: a `UserService` that also sends emails, writes PDFs, and configures HTTP clients. Split by capability (`UserService`, `MailNotification`, `ReportRenderer`).

## O — Open/Closed Principle (OCP)
Types should be **open for extension** but **closed for modification** (often via interfaces, composition, or strategy pattern rather than editing core logic for every variant).

**Java example**: payment processing behind `PaymentProcessor` with `CardProcessor`, `WalletProcessor` implementations instead of giant `if/else` chains.

## L — Liskov Substitution Principle (LSP)
Subtypes must be **substitutable** for their base types without breaking callers’ expectations (contracts, invariants, exceptions).

**Java pitfall**: a `ReadOnlyList` that `throws` on `add` may violate expectations of `List` consumers—prefer narrower interfaces (`Collection` / custom read-only API).

## I — Interface Segregation Principle (ISP)
Prefer **small, focused** interfaces over one “fat” interface forcing implementers to provide meaningless methods.

**Java example**: split `Printer`, `Scanner` instead of a single `MultiFunctionDevice` interface when many clients only print.

## D — Dependency Inversion Principle (DIP)
High-level modules should depend on **abstractions**, not concrete implementations; details depend on abstractions.

**Java practice**: inject `Clock`, `UserRepository`, `PaymentGateway` interfaces into services; wire concrete classes in configuration (Spring `@Bean`).

## Using SOLID with Spring
Spring’s **dependency injection** supports DIP; **interface-based beans** and **segregated services** align with ISP/SRP. Avoid “god services” that grow without bounds.

## Related Topics
- `30_Design_Patterns.md`
- `10_Classes_and_Objects.md`
- `15_Interfaces.md`
