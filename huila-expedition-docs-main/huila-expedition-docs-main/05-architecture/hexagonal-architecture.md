# Hexagonal Architecture (Ports & Adapters)

> Hexagonal architecture organizes a service so that the **business domain is independent from external technologies**.
>
> For Huila Travel Expedition, the business rules related to agencies, tourists, tourist plans, availability, reservations and reviews should not depend directly on the database, HTTP framework or other external technologies.

> **Stack note:** The Huila Travel Expedition SRS proposes Laravel 10+, PHP 8.2+, MySQL 8.0, Redis and Bootstrap/Tailwind. The final implementation of the hexagonal architecture and its exact framework structure must be defined by the technical team.

---

## The problem it solves

A traditional architecture can make business logic highly dependent on controllers, database queries or framework-specific code.

For Huila Travel Expedition, this could make it difficult to change the database, modify the web layer or test reservation and availability rules independently.

### Hexagonal Architecture

```text
                 PRIMARY ADAPTERS
        ┌──────────────┬──────────────┐
        │              │              │
   HTTP/Web        Automated       Tests
        │              │              │
        └──────────────┴──────────────┘
                       │
                 [Driving Ports]
                       │
              ┌──────────────────┐
              │                  │
              │     DOMAIN       │
              │                  │
              │ Agencies         │
              │ Tourists         │
              │ Tourist Plans    │
              │ Reservations     │
              │ Availability     │
              │ Reviews          │
              │                  │
              └──────────────────┘
                       │
                 [Driven Ports]
                       │
             ┌─────────┴─────────┐
             │                   │
       Database Adapter     External Services
             │                   │
           MySQL             Email / other
```

> The exact final service boundaries and external integrations are **not specified in the SRS** and must be defined during architecture design.

---

## Folder structure

The following structure is a proposed organization for a service using hexagonal architecture:

```text
src/
├── domain/
│   ├── agency/
│   │   ├── Agency.php
│   │   ├── AgencyId.php
│   │   ├── events/
│   │   └── ports/
│   │       ├── in/
│   │       └── out/
│   │
│   ├── tourist/
│   ├── tourist-plan/
│   ├── reservation/
│   ├── availability/
│   ├── review/
│   └── shared/
│       └── value-objects/
│
├── application/
│   ├── agency/
│   ├── tourist/
│   ├── tourist-plan/
│   ├── reservation/
│   ├── availability/
│   └── review/
│
├── infrastructure/
│   ├── adapters/
│   │   ├── in/
│   │   │   └── http/
│   │   └── out/
│   │       ├── persistence/
│   │       └── external/
│   └── config/
│
└── main.php
```

> This is a **proposed structure**, not a final implementation requirement. The SRS specifies the technology stack and functional requirements but does not define this exact folder structure.

---

## The Ports

Ports are abstract contracts that allow the business logic to communicate with the outside world without depending on a concrete technology.

### Driving Port (Input Port)

A driving port defines an operation that the application can execute.

For example, a reservation request could be represented conceptually as:

```php
interface RequestReservationPort
{
    public function execute(RequestReservationRequest $request);
}
```

For Huila Travel Expedition, possible input operations include:

* Register agency
* Authenticate user
* Create tourist plan
* Search tourist plans
* Request reservation
* Approve reservation
* Cancel reservation
* Submit review

> Exact interface names and implementation are **proposed** because the SRS does not define them.

### Driven Port (Output Port)

A driven port defines what the domain needs from an external system.

For example:

```php
interface ReservationRepositoryPort
{
    public function save(Reservation $reservation): void;

    public function findById(string $id): ?Reservation;
}
```

The domain depends on this contract instead of directly depending on MySQL or another database.

---

## The Adapters

### Primary Adapter — HTTP Controller

The HTTP controller receives requests from the web application and translates them into application use cases.

Conceptually:

```text
HTTP Request
     ↓
Controller
     ↓
Driving Port
     ↓
Application Use Case
     ↓
Domain
```

For example:

```text
POST /reservations
        ↓
ReservationController
        ↓
RequestReservationUseCase
        ↓
Reservation Domain
```

> The exact endpoint `/reservations` is illustrative. The SRS does not define final API routes.

---

### Secondary Adapter — Repository

The repository implements a driven port and communicates with the database.

```text
Domain
   ↓
ReservationRepositoryPort
   ↓
ReservationRepository
   ↓
MySQL
```

The domain does not need to know whether the data is stored in MySQL, PostgreSQL or another persistence technology.

The SRS identifies **MySQL 8.0** as the proposed primary database and PostgreSQL as an alternative.

---

## The Use Case (Application Service)

The application layer coordinates the execution of a use case.

For example, the reservation process can be represented as:

```text
Tourist
   ↓
Reservation Controller
   ↓
Request Reservation Use Case
   ↓
Check Availability
   ↓
Create Reservation
   ↓
Save Reservation
   ↓
Return Result
```

The SRS specifies that reservation processing must maintain data integrity and prevent overbooking through **ACID transactions and locking**.

Therefore, the reservation use case must coordinate these operations while the corresponding business rules remain in the domain.

---

## Domain responsibilities

The domain should contain the business rules of Huila Travel Expedition without depending directly on Laravel, MySQL or HTTP.

Examples of domain responsibilities include:

### Agency

* Agency registration rules.
* Agency information.
* Agency verification requirements.

### Tourist Plans

* Tourist plan creation and modification.
* Destination information.
* Tourism type classification.
* Rates.
* Availability information.

### Reservations

* Reservation request.
* Reservation status.
* Approval and cancellation.
* Reservation integrity.
* Prevention of overbooking.

### Reviews

* Rating and review information.
* Rules related to submitting reviews.
* Administrative moderation rules.

