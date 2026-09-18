# Non-Functional Requirements (NFR)

> NFRs define the qualities of the Huila Travel Expedition system, specifying how well the system must perform rather than what functionality it provides.
>
> The following NFRs are based on the requirements established in the Huila Travel Expedition SRS.

---

## NFR-001: Performance

| Attribute                   | Metric                     | Test condition                                      |
| --------------------------- | -------------------------- | --------------------------------------------------- |
| Main page loading           | ≤ 3 seconds                | Under normal conditions                             |
| Tourist plan detail loading | ≤ 3 seconds                | Under normal conditions                             |
| Query response              | 95% of queries < 3 seconds | Normal system operation                             |
| Frequent database queries   | < 1 second                 | Frequent queries according to database requirements |

**Defined critical operations:**

* Main page loading — critical because it is the first point of access for tourists.
* Tourist plan detail loading — critical because tourists need to review information before making a reservation.
* Reservation processing — the SRS establishes processing reservations in less than 3 seconds.

**Performance validation tools:**

* Google PageSpeed
* Lighthouse

**Where is it validated?**

Performance must be verified through functional and performance tests before deployment.

---

## NFR-002: Availability

| Environment | SLO                  | Maintenance window             | Maximum downtime/month |
| ----------- | -------------------- | ------------------------------ | ---------------------- |
| Production  | 99% monthly          | Scheduled maintenance excluded | 7.2 hours              |
| Development | Not specified in SRS | Not specified                  | Not specified          |

**Monthly availability target in production:** 99%.

The SRS specifies that the service must remain available 99% of the time, except during scheduled maintenance.

**Health checks:**

The SRS does not define specific `/health` or `/health/ready` endpoints.

Health monitoring and operational endpoints may be defined later in the architecture and operations documentation.

---

## NFR-003: Scalability and Concurrency

The SRS defines scalability and concurrent-user requirements at two levels.

| Scenario                     | Expected behavior                                                    |
| ---------------------------- | -------------------------------------------------------------------- |
| Normal concurrent navigation | Support 30–50 simultaneous users without service degradation         |
| Peak business traffic        | Support up to 500 concurrent users according to RT05                 |
| Reservation processing       | Process reservations in less than 3 seconds                          |
| Growth of the platform       | Support increased volume of agencies and reservations                |
| Infrastructure scaling       | Migration from shared hosting to VPS without rewriting the base code |

### Initial infrastructure

The SRS establishes an initial shared-hosting environment of:

```text
1 vCPU
1 GB RAM
5 GB SSD
```

The initial concurrent navigation requirement is **30–50 users without service degradation**.

### Scaled infrastructure

The SRS establishes a possible VPS environment of:

```text
2 vCPU
4 GB RAM
20 GB SSD
```

The migration to VPS must be completable in **less than 8 hours of technical work**.

The SRS also defines RT05 as a requirement to support at least **500 concurrent users during traffic peaks**, process reservations in less than 3 seconds and scale without performance degradation.

---

## NFR-004: Security

### Authentication and Authorization

The SRS requires secure authentication and role-based access control.

The system defines three main roles:

```text
Administrator
Agency
Tourist
```

Protected functionality must only be accessible according to the corresponding role and permissions.

The SRS does **not** define JWT expiration times or refresh-token durations, so these values must not be considered mandatory NFRs at this stage.

### Data transmission

* HTTPS is mandatory for communication between the client and server.
* The system must use a valid and current SSL certificate.
* HTTP access must be automatically redirected to HTTPS.

### Password security

* Passwords must never be stored in plain text.
* Passwords must use Laravel's native hashing mechanism.
* The original password must not be recoverable from the stored value.

### Personal data

Personal data must be handled according to **Law 1581 of 2012**.

The system must record explicit acceptance of terms and conditions with the corresponding date and time.

### Information security

According to RT03, the system must protect user and transaction information through:

* Encryption.
* Robust authentication.
* Audit logging.
* Access controls.

### Security validation

Security must be verified through:

* Authentication tests.
* Authorization tests.
* Password-storage verification.
* HTTPS verification.
* Input validation.
* Access-control tests.

The SRS references Law 1581 of 2012, Law 1480 of 2011 and PCI-DSS as part of the project's legal and security considerations.

---

## NFR-005: Usability and Compatibility

Although usability and compatibility are separate RNFs in the SRS, they are included here because they define system quality from the user's perspective.

