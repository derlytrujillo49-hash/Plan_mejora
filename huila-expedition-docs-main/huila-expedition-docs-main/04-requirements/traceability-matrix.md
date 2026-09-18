# Traceability Matrix

> Traceability connects the requirements defined in the Huila Travel Expedition SRS with user stories, validation tests, implementations and responsible services.
>
> It helps identify requirements without user stories, user stories without tests, and functionality that does not have a documented requirement.

---

## How to use this matrix

```text
Requirement → HU → Test Case → Implementation → Service
```

* If a requirement has no HU: it has not been planned.
* If a HU has no test case: its validation is not yet documented.
* If a test exists without implementation: there is technical debt.
* If code exists without a requirement or HU: it should be reviewed to determine whether it is necessary.

---

## FR → HU → Test → Service matrix

| FR ID | FR Description                                              | HU(s)           | Tests that verify it | Service                        | Status     |
| ----- | ----------------------------------------------------------- | --------------- | -------------------- | ------------------------------ | ---------- |
| RF1   | Agency registration                                         | HU-AGENCIA-001  | Not defined yet      | Agency / Authentication        | 🔴 Pending |
| RF2   | Secure authentication                                       | HU-AUTH-002     | Not defined yet      | Authentication                 | 🔴 Pending |
| RF3   | Update contact information and social networks              | Not defined yet | Not defined yet      | Agency                         | 🔴 Pending |
| RF4   | Create, edit and delete tourist plans                       | HU-AGENCIA-003  | Not defined yet      | Agency / Tourist Plans         | 🔴 Pending |
| RF5   | Upload destination images                                   | Not defined yet | Not defined yet      | Tourist Plans / Media          | 🔴 Pending |
| RF6   | Classify plans by type of tourism                           | Not defined yet | Not defined yet      | Tourist Plans                  | 🔴 Pending |
| RF7   | Search and filter plans by municipality, price and duration | HU-TURISTA-005  | Not defined yet      | Tourist / Search               | 🔴 Pending |
| RF8   | Configure differentiated rates                              | Not defined yet | Not defined yet      | Agency / Tourist Plans         | 🔴 Pending |
| RF9   | Manage calendar availability                                | HU-AGENCIA-007  | Not defined yet      | Agency / Availability          | 🔴 Pending |
| RF10  | Request reservations                                        | HU-TURISTA-008  | Not defined yet      | Reservation                    | 🔴 Pending |
| RF11  | Approve or cancel reservations                              | HU-AGENCIA-009  | Not defined yet      | Reservation                    | 🔴 Pending |
| RF12  | Send reservation confirmation emails                        | Not defined yet | Not defined yet      | Notification                   | 🔴 Pending |
| RF13  | Consult reservation history                                 | HU-TURISTA-010  | Not defined yet      | Reservation                    | 🔴 Pending |
| RF14  | Rate and review tourist plans                               | HU-TURISTA-011  | Not defined yet      | Review                         | 🔴 Pending |
| RF15  | Administrator dashboard and statistics                      | HU-ADMIN-012    | Not defined yet      | Administration                 | 🔴 Pending |
| RF16  | Role-based access                                           | HU-AUTH-002     | Not defined yet      | Authentication / Authorization | 🔴 Pending |
| RF17  | Manage featured tourist plans                               | HU-ADMIN-014    | Not defined yet      | Administration                 | 🔴 Pending |
| RF18  | Generate PDF reports                                        | HU-ADMIN-013    | Not defined yet      | Administration / Reports       | 🔴 Pending |
| RF19  | Support/contact form                                        | Not defined yet | Not defined yet      | Support                        | 🔴 Pending |
| RF20  | Terms and conditions acceptance                             | Not defined yet | Not defined yet      | Authentication / Reservation   | 🔴 Pending |

> **Note:** The HU identifiers already defined in the product backlog are used where there is a direct relationship with an SRS functional requirement. User stories and test cases that have not yet been created are marked accordingly.

---

## NFR → Validation matrix

| NFR ID  | Description                                                                         | How it is validated                       | Tool                                 | Status     |
| ------- | ----------------------------------------------------------------------------------- | ----------------------------------------- | ------------------------------------ | ---------- |
| NFR-001 | 95% of queries must respond in less than 3 seconds                                  | Performance testing                       | Lighthouse / performance tests       | 🔴 Pending |
| NFR-002 | 99% monthly availability                                                            | Availability monitoring                   | Monitoring system                    | 🔴 Pending |
| NFR-003 | Support 30–50 simultaneous users and up to 500 concurrent users during peak traffic | Load and concurrency testing              | Load testing tool to be defined      | 🔴 Pending |
| NFR-004 | Secure authentication, HTTPS, password protection and role-based access             | Security and authorization tests          | Security testing tools to be defined | 🔴 Pending |
| NFR-005 | Responsive interface and browser compatibility                                      | Functional and compatibility testing      | Browser testing                      | 🔴 Pending |
| NFR-006 | Uploaded images must not exceed 500 KB                                              | File upload and validation testing        | Functional tests                     | 🔴 Pending |
| NFR-007 | Data integrity and reservation consistency                                          | Database and concurrent reservation tests | Database tests                       | 🔴 Pending |
| NFR-008 | Migration to VPS without rewriting the base code                                    | Deployment/migration test                 | Deployment environment               | 🔴 Pending |
| NFR-009 | Security-related operations must be traceable                                       | Audit-log verification                    | Application logs                     | 🔴 Pending |
| NFR-010 | Periodic technological updating and technical support                               | Maintenance verification                  | Maintenance process                  | 🔴 Pending |
| NFR-011 | Weekly backups and restoration in less than 4 hours                                 | Backup and recovery test                  | Backup/recovery tools                | 🔴 Pending |

