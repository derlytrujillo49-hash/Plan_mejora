# Design Patterns and Microservices Guide

> This document is the pattern catalog for Huila Travel Expedition.
> For each pattern: when to use it, when NOT to use it, and how it can apply to the project.
>
> Patterns are not recipes — they are tools. They should be adopted only when the project has a real problem that requires them.

---

## Stack note

The Huila Travel Expedition SRS proposes:

* Laravel 10+
* PHP 8.2+
* MySQL 8.0
* PostgreSQL as an alternative
* Redis for caching
* Bootstrap or Tailwind for the interface

The patterns described in this document are mostly technology-agnostic. Concrete implementation decisions must be documented through the corresponding ADRs.

---

## Index

**Design patterns**

1. [Creational patterns](#creational)
2. [Structural patterns](#structural)
3. [Behavioral patterns](#behavioral)

**Microservices patterns**

4. [System decomposition](#decomposition)
5. [Inter-service communication](#communication)
6. [Resilience](#resilience)
7. [Data and consistency](#data)
8. [Observability](#observability)

---

# Design patterns (GoF and SOLID)

<a name="creational"></a>

## 1. Factory Method

**Problem:**
An object must be created without exposing all of its creation logic to the rest of the application.

**When to use it:**

* When different types of domain objects may need to be created.
* When creation requires validations or business rules.
* When object creation should remain inside the domain.

**Possible Huila Travel Expedition example:**

A factory could be used to create different types of tourist plans when the creation process has different business rules.

```php
class TouristPlanFactory
{
    public static function create(array $data): TouristPlan
    {
        // Validate required information
        // Apply creation rules
        return new TouristPlan($data);
    }
}
```

**When NOT to use it:**

* When object creation is simple.
* When introducing a factory would add unnecessary complexity.

---

## 2. Builder

**Problem:**
An object contains many optional attributes and direct construction becomes difficult to read.

**When to use it:**

* Complex tourist plans.
* Test data creation.
* Objects with many optional properties.

**Example:**

```php
$plan = TouristPlanBuilder::create()
    ->withName('Tour San Agustín')
    ->withDuration(3)
    ->withPrice(450000)
    ->withMunicipality('San Agustín')
    ->build();
```

> This is an illustrative example. The SRS does not require a Builder implementation.

**When NOT to use it:**

* When the object has only a few simple attributes.
* When a normal constructor is easier to understand.

---

## 3. Singleton (with caution)

**Problem:**
A component requires a single shared instance.

**Possible uses:**

* Configuration management.
* Specific infrastructure resources.

**Warning:**
Singletons can make testing and dependency management more difficult.

For Huila Travel Expedition, **dependency injection is preferred** instead of manually implementing Singleton classes.

```php
class Configuration
{
    // Prefer framework/container-managed instances
}
```

**When NOT to use it:**

* For ordinary domain entities.
* When dependency injection can solve the same problem.
* When the singleton would create hidden global state.

---

<a name="structural"></a>

## 4. Adapter

**Problem:**
The application needs to use an external component whose interface is different from the interface required by the domain.

**When to use it:**

* External email providers.
* Future payment gateways.
* External APIs.
* Infrastructure implementations.

**Huila Travel Expedition example:**

The application can define an email port:

```php
interface NotificationPort
{
    public function sendConfirmation(
        string $email,
        string $message
    ): void;
}
```

An external email provider can then implement the adapter:

```php
class EmailServiceAdapter implements NotificationPort
{
    public function sendConfirmation(
        string $email,
        string $message
    ): void {
        // Translate application data
        // into the external provider format.
    }
}
```

This keeps the domain independent from the concrete email provider.

**When NOT to use it:**

* When there is no external interface to adapt.
* When the abstraction adds complexity without solving a real integration problem.

---

## 5. Decorator

**Problem:**
Additional behavior is required without modifying the original object.

**Possible Huila Travel Expedition uses:**

* Logging.
* Caching.
* Validation.
* Performance measurement.

Example:

```php
class CachedTouristPlanRepository
{
    public function __construct(
        private TouristPlanRepository $repository,
        private CacheService $cache
    ) {}

    public function findById(string $id)
    {
        // Check cache first.
        // If unavailable, query repository.
    }
}
```

The SRS proposes Redis for caching, so a cache decorator could be considered if caching becomes necessary.

**When NOT to use it:**

* When the additional behavior is simple enough to be handled directly.
* When multiple decorators make the code difficult to understand.

---

<a name="behavioral"></a>

## 6. Observer / Internal Event Bus

**Problem:**
An operation needs to notify other components without creating direct dependencies between them.

**Possible Huila Travel Expedition uses:**

* Reservation confirmation.
* Review submission.
* Agency registration.
* Reservation approval.

Conceptually:

```text
Reservation approved
        ↓
Domain/Application Event
        ↓
Notification handler
        ↓
Confirmation email
```

Example:

```php
class ReservationApproved
{
    public function __construct(
        public string $reservationId
    ) {}
}
```

**When NOT to use it:**

* When the operation is simple and direct communication is sufficient.
* When events would make the flow unnecessarily difficult to follow.

---

## 7. Strategy

**Problem:**
The system needs to apply different algorithms or rules depending on a specific situation.

**Possible Huila Travel Expedition uses:**

* Different pricing rules.
* Different tourism classifications.
* Different report formats.
* Different notification mechanisms.

Example:

```php
interface PricingStrategy
{
    public function calculate(float $basePrice): float;
}

class StandardPricing implements PricingStrategy
{
    public function calculate(float $basePrice): float
    {
        return $basePrice;
    }
}

class SpecialRatePricing implements PricingStrategy
{
    public function calculate(float $basePrice): float
    {
        return $basePrice;
    }
}
```

> The SRS requires differentiated rates but does not define the exact pricing algorithms. Therefore, this is only a possible design approach.

**When NOT to use it:**

* When there is only one algorithm.
* When creating multiple strategies only for theoretical flexibility.

---

## 8. Template Method

**Problem:**
Several processes follow the same general structure but have different steps.

**Possible use:**

Generating different types of reports:

```text
Validate data
      ↓
Transform data
      ↓
Generate report
      ↓
Register export
```

The SRS requires PDF reports for administrators.

```php
abstract class ReportGenerator
{
    public function generate(array $data)
    {
        $validated = $this->validate($data);
        $transformed = $this->transform($validated);

        return $this->createReport($transformed);
    }

    protected function validate(array $data)
    {
        return $data;
    }

    abstract protected function transform(array $data);

    abstract protected function createReport(array $data);
}
```

**When NOT to use it:**

* When reports do not share a common process.
* When composition or separate services are simpler.

---

# Microservices Patterns

<a name="decomposition"></a>

## Decomposition

### API Gateway

**Problem:**
Clients must communicate with several independent services.

```text
                  ┌──────────────────┐
Web Application ─▶│   API Gateway    │
                  └────────┬─────────┘
                           │
                ┌──────────┼──────────┐
                ↓          ↓          ↓
             Agency     Plans     Reservation
             Service    Service      Service
```

**Current project status:** Not adopted.

Huila Travel Expedition is initially documented as a **Modular Monolith + Hexagonal Architecture**. The SRS does not define independent deployable microservices or an API Gateway.

**When to use it:**

* When the system has multiple independent services.
* When routing, authentication or aggregation must be centralized.

**When NOT to use it:**

* When the system is a modular monolith.
* When there are no independent services requiring centralized routing.

---

### Backend for Frontend (BFF)

**Problem:**
Different clients require significantly different APIs.

```text
Web ──▶ BFF Web ──▶ Internal Services

Mobile ──▶ BFF Mobile ──▶ Internal Services
```

**Current project status:** Not adopted.

The SRS defines a responsive web platform and does not define separate mobile and web APIs.

**When to use it:**

* When different clients have substantially different data requirements.

**When NOT to use it:**

* When there is only one main client.
* When a standard API can serve the clients adequately.

---

### Strangler Fig

**Problem:**
A monolithic system needs to be gradually migrated to independent services.

```text
Phase 1:
Client → Monolith

Phase 2:
Client → Gateway → Monolith + New Service

Phase 3:
Client → Gateway → New Services
```

**Current project status:** Future option.

This pattern could be considered if Huila Travel Expedition grows and specific modules need to become independent services.

---

<a name="communication"></a>

# Inter-service communication

## Synchronous: REST / gRPC

### REST

REST is appropriate for HTTP APIs used by the web application.

Possible communication:

```text
Web Application
       ↓ HTTPS
Application API
       ↓
Huila Travel Expedition modules
```

**Current project status:** REST/HTTP is a possible API approach, but the final API architecture is not yet defined in the SRS.

### gRPC

gRPC could be considered if independent internal services are introduced in the future.

**Current project status:** Not adopted.

**When to use synchronous communication:**

* When the client needs an immediate response.
* For searches and queries.
* For reservation operations requiring an immediate result.

---

## Asynchronous: Message Broker

Examples include Kafka and RabbitMQ.

```text
[Module A]
    │
    │ Event
    ▼
[Message Broker]
    │
    ▼
[Module B]
```

**Current project status:** Not adopted.

The SRS does not specify Kafka, RabbitMQ or another message broker.

Possible future use:

* Sending notification emails asynchronously.
* Processing reports.
* Integrating independently deployed services.

**When NOT to use it:**

* When the operation requires a simple immediate response.
* When introducing a broker only adds infrastructure complexity.

---

<a name="resilience"></a>

# Resilience

## Circuit Breaker

**Problem:**
A failing external service can cause repeated failures in the application.

```text
Normal:

Application → External Service
                 ↓
              Response


Failure:

Application → Circuit Breaker → External Service
                    ↓
                 Fallback
```

**Current project status:** Not adopted.

It could be considered if Huila Travel Expedition depends on several external services.

**When to use it:**

* External services may become unavailable.
* A slow external service could affect the application.
* A fallback is possible.

**When NOT to use it:**

* For operations completely internal to the same application.
* When there is no external dependency requiring protection.

---

## Retry with Exponential Backoff

**Problem:**
A temporary network or external-service failure occurs.

Example concept:

```text
Attempt 1 → fail
    ↓
wait
Attempt 2 → fail
    ↓
wait longer
Attempt 3 → success/fail
```

**Possible use:** External email notifications.

**Current project status:** Not formally adopted.

Retries must be used carefully to avoid sending duplicate notifications or creating excessive traffic.

---

<a name="data"></a>

# Data and consistency

## Database per Service

**Rule:**
Each independent microservice has its own database.

```text
Service A → Database A

Service B → Database B
```

**Current project status:** No.

Huila Travel Expedition is initially a modular monolith, and the SRS proposes MySQL as the primary database.

Therefore, a database-per-service strategy is not required for the initial architecture.

If the system is later decomposed into microservices, this decision must be reviewed through an ADR.

---

## ACID Transactions

**Problem:**
Several database operations must be completed consistently as one transaction.

**Current project status:** Adopted for reservation integrity.

The SRS specifically requires ACID transactions and locking mechanisms to prevent overbooking.

Conceptual flow:

```text
BEGIN TRANSACTION
      ↓
Check availability
      ↓
Lock availability
      ↓
Create reservation
      ↓
Update availability
      ↓
COMMIT
```

If an operation fails:

```text
ROLLBACK
```

This is particularly important for simultaneous reservation requests.

---

## Saga

**Problem:**
A business transaction spans several independent services and cannot use a single ACID transaction.

**Current project status:** Not adopted.

The current architecture can use a local ACID transaction because the initial system is a modular monolith.

```text
Reservation
     ↓
Availability
     ↓
Database Transaction
```

A Saga should only be considered if the reservation process is later distributed across independent services.

**When NOT to use it:**

* When the transaction fits inside one service or application.
* When a simple ACID transaction is sufficient.

---

## CQRS

**Problem:**
The read model and write model have very different requirements.

**Current project status:** Not adopted.

The SRS does not require separate read and write models.

A conventional application model is sufficient for the initial implementation.

**When to use it:**

* Very high read volume.
* Complex read models.
* Different scalability requirements for reading and writing.

**When NOT to use it:**

* When read and write operations are relatively simple.
* When the additional infrastructure would not provide a clear benefit.

---

## Outbox Pattern

**Problem:**
The application must guarantee that a database operation and an event publication are reliably coordinated.

**Current project status:** Not adopted.

It may become useful if Huila Travel Expedition later adopts asynchronous events and independent microservices.

```text
Transaction
   ├── Save business data
   └── Save event in Outbox
              ↓
          Event Relay
              ↓
        Message Broker
```

The pattern should not be introduced before an event-driven architecture is actually required.

---

## Event Sourcing

**Problem:**
The system needs to reconstruct its state from a complete history of events.

**Current project status:** Not adopted.

Huila Travel Expedition does not require event sourcing according to the SRS.

The application can use conventional database persistence.

**When NOT to use it:**

* When complete event history is not a business requirement.
* When conventional persistence is sufficient.
* When the additional complexity is not justified.

---

<a name="observability"></a>

# Observability

## Sidecar Pattern

**Problem:**
Infrastructure capabilities such as logging, metrics or network management need to be added without modifying application code.

```text
┌─────────────────────────────┐
│ Application                 │
│                             │
│ Sidecar / Infrastructure    │
└─────────────────────────────┘
```

**Current project status:** Not adopted.

This pattern is mainly useful in containerized environments such as Kubernetes and is not required by the current SRS.

---

# When NOT to use each pattern

| Pattern              | Do not use it when...                                                   |
| -------------------- | ----------------------------------------------------------------------- |
| Factory Method       | Object creation is simple and does not require abstraction.             |
| Builder              | The object has few simple attributes.                                   |
| Singleton            | Dependency injection can solve the same problem.                        |
| Adapter              | There is no external interface requiring translation.                   |
| Decorator            | The added behavior is simple and does not justify another abstraction.  |
| Observer / Event Bus | Direct communication is simpler and easier to understand.               |
| Strategy             | There is only one algorithm or rule.                                    |
| Template Method      | Processes do not share a common structure.                              |
| API Gateway          | The system is not composed of independent microservices.                |
| BFF                  | Clients have similar requirements.                                      |
| Circuit Breaker      | The dependency is internal and does not create cascading failures.      |
| Saga                 | The transaction fits within a single application transaction.           |
| CQRS                 | Read and write models are not significantly different.                  |
| Event Sourcing       | Complete event history is not required.                                 |
| Database per Service | The architecture is still a modular monolith.                           |
| Outbox Pattern       | There is no asynchronous event publication requiring reliable delivery. |
| Sidecar              | The infrastructure does not require sidecar-based deployment.           |

---

# Patterns adopted in this project

> The following decisions are based on the current Huila Travel Expedition architecture and SRS. Patterns that are not supported by the current architecture remain unadopted.

| Pattern                       | Adopted? | Justification / ADR                                                                                                                               |
| ----------------------------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| Factory Method                | Proposed | May be used when domain object creation requires business validation.                                                                             |
| Builder                       | Proposed | May be used for complex objects or test data.                                                                                                     |
| Adapter                       | Yes      | Supports separation between application ports and external/infrastructure implementations. Reference: `05-architecture/hexagonal-architecture.md` |
| Decorator                     | Proposed | May be used for caching, logging or validation when justified.                                                                                    |
| Observer / Internal Event Bus | Proposed | May support decoupled notification processes if required.                                                                                         |
| Strategy                      | Proposed | May support differentiated pricing or other variable business rules.                                                                              |
| Template Method               | Proposed | May be used for report-generation flows with shared processing steps.                                                                             |
| API Gateway                   | No       | Initial architecture is a modular monolith.                                                                                                       |
| Database per Service          | No       | Initial architecture uses a shared application database.                                                                                          |
| Circuit Breaker               | No       | No microservice/external dependency architecture requiring it has been defined.                                                                   |
| Saga (choreographed)          | No       | Reservation integrity is handled through ACID transactions and locking.                                                                           |
| Outbox Pattern                | No       | No message broker/event-driven architecture has been adopted.                                                                                     |
| CQRS                          | No       | The SRS does not require separate read/write models.                                                                                              |
| Event Sourcing                | No       | Complete event-sourced history is not a current requirement.                                                                                      |
| BFF                           | No       | The SRS defines a responsive web platform and does not require separate client-specific backends.                                                 |
| Sidecar                       | No       | Not required by the current deployment architecture.                                                                                              |

> **Important:** “Proposed” means the pattern may be useful in implementation but is not mandatory. “Yes” means it is consistent with the adopted Hexagonal Architecture. Microservices patterns marked “No” must not be implemented as if they were already architectural decisions.

---

# Correlations

* Hexagonal Architecture → `05-architecture/hexagonal-architecture.md`
* System Architecture Overview → `05-architecture/system-architecture-overview.md`
* ADR for pattern decisions → `05-architecture/decisions/`
* Domain map → `02-domain/domain-map.md`
* Functional requirements → `04-requirements/functional.md`
* Non-functional requirements → `04-requirements/non-functional.md`
* API contracts → `07-api/contracts/openapi/`
* UML diagrams → `08-uml/`
* Microservices documentation → `09-microservices/`
* Testing strategy → `11-quality/testing-strategy.md`
* Technical backlog → `15-project-control/technical-backlog.md`

---

# Source of Truth

The **Huila Travel Expedition SRS** is the primary source for business, functional, non-functional and technical requirements.

The architectural patterns in this document must not be considered mandatory merely because they appear in the pattern catalog.

A pattern becomes an adopted architectural decision only when:

1. There is a real problem that justifies it.
2. The technical team validates the solution.
3. The decision is documented in an ADR when appropriate.
4. The implementation is consistent with the project's architecture.

The initial architecture is **Modular Monolith + Hexagonal Architecture**. Microservices patterns remain possible future options if the project evolves and their complexity is justified.