### Responsive interface

| Attribute          | Metric                          | Test condition             |
| ------------------ | ------------------------------- | -------------------------- |
| Responsive design  | Functional from 320px width     | Mobile, tablet and desktop |
| Interface approach | Mobile-first                    | Supported devices          |
| Touch interaction  | Buttons, forms and menus usable | Touch-screen devices       |

The interface must work correctly on:

* Mobile phones.
* Tablets.
* Desktop computers.

The SRS proposes **Bootstrap or Tailwind CSS** for the mobile-first implementation.

### Browser compatibility

| Browser | Requirement                             |
| ------- | --------------------------------------- |
| Chrome  | Compatible with the latest two versions |
| Firefox | Compatible with the latest two versions |
| Safari  | Compatible with the latest two versions |
| Edge    | Compatible with the latest two versions |

Compatibility must be verified before each deployment.

### Form validation

Registration and reservation forms must:

* Validate information in real time.
* Display clear and specific error messages.
* Display messages in Spanish.
* Prevent submission when required fields are invalid.

---

## NFR-006: Image and Storage Performance

The system must automatically compress or resize images uploaded to the platform.

| Attribute          | Metric                       |
| ------------------ | ---------------------------- |
| Maximum image size | 500 KB per image             |
| Storage capacity   | 5–10 GB according to the SRS |
| Compression        | Automatic                    |
| Visual quality     | No significant visual loss   |

### Expected behavior

```text
Upload image
      ↓
Validate image
      ↓
Compress / resize
      ↓
Verify maximum size
      ↓
Store image
```

The compression must occur without significantly affecting the user experience.

This requirement corresponds to **RNF2** of the SRS.

---

## NFR-007: Reliability and Data Integrity

The system must maintain the integrity and reliability of information, especially for reservations, calendars and inventory.

### Reservation integrity

The database must use mechanisms such as:

* ACID transactions.
* Locking mechanisms.
* Referential integrity.
* Availability verification.

These mechanisms must prevent overbooking when multiple reservations are processed concurrently.

### Database performance

The SRS establishes that frequent queries should have response times below **1 second**.

### Backup

| Requirement        | Metric                    |
| ------------------ | ------------------------- |
| Database backup    | Weekly                    |
| System-file backup | Weekly                    |
| Backup storage     | Separate from main server |
| Restoration        | Less than 4 hours         |

The SRS establishes periodic backup and a recovery time of less than 4 hours.

---

## NFR-008: Portability and Technological Updating

The SRS establishes that the application must be capable of moving from shared hosting to a VPS without rewriting the base code.

| Attribute           | Metric                              |
| ------------------- | ----------------------------------- |
| Migration to VPS    | Less than 8 hours of technical work |
| Base code rewriting | Not required                        |
| Technology updating | Periodic                            |

### Technology

The SRS proposes:

```text
Backend: Laravel 10 or higher
PHP: 8.2+
Database: MySQL 8.0
Alternative database: PostgreSQL
Cache: Redis
Frontend: Bootstrap or Tailwind CSS
```

The system must receive periodic software and component updates to maintain security patches and adapt to technological changes.

---

## NFR-009: Observability and Audit Logging

The SRS establishes **audit logging** as part of information security.

Security-relevant operations should be traceable, including:

```text
Successful authentication
Failed authentication
Unauthorized access
Agency verification
Reservation operations
Review moderation
Administrative actions
```

Logs must support the identification of:

* User involved, when applicable.
* Action performed.
* Date and time.
* Resource or operation affected.
* Result of the operation.

### Important limitation

The SRS does **not** define specific metrics for:

* Log processing time.
* Alert response time.
* Distributed tracing.
* Correlation IDs.
* Prometheus/Grafana.
* OpenTelemetry.
* PagerDuty.

Therefore, these values and technologies are not mandatory NFRs at this stage and may be defined later in the operations and architecture documentation.

---

## NFR-010: Maintainability and Technical Support

The SRS establishes the need for permanent and qualified technical support and periodic technological updates.

### Requirements

* Technical support must be available to address incidents.
* Software components must be updated periodically.
* Security patches must be maintained.
* The system must adapt to technological changes.
* Migration to VPS must not require rewriting the base code.

### Metrics defined by the SRS

| Attribute          | Metric                      |
| ------------------ | --------------------------- |
| VPS migration      | < 8 hours of technical work |
| Technology updates | Periodic                    |
| Technical support  | Permanent and qualified     |