> The SRS defines the measurable requirements, but it does not establish a final CI/CD pipeline, specific automated testing framework, or final observability platform. Those values must be defined by the technical team.

---

## Inverse traceability: HU → FR

| HU             | Title                           | FR(s) it implements | Sprint   |
| -------------- | ------------------------------- | ------------------- | -------- |
| HU-AGENCIA-001 | Register travel agency          | RF1                 | Sprint 1 |
| HU-AUTH-002    | Secure user authentication      | RF2, RF16           | Sprint 1 |
| HU-AGENCIA-003 | Manage tourist plans            | RF4                 | Sprint 1 |
| HU-TURISTA-004 | Consult tourist plans           | RF5, RF6            | Sprint 1 |
| HU-TURISTA-005 | Search and filter tourist plans | RF7                 | Sprint 1 |
| HU-TURISTA-006 | Compare tourist offers          | RF7                 | Sprint 1 |
| HU-AGENCIA-007 | Manage availability             | RF9                 | Sprint 2 |
| HU-TURISTA-008 | Request reservation             | RF10                | Sprint 2 |
| HU-AGENCIA-009 | Manage reservations             | RF11                | Sprint 2 |
| HU-TURISTA-010 | Consult reservation history     | RF13                | Sprint 2 |
| HU-TURISTA-011 | Rate and review tourist plans   | RF14                | Sprint 2 |
| HU-ADMIN-012   | View platform statistics        | RF15                | Sprint 2 |
| HU-ADMIN-013   | Generate PDF reports            | RF18                | Sprint 2 |
| HU-ADMIN-014   | Manage featured content         | RF17                | Sprint 2 |

---

## Status legend

| Status         | Meaning                                      |
| -------------- | -------------------------------------------- |
| ✅ Done         | Implemented and validated                    |
| 🟡 In progress | Currently under development                  |
| 🔴 Pending     | Planned but not yet implemented or validated |
| ⏸ Blocked      | Has an identified blocker                    |
| ❌ Cancelled    | Removed from the project scope               |

> At the current documentation stage, the requirements and HUs are being documented. Therefore, they are marked as **Pending** until implementation and testing evidence exists.

---

## Identified gaps (requirements without coverage)

| Gap type        | Description                                       | Required action                                                   | Owner                 | Date       |
| --------------- | ------------------------------------------------- | ----------------------------------------------------------------- | --------------------- | ---------- |
| FR without HU   | RF3 has no specific HU documented yet             | Create a user story for contact and social information management | Product Owner         | 2026-09-17 |
| FR without HU   | RF5 has no specific HU documented yet             | Create a user story for destination images                        | Product Owner         | 2026-09-17 |
| FR without HU   | RF6 has no specific HU documented yet             | Create a user story for tourism type classification               | Product Owner         | 2026-09-17 |
| FR without HU   | RF8 has no specific HU documented yet             | Create a user story for differentiated rates                      | Product Owner         | 2026-09-17 |
| FR without HU   | RF12 has no specific HU documented yet            | Create a user story for reservation confirmation emails           | Product Owner         | 2026-09-17 |
| FR without HU   | RF19 has no specific HU documented yet            | Create a user story for the support form                          | Product Owner         | 2026-09-17 |
| FR without HU   | RF20 has no specific HU documented yet            | Create a user story for terms and conditions acceptance           | Product Owner         | 2026-09-17 |
| HU without test | Current HUs do not yet have documented test cases | Create acceptance/validation tests                                | QA / Development Team | 2026-09-17 |

---

## How to maintain this matrix

1. When an HU is created, associate it with the corresponding RF.
2. When a test case is created, add its reference to the corresponding RF/HU.
3. When the implementation is completed and validated, update the status.
4. During Sprint Planning, review requirements without HUs.
5. During Sprint Review, verify that completed HUs have corresponding tests and implementation evidence.
6. Update this matrix whenever requirements, user stories or implementation status changes.

---

## Correlations

* User Stories → `03-product/product-backlog.md`
* Functional Requirements → `04-requirements/functional.md`
* Non-Functional Requirements → `04-requirements/non-functional.md`
* Testing Strategy → `11-quality/testing-strategy.md`
* Definition of Done → `00-governance/definition-of-done.md`
* Security Policy → `00-governance/security-policy.md`
* Architecture → `05-architecture/README.md`
* Microservices → `09-microservices/services/`

---

## Source of Truth

The **Huila Travel Expedition SRS** is the primary source for the functional and non-functional requirements referenced in this matrix.

Where a requirement does not yet have a corresponding HU, test case or implementation, it is explicitly identified as a gap rather than assuming that the functionality already exists.
