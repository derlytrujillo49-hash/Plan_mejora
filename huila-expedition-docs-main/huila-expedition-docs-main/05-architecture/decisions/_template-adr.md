# ADR-001 — Architectural Style

* **ID:** ADR-001
* **Date:** 2026-09-17
* **Status:** Proposed
* **Authors:** Huila Travel Expedition Team

---

## Context

Huila Travel Expedition is a local web platform designed to centralize, promote and allow comparison of tourist offers from travel agencies in the department of Huila.

The system contains several functional areas, including authentication, agency management, tourist plans, availability, reservations, reviews, administration, reports and notifications.

An architectural style must be defined to organize these responsibilities and maintain clear separation between business logic and infrastructure.

The SRS defines a web platform and its functional and non-functional requirements, but it does not require independent microservices, an API Gateway, a message broker, Kubernetes or a database-per-service strategy.

**Known constraints:**

* The project is initially a web platform.
* Laravel 10+ and PHP 8.2+ are proposed technologies.
* MySQL 8.0 is the proposed primary database.
* Redis is proposed for caching.
* Reservations require ACID transactions and database locking to prevent overbooking.
* The system should support future evolution without requiring unnecessary distributed-system complexity.

---

## Decision

**We decided:** Use a **Modular Monolith combined with Hexagonal Architecture (Ports and Adapters)** as the initial architectural style for Huila Travel Expedition.

**Justification:**

This architecture allows the system to be divided into clear logical modules while keeping the application as a single deployable system.

Hexagonal Architecture separates the domain and application logic from infrastructure technologies such as MySQL, Laravel and external email services.

This approach also supports the reservation integrity requirements because the initial system can use ACID transactions and database locking without introducing distributed transactions.

The architecture can evolve toward independent microservices in the future if project requirements justify the additional complexity.

---

## Evaluated alternatives

| Alternative                                            | Pros                                                                                                                      | Cons                                                                                                  | Reason for discarding                            |
| ------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------- | ------------------------------------------------ |
| **Modular Monolith + Hexagonal Architecture — chosen** | Clear module separation; simpler deployment; supports separation of domain and infrastructure; easier initial development | Modules share the same application deployment and database                                            | — (chosen)                                       |
| **Independent Microservices**                          | Independent deployment and scaling; service-level separation                                                              | Higher infrastructure and communication complexity; requires additional distributed-system management | Not required by the current SRS                  |
| **Traditional Monolith**                               | Simple deployment and development                                                                                         | Can create stronger coupling between components and make boundaries less clear                        | Does not provide the desired modular separation  |
| **Serverless Architecture**                            | Automatic scaling and reduced server management                                                                           | Adds platform dependencies and is not defined by the SRS                                              | Not required by the current project requirements |

---

## Consequences

**Positive:**

* The application has clear logical module boundaries.
* Business logic can remain independent from infrastructure technologies.
* The system can use a single MySQL database during the initial stage.
* Reservation integrity can be handled with ACID transactions and database locking.
* Deployment is simpler than a distributed microservices architecture.
* The architecture can evolve toward microservices if future requirements justify it.

**Negative / Trade-offs:**

* The initial modules are not independently deployable services.
* Modules share the same application deployment.
* The architecture requires discipline to maintain module boundaries.
* A future migration to microservices may require additional refactoring.

**Impact on the system:**

* **Affected services:** Authentication, Agency Management, Tourist Plans, Availability, Reservations, Reviews, Administration, Notifications and Support.
* **Documents that must be updated:** `05-architecture/overview.md`, `05-architecture/hexagonal-architecture.md`, `05-architecture/pattern-guide.md`, `09-microservices/` documentation and deployment documentation when applicable.

---

## Risks

| Risk                                                   | Probability | Impact | Mitigation                                                                                 |
| ------------------------------------------------------ | ----------- | ------ | ------------------------------------------------------------------------------------------ |
| Module boundaries become unclear                       | Medium      | Medium | Define responsibilities for each module and review dependencies.                           |
| Strong coupling between modules appears                | Medium      | High   | Apply dependency inversion and Hexagonal Architecture principles.                          |
| Future migration to microservices requires refactoring | Medium      | Medium | Maintain clear module boundaries and document architectural decisions.                     |
| Single database becomes a scalability limitation       | Low         | Medium | Monitor system requirements and evaluate database or service decomposition if necessary.   |
| Architecture becomes unnecessarily complex             | Low         | Medium | Introduce additional patterns or infrastructure only when justified by a real requirement. |

---

## References

* Huila Travel Expedition SRS → Functional and non-functional requirements
* System architecture overview → `05-architecture/overview.md`
* Hexagonal Architecture → `05-architecture/hexagonal-architecture.md`
* Pattern guide → `05-architecture/pattern-guide.md`
* Microservices documentation → `09-microservices/`
* Related ADRs → `05-architecture/decisions/`