The SRS does **not** define metrics for test coverage, cyclomatic complexity, technical debt or build time. These should only be added if the team later establishes them as project standards.

---

## NFR-011: Disaster Recovery and Backup

| Scenario                | Recovery requirement             |
| ----------------------- | -------------------------------- |
| Database/system failure | Restoration in less than 4 hours |
| Data backup             | Weekly                           |
| Backup storage          | Separate from main server        |

### Recovery flow

```text
System failure
      ↓
Identify latest valid backup
      ↓
Restore backup
      ↓
Verify database integrity
      ↓
Verify application
      ↓
Restore service
```

**Recovery Time Objective (RTO):** less than 4 hours.

The SRS does not define a specific **Recovery Point Objective (RPO)**, so no RPO value is established in this document.

---

## NFR Priority Matrix

| NFR               | Priority         | Validated in CI?     | Owner            |
| ----------------- | ---------------- | -------------------- | ---------------- |
| Performance       | High             | Not specified in SRS | Development team |
| Availability      | High             | Not specified in SRS | Technical team   |
| Scalability       | High             | Not specified in SRS | Technical team   |
| Security          | High / Essential | Not specified in SRS | Development team |
| Usability         | High             | Not specified in SRS | Development team |
| Compatibility     | High             | Not specified in SRS | Development team |
| Reliability       | High             | Not specified in SRS | Technical team   |
| Portability       | High / Essential | Not specified in SRS | Technical team   |
| Observability     | High / Essential | Not specified in SRS | Technical team   |
| Maintainability   | High / Essential | Not specified in SRS | Technical team   |
| Disaster Recovery | High             | Not specified in SRS | Technical team   |

> **Note:** The SRS establishes priorities such as High/Essential for the requirements but does not define CI/CD validation pipelines or individual owners for each NFR. Therefore, these fields must be confirmed by the team before becoming formal project rules.

---

## NFR Traceability to the SRS

| NFR                                            | SRS Requirement                    |
| ---------------------------------------------- | ---------------------------------- |
| NFR-001 Performance                            | RNF1, RT05                         |
| NFR-002 Availability                           | RNF10                              |
| NFR-003 Scalability and Concurrency            | RNF3, RNF12, RT05                  |
| NFR-004 Security                               | RNF7, RNF8, RNF9, RT03, RF16, RF20 |
| NFR-005 Usability and Compatibility            | RNF4, RNF5, RNF6                   |
| NFR-006 Image Performance                      | RNF2                               |
| NFR-007 Reliability and Data Integrity         | RNF11 + database requirements      |
| NFR-008 Portability and Technological Updating | RNF12, RT04                        |
| NFR-009 Observability and Audit Logging        | RT03                               |
| NFR-010 Maintainability and Technical Support  | RT02, RT04                         |
| NFR-011 Disaster Recovery                      | RNF11                              |

---

## Correlations

* Governance security rules → `00-governance/security-policy.md`
* Technical security rules → `00-governance/technical-security-rules.md`
* User stories → `03-product/product-backlog.md`
* Functional requirements → `04-requirements/functional.md`
* Architecture → `05-architecture/README.md`
* Database requirements → `06-data/models.md`
* API documentation → `07-api/`
* Microservices → `09-microservices/services/`
* DevOps and deployment → `10-devops/`
* Operations and SLOs → `13-operations/`

---

## Source of Truth

The **Huila Travel Expedition SRS** is the primary source for these Non-Functional Requirements.

The measurable values defined in this document come from the SRS, including:

```text
RNF1  → 95% of queries < 3 seconds
RNF2  → Images ≤ 500 KB
RNF3  → 30–50 simultaneous users
RNF4  → Responsive from 320px
RNF5  → Latest two versions of Chrome, Firefox, Safari and Edge
RNF6  → Real-time form validation
RNF7  → HTTPS + valid SSL certificate
RNF8  → Laravel native password hashing
RNF9  → Law 1581 of 2012
RNF10 → 99% monthly availability
RNF11 → Weekly backup + restoration < 4 hours
RNF12 → VPS migration < 8 hours of technical work
RT05  → Up to 500 concurrent users + reservations < 3 seconds
```

Where the SRS does not define a specific metric, technology or operational value, this document explicitly marks it as **not specified in the SRS** instead of introducing an unsupported project requirement.