---

## The Dependency Rule

> **Dependencies always point inward.**

```text
Infrastructure
      ↓
Application
      ↓
Domain

Domain
  ↑
must not depend on Infrastructure
or Application
```

The domain should not import:

* Laravel infrastructure components.
* Database-specific implementations.
* HTTP controllers.
* External APIs.

Instead, the domain defines the required interfaces and the infrastructure implements them.

---

## Dependency inversion in practice

Conceptually:

```php
// Domain
interface ReservationRepositoryPort
{
    public function save(Reservation $reservation): void;
}
```

The infrastructure implements the contract:

```php
// Infrastructure
class ReservationRepository implements ReservationRepositoryPort
{
    public function save(Reservation $reservation): void
    {
        // Persistence implementation
    }
}
```

The application use case receives the interface:

```php
class RequestReservationUseCase
{
    public function __construct(
        private ReservationRepositoryPort $reservationRepository
    ) {}
}
```

This allows the business logic to remain independent from the concrete database implementation.

---

## Application to Huila Travel Expedition

The architecture can organize the main business capabilities as follows:

| Business capability | Possible domain area | Main responsibility                      |
| ------------------- | -------------------- | ---------------------------------------- |
| Agency registration | Agency               | Register and manage agencies             |
| Authentication      | Authentication       | Authenticate users and control access    |
| Tourist plans       | Tourist Plan         | Create and manage tourism offers         |
| Search              | Tourist / Search     | Search and filter offers                 |
| Availability        | Availability         | Manage dates and capacity                |
| Reservations        | Reservation          | Request, approve and cancel reservations |
| History             | Reservation          | Consult tourist reservation history      |
| Reviews             | Review               | Ratings and reviews                      |
| Administration      | Administration       | Statistics, reports and featured plans   |
| Notifications       | Notification         | Reservation confirmation emails          |

> These areas are logical groupings based on the SRS. They are not a final microservice decomposition.

---

## Reservation integrity

Reservation processing is a critical business rule.

The SRS specifies that the system must use **ACID transactions and locking mechanisms** to maintain reservation integrity and prevent overbooking.

The conceptual flow is:

```text
Tourist requests reservation
            ↓
Check availability
            ↓
Begin transaction
            ↓
Lock relevant availability
            ↓
Validate capacity
            ↓
Create reservation
            ↓
Update availability
            ↓
Commit transaction
```

If the required capacity is not available, the reservation request must be rejected.

---

## Security considerations

The architecture must support the security requirements defined in the SRS:

* HTTPS with valid SSL.
* Passwords must never be stored in plain text.
* Laravel native password hashing is proposed.
* Role-based access control.
* Protection of personal information according to Law 1581 of 2012.
* Audit logging for security-related operations.
* Access controls according to the assigned role.

Authentication and authorization details should remain behind appropriate application/infrastructure boundaries instead of placing framework-specific security logic inside the domain.

---

## Advantages for TDD

Hexagonal architecture supports testing because the domain can be tested independently from external technologies.

### Domain tests

Business rules can be tested without starting the complete web application or connecting to the production database.

Examples:

```text
Test: Reservation cannot exceed available capacity
Test: Invalid reservation state cannot be approved
Test: Invalid plan information is rejected
Test: Unauthorized role cannot access administrative functionality
```

### Application tests

Use cases can be tested using fake repositories or other test adapters instead of real infrastructure.

```text
Use Case
   ↓
Fake Repository
   ↓
Test Result
```

This reduces dependence on the real database during unit testing.

---

## Hexagonal Architecture Checklist

When reviewing a service or pull request, verify:

* [ ] `domain/` does not depend on infrastructure.
* [ ] `domain/` does not depend directly on HTTP controllers.
* [ ] Repository interfaces are defined as ports.
* [ ] Use cases communicate through defined ports.
* [ ] Database implementations remain in infrastructure.
* [ ] HTTP adapters remain outside the domain.
* [ ] Business rules are located in the domain.
* [ ] Reservation integrity rules are tested.
* [ ] Role-based access rules are tested.
* [ ] Mappers and persistence logic remain outside the domain.

---

## Common mistakes (anti-patterns)

| Anti-pattern                               | Why it is bad                                        | Solution                                       |
| ------------------------------------------ | ---------------------------------------------------- | ---------------------------------------------- |
| Business logic inside controllers          | Couples business rules to HTTP                       | Move business rules to the domain              |
| Domain directly querying MySQL             | Couples the domain to the database                   | Define a repository port                       |
| Domain importing Laravel components        | Couples business logic to the framework              | Keep framework dependencies outside the domain |
| Repository returning only database records | Domain cannot apply its business rules correctly     | Reconstruct domain objects                     |
| Excessive logic in one use case            | Makes the application difficult to maintain and test | Divide responsibilities into smaller use cases |
| Hard-coded external services               | Makes integrations difficult to replace or test      | Use driven ports and adapters                  |

---

## References and correlations

* Requirements → `04-requirements/functional.md`
* Non-functional requirements → `04-requirements/non-functional.md`
* Domain map → `02-domain/domain-map.md`
* Entities and invariants → `02-domain/entities-and-rules.md`
* Domain events → `02-domain/domain-events.md`
* API contracts → `07-api/contracts/openapi/`
* Microservices → `09-microservices/services/`
* Testing strategy → `11-quality/testing-strategy.md`
* TDD guide → `11-quality/tdd-guide.md`

---

## Source of Truth

The **Huila Travel Expedition SRS** is the primary source for the business requirements, security requirements, reservation integrity rules and proposed technology stack.

The hexagonal structure, ports, adapters and folder organization described here are an architectural proposal based on those requirements. They should be validated by the technical team before being treated as final implementation decisions.
