# 05 — Architecture

> **What is this?** The system's design decisions: how Huila Travel Expedition is organized, why the architecture was selected, what alternatives were evaluated, and how the system is planned to evolve and be deployed. ADRs document the most important architectural decisions.

## Why this section exists

A system's architecture is the set of decisions that are hard to change later.
Documenting them has three benefits:

1. **New team members** understand the system without having to ask everything from scratch
2. **The team** does not repeat already-resolved discussions
3. **Years later**, everyone remembers why a specific technology or architecture was chosen

---

## What is here and how to fill it in

### `overview.md` ⭐ (Start here)

High-level view of the complete Huila Travel Expedition system.

**Filled with:**

* C4 Level 1 System Context diagram.
* C4 Level 2 Container diagram.
* Main logical modules and their responsibilities.
* Communication patterns.
* Technologies used in each layer.
* Initial architectural style: **Modular Monolith + Hexagonal Architecture**.

The current architecture does not define independent deployable microservices because the SRS does not require them at this stage.

**File:** `overview.md`

The main modules are:

| Service / Module              | Responsibility                                              | Technology                            | DB    |
| ----------------------------- | ----------------------------------------------------------- | ------------------------------------- | ----- |
| `web-application`             | Responsive interface and access to platform functionality   | Laravel / PHP + Bootstrap or Tailwind | —     |
| `authentication-module`       | Registration, authentication and role-based access          | Laravel / PHP                         | MySQL |
| `agency-module`               | Agency registration and management                          | Laravel / PHP                         | MySQL |
| `tourist-plans-module`        | Tourist plans, images, classification, search and filtering | Laravel / PHP                         | MySQL |
| `availability-module`         | Tourist plan availability                                   | Laravel / PHP                         | MySQL |
| `reservation-module`          | Reservation requests, approval, cancellation and history    | Laravel / PHP                         | MySQL |
| `review-module`               | Ratings and reviews                                         | Laravel / PHP                         | MySQL |
| `administration-module`       | Agency verification, statistics, reports and featured plans | Laravel / PHP                         | MySQL |
| `notification-support-module` | Confirmation emails and support requests                    | Laravel / PHP                         | MySQL |

### Communication patterns

* **Sync:** HTTPS requests through the web application and internal application calls between modules.
* **Async:** No message broker is adopted in the initial architecture. Internal application events may be considered for notifications.
* **Gateway:** No API Gateway is adopted because the initial system is a modular monolith.

### Technology per layer

| Layer                | Technology                                         |
| -------------------- | -------------------------------------------------- |
| Presentation         | Bootstrap or Tailwind                              |
| Backend              | PHP 8.2+                                           |
| Framework            | Laravel 10+                                        |
| Database             | MySQL 8.0                                          |
| Alternative database | PostgreSQL                                         |
| Cache                | Redis                                              |
| Communication        | HTTPS / HTTP                                       |
| Security             | HTTPS, SSL, password hashing and role-based access |

---

### `deployment.md` ⭐

This file will document how Huila Travel Expedition is deployed in each environment.

Based on the SRS, the project considers:

* Initial deployment on shared hosting.
* Possible migration to VPS.
* Migration technical work estimated at less than 8 hours without rewriting the application.
* Weekly backups stored separately.
* Recovery target of less than 4 hours.
* 99% monthly availability target.

**Current status:**

The SRS does not define the final infrastructure diagram, Docker configuration, Kubernetes configuration, exact network topology or VPS hardware specifications.

Therefore, these elements must be defined before treating the deployment architecture as final.

**File:** `deployment.md`

---

### `cross-cutting.md`

This file documents concerns that apply across the Huila Travel Expedition modules.

The current cross-cutting requirements include:

| Concern            | Solution                                       |
| ------------------ | ---------------------------------------------- |
| Authentication     | Secure authentication                          |
| Authorization      | Role-based access control                      |
| Passwords          | Laravel native hashing; never store plain text |
| Transport security | HTTPS + valid SSL                              |
| Personal data      | Law 1581 of 2012                               |
| Failed login       | 5 failed attempts → 15-minute block            |
| Sessions           | Expire after 30 minutes of inactivity          |
| Validation         | Real-time form validation                      |
| Data integrity     | ACID transactions and locking                  |
| Backups            | Weekly backups in separate storage             |
| Availability       | 99% monthly target                             |
| Images             | Maximum 500 KB with compression                |
| Audit              | Audit logging and access controls              |

The SRS does not define specific distributed tracing or monitoring technologies such as Jaeger, OpenTelemetry or Prometheus.

---

### `pattern-guide.md`

This file contains the design pattern catalog for Huila Travel Expedition.

Patterns documented include:

* Factory Method.
* Builder.
* Singleton.
* Adapter.
* Decorator.
* Observer / Internal Event Bus.
* Strategy.
* Template Method.
* API Gateway.
* BFF.
* Strangler Fig.
* REST / gRPC.
* Message Broker.
* Circuit Breaker.
* Retry.
* Database per Service.
* ACID Transactions.
* Saga.
* CQRS.
* Outbox.
* Event Sourcing.
* Sidecar.

The current architecture adopts or supports:

* **Modular Monolith**
* **Hexagonal Architecture**
* **ACID Transactions**
* **Database Locking**
* **Adapter**

Other patterns remain proposed, future or not adopted unless an architectural need and corresponding ADR justify them.

**File:** `pattern-guide.md`

---

### `security-threat-model.md`

This file will document security threats using the **STRIDE methodology**:

* **Spoofing**
* **Tampering**
* **Repudiation**
* **Information Disclosure**
* **Denial of Service**
* **Elevation of Privilege**

The SRS already defines several security mitigations that must be considered in this analysis:

| Threat area            | Current mitigation from SRS                                  |
| ---------------------- | ------------------------------------------------------------ |
| Spoofing               | Authentication and role-based access                         |
| Tampering              | HTTPS, validation and access controls                        |
| Repudiation            | Audit logging                                                |
| Information Disclosure | HTTPS, password hashing and personal-data protection         |
| Denial of Service      | Login attempt blocking and capacity/performance requirements |
| Elevation of Privilege | Role-based access control                                    |

The complete threat model must be documented in:

`security-threat-model.md`

---

### `decisions/` ⭐⭐ — Architecture Decision Records (ADRs)

#### What is an ADR?

A record of ONE important architectural decision: what was decided, why, what alternatives
were evaluated, and what the consequences are.

For Huila Travel Expedition, ADRs should be used for decisions that could require significant refactoring if changed later.

### Current architectural decision

The initial architecture is proposed as:

**Modular Monolith + Hexagonal Architecture**

Reason:

* The SRS defines a web platform with several functional areas.
* The SRS does not require independent microservices.
* The SRS does not define a message broker.
* The SRS does not define an API Gateway.
* The SRS does not define database-per-service.
* A modular monolith allows clear separation without unnecessary distributed complexity.

**Potential ADR file:**

`decisions/ADR-001-architectural-style.md`

> The ADR must be reviewed and approved by the team before the decision is considered formally adopted.

---

## Correlations with other sections

| This section is fed by...                    | And feeds...                                           |
| -------------------------------------------- | ------------------------------------------------------ |
| `02-domain/domain-map.md` → bounded contexts | `09-microservices/` → future service boundaries        |
| `04-requirements/non-functional.md` → NFRs   | Decisions about technology, performance and scale      |
| ADRs chosen here                             | `09-microservices/` implements future decided patterns |
| `deployment.md`                              | `10-devops/environments.md`                            |
| `04-requirements/functional.md`              | Application modules and use cases                      |
| `05-architecture/hexagonal-architecture.md`  | Domain, application and infrastructure organization    |
| `05-architecture/pattern-guide.md`           | Design pattern application                             |

---

## The 5 most common architecture mistakes

The following principles are used as review criteria for Huila Travel Expedition:

1. **Microservices too small** — The project should not create independent services without a real reason and defined boundaries.
2. **Shared database** — A shared database would be a problem if the system were decomposed into independent microservices. The current system is a modular monolith, so MySQL is shared by the application modules.
3. **Only synchronous communication** — Asynchronous communication may be considered in the future for non-critical processes, such as notifications, if justified.
4. **No API Gateway** — An API Gateway is not required while the system remains a modular monolith. It may be evaluated if independent services are introduced.
5. **No documented decisions** — Important architectural decisions must be recorded in ADRs to avoid repeating previous discussions.

---

## Questions this section must answer

### How is the system organized into large blocks?

Huila Travel Expedition is initially organized as a **Modular Monolith + Hexagonal Architecture**.

The main logical blocks are:

* Authentication and Authorization.
* Agency Management.
* Tourist Plans and Search.
* Availability.
* Reservations.
* Reviews.
* Administration and Reports.
* Notifications and Support.
* MySQL database.
* Redis cache.

### Why was each key technology chosen?

The technologies are based on the Huila Travel Expedition SRS:

* **Laravel 10+** — proposed application framework.
* **PHP 8.2+** — proposed backend language/runtime.
* **MySQL 8.0** — proposed primary database.
* **PostgreSQL** — identified as an alternative database.
* **Redis** — proposed for caching.
* **Bootstrap or Tailwind** — proposed for the responsive interface.

### What alternatives were evaluated and why were they discarded?

The SRS identifies PostgreSQL as an alternative to MySQL.

For architecture, independent microservices, API Gateway, message broker and database-per-service are possible future options, but they are not required by the current SRS.

They are therefore **not adopted for the initial architecture**.

The decision can be revisited if future requirements justify distributed architecture.

### How is the system deployed?

The SRS indicates:

```text
Initial environment
        ↓
Shared Hosting
        ↓
Huila Travel Expedition
        ↓
Possible future migration
        ↓
VPS
```

The SRS also establishes:

* Weekly backups.
* Separate backup storage.
* Recovery in less than 4 hours.
* 99% monthly availability.
* Migration from shared hosting to VPS without application rewriting, with technical work estimated at less than 8 hours.

The exact Docker, Kubernetes, network and hardware configuration is not specified in the SRS and must be defined in `deployment.md`.

### What patterns does the team apply and how?

The current architecture applies:

* **Modular Monolith** — organizes the application into logical modules.
* **Hexagonal Architecture** — separates domain logic from external technologies.
* **Adapter** — separates external integrations from the application.
* **ACID Transactions** — protects reservation integrity.
* **Database Locking** — helps prevent reservation conflicts and overbooking.

Other patterns are documented in `pattern-guide.md` but are not automatically considered adopted.

---

## Architecture source of truth

The **Huila Travel Expedition SRS** is the primary source of truth for:

* Functional requirements.
* Non-functional requirements.
* Security requirements.
* Performance requirements.
* Reservation integrity.
* Technology stack.
* Availability.
* Backup and recovery requirements.
* Initial project scope.

Architectural elements that are not explicitly defined in the SRS must be treated as proposals until validated by the technical team.

---

## Main architecture files

```text
05-architecture/
│
├── README.md
├── overview.md
├── deployment.md
├── cross-cutting.md
├── pattern-guide.md
├── security-threat-model.md
│
└── decisions/
    ├── _template-adr.md
    └── ADR-001-architectural-style.md
```

### Current status

| File                                       | Status                      |
| ------------------------------------------ | --------------------------- |
| `README.md`                                | Filled                      |
| `overview.md`                              | Filled                      |
| `deployment.md`                            | Pending                     |
| `cross-cutting.md`                         | Pending                     |
| `pattern-guide.md`                         | Filled                      |
| `security-threat-model.md`                 | Pending                     |
| `decisions/ADR-001-architectural-style.md` | Proposed / pending approval |

---

## Source of Truth

The **Huila Travel Expedition SRS** is the primary source for the requirements and proposed technology stack.

The architecture documents complement the SRS by recording technical organization and decisions.

No technology, microservice, API Gateway, message broker or deployment component should be considered mandatory unless it is supported by the SRS or formally approved through an ADR.
