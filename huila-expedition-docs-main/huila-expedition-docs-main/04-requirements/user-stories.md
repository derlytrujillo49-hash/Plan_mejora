# User Stories — Backlog

> **What to fill in here:** The product's User Story backlog.
> Each HU uses the standard format with Acceptance Criteria in Given/When/Then.
> Refined (Ready) HUs go to the sprint. Unrefined ones are epics or ideas.

---

## Backlog status

| Cut   | Sprint     | Total HUs | Refined | In progress | Completed |
| ----- | ---------- | --------- | ------- | ----------- | --------- |
| Cut 1 | Sprint 1-2 | 14        | 14      | 0           | 0         |
| Cut 2 | Sprint 3-4 | 0         | 0       | 0           | 0         |

> **Note:** The current backlog contains 14 defined HUs. They are considered Ready based on the available SRS and product backlog information. No implementation or completion evidence is documented yet, so In Progress and Completed remain at 0.

---

## Epics

| ID     | Epic                      | Description                                                                           |
| ------ | ------------------------- | ------------------------------------------------------------------------------------- |
| EP-001 | Agency Management         | Register agencies, manage agency information, tourist plans and availability.         |
| EP-002 | Authentication and Access | Provide secure authentication and role-based access to the platform.                  |
| EP-003 | Tourist Discovery         | Allow tourists to consult, search, filter and compare tourist offers.                 |
| EP-004 | Reservations              | Allow tourists to request reservations and agencies to manage them.                   |
| EP-005 | Reviews and History       | Allow tourists to review completed interactions and manage their reservation history. |
| EP-006 | Administration            | Provide administrators with statistics, reports and featured content management.      |

---

## User Stories

### HU-AGENCIA-001 — Register travel agency {#HU-AGENCIA-001}

**Epic:** EP-001

> **As** a travel agency representative
> **I want** to register my travel agency on Huila Travel Expedition
> **so that** I can access the platform and manage my tourist plans and services.

**Acceptance Criteria:**

```gherkin
Scenario 1: Valid agency registration
  Given the agency is not registered
  When the representative enters all required registration information
  Then the system validates the information
  And creates the agency registration

Scenario 2: Invalid or incomplete registration
  Given the registration form contains missing or invalid information
  When the representative submits the form
  Then the system displays the corresponding validation errors
  And does not complete the registration

Scenario 3: Duplicate agency registration
  Given the agency is already registered
  When the representative attempts to register the same agency again
  Then the system rejects the duplicate registration

Scenario 4: Administrative verification
  Given the agency registration has been created
  When administrative verification is required
  Then the agency remains pending verification until the Administrator validates the required information
```

| Field               | Value                   |
| ------------------- | ----------------------- |
| Story Points        | 5                       |
| Priority            | Must Have               |
| Target sprint       | Sprint 1                |
| Assigned to         | Not specified           |
| Status              | Ready                   |
| Dependencies        | None                    |
| Affected service(s) | Agency / Authentication |

---

### HU-AUTH-002 — Secure user authentication {#HU-AUTH-002}

**Epic:** EP-002

> **As** a registered platform user
> **I want** to authenticate securely according to my role
> **so that** I can access the functions available to me while protecting my account.

**Acceptance Criteria:**

```gherkin
Scenario 1: Successful authentication
  Given the user has valid credentials
  When the user submits the login form
  Then the system authenticates the user
  And grants access according to the assigned role

Scenario 2: Invalid credentials
  Given the user enters invalid credentials
  When the user submits the login form
  Then the system rejects the authentication
  And displays an authentication error

Scenario 3: Repeated failed authentication
  Given the user has reached five consecutive failed login attempts
  When the user attempts to authenticate again
  Then the system blocks access for 15 minutes

Scenario 4: Session inactivity
  Given an authenticated user has been inactive for 30 minutes
  When the session timeout is reached
  Then the system expires the session
  And requires the user to authenticate again
```

| Field               | Value                          |
| ------------------- | ------------------------------ |
| Story Points        | 5                              |
| Priority            | Must Have                      |
| Target sprint       | Sprint 1                       |
| Assigned to         | Not specified                  |
| Status              | Ready                          |
| Dependencies        | HU-AGENCIA-001                 |
| Affected service(s) | Authentication / Authorization |

---

### HU-AGENCIA-003 — Manage tourist plans {#HU-AGENCIA-003}

**Epic:** EP-001

> **As** a travel agency representative
> **I want** to create, edit and delete tourist plans
> **so that** I can keep my agency's offers updated for tourists.

**Acceptance Criteria:**

```gherkin
Scenario 1: Create tourist plan
  Given the agency is authenticated
  When the representative enters valid tourist plan information
  Then the system creates the tourist plan
  And associates it with the agency

Scenario 2: Edit tourist plan
  Given the agency has an existing tourist plan
  When the representative modifies valid information
  Then the system saves the updated information

Scenario 3: Delete tourist plan
  Given the agency has an existing tourist plan
  When the representative requests its deletion
  Then the system removes or deactivates the tourist plan according to the platform rules

Scenario 4: Invalid plan information
  Given required tourist plan information is missing or invalid
  When the representative submits the plan
  Then the system displays validation errors
  And does not save the invalid information
```

| Field               | Value                       |
| ------------------- | --------------------------- |
| Story Points        | 5                           |
| Priority            | Must Have                   |
| Target sprint       | Sprint 1                    |
| Assigned to         | Not specified               |
| Status              | Ready                       |
| Dependencies        | HU-AGENCIA-001, HU-AUTH-002 |
| Affected service(s) | Agency / Tourist Plans      |

---

### HU-TURISTA-004 — Consult tourist plans {#HU-TURISTA-004}

**Epic:** EP-003

> **As** a tourist
> **I want** to consult tourist plans published by agencies
> **so that** I can review available destinations and services before choosing an offer.

**Acceptance Criteria:**

```gherkin
Scenario 1: View available plans
  Given tourist plans have been published
  When the tourist accesses the plans section
  Then the system displays the available tourist plans

Scenario 2: View plan details
  Given the tourist selects a published plan
  When the tourist opens the plan
  Then the system displays its available information
  And shows the destination, tourism type and offer details

Scenario 3: No available plans
  Given there are no published plans matching the available information
  When the tourist accesses the plans section
  Then the system displays an appropriate empty-result message
```

| Field               | Value                   |
| ------------------- | ----------------------- |
| Story Points        | 3                       |
| Priority            | Must Have               |
| Target sprint       | Sprint 1                |
| Assigned to         | Not specified           |
| Status              | Ready                   |
| Dependencies        | HU-AGENCIA-003          |
| Affected service(s) | Tourist / Tourist Plans |

---

### HU-TURISTA-005 — Search and filter tourist offers {#HU-TURISTA-005}

**Epic:** EP-003

> **As** a tourist
> **I want** to search and filter tourist offers by municipality, price and duration
> **so that** I can find offers that match my travel preferences.

**Acceptance Criteria:**

```gherkin
Scenario 1: Filter by municipality
  Given tourist plans are available for different municipalities
  When the tourist selects a municipality filter
  Then the system displays plans corresponding to that municipality

Scenario 2: Filter by price
  Given tourist plans have different prices
  When the tourist applies a price filter
  Then the system displays plans matching the selected price criteria

Scenario 3: Filter by duration
  Given tourist plans have different durations
  When the tourist applies a duration filter
  Then the system displays plans matching the selected duration criteria

Scenario 4: Combined filters
  Given multiple filter criteria are available
  When the tourist applies more than one filter
  Then the system displays offers matching the selected criteria
```

| Field               | Value            |
| ------------------- | ---------------- |
| Story Points        | 5                |
| Priority            | Must Have        |
| Target sprint       | Sprint 1         |
| Assigned to         | Not specified    |
| Status              | Ready            |
| Dependencies        | HU-TURISTA-004   |
| Affected service(s) | Tourist / Search |

---

### HU-TURISTA-006 — Compare tourist offers {#HU-TURISTA-006}

**Epic:** EP-003

> **As** a tourist
> **I want** to compare tourist offers
> **so that** I can evaluate differences in price, duration and included services before making a reservation decision.

**Acceptance Criteria:**

```gherkin
Scenario 1: Compare available offers
  Given multiple tourist offers are available
  When the tourist selects offers to compare
  Then the system displays their relevant information for comparison
  And allows the tourist to identify differences between the offers

Scenario 2: Compare offer information
  Given selected offers contain price, duration and service information
  When the comparison is displayed
  Then the system shows those attributes for each selected offer
```

| Field               | Value                          |
| ------------------- | ------------------------------ |
| Story Points        | 5                              |
| Priority            | Must Have                      |
| Target sprint       | Sprint 1                       |
| Assigned to         | Not specified                  |
| Status              | Ready                          |
| Dependencies        | HU-TURISTA-004, HU-TURISTA-005 |
| Affected service(s) | Tourist / Search               |

---

### HU-AGENCIA-007 — Manage tourist plan availability {#HU-AGENCIA-007}

**Epic:** EP-001

> **As** a travel agency representative
> **I want** to manage the availability calendar of my tourist plans
> **so that** tourists can request reservations according to the available dates and capacity.

**Acceptance Criteria:**

```gherkin
Scenario 1: Register availability
  Given the agency is authenticated
  When the representative enters valid availability information
  Then the system records the available dates and capacity

Scenario 2: Update availability
  Given a tourist plan has existing availability
  When the representative modifies the availability
  Then the system updates the calendar

Scenario 3: Unavailable date
  Given a date has no available capacity
  When a tourist attempts to request a reservation for that date
  Then the system does not allow the reservation request for that unavailable capacity
```

| Field               | Value                 |
| ------------------- | --------------------- |
| Story Points        | 5                     |
| Priority            | Should Have           |
| Target sprint       | Sprint 2              |
| Assigned to         | Not specified         |
| Status              | Ready                 |
| Dependencies        | HU-AGENCIA-003        |
| Affected service(s) | Agency / Availability |

---

### HU-TURISTA-008 — Request tourist reservation {#HU-TURISTA-008}

**Epic:** EP-004

> **As** a tourist
> **I want** to request a reservation for an available tourist plan
> **so that** I can reserve a tourism service with the selected agency.

**Acceptance Criteria:**

```gherkin
Scenario 1: Valid reservation request
  Given the tourist is authenticated and the selected plan has availability
  When the tourist submits a valid reservation request
  Then the system records the reservation request
  And associates it with the tourist and selected plan

Scenario 2: No availability
  Given the selected tourist plan has no available capacity
  When the tourist submits a reservation request
  Then the system rejects the request
  And informs the tourist that the plan is unavailable

Scenario 3: Concurrent reservation requests
  Given multiple users request the same available capacity
  When the system processes the requests
  Then the system maintains reservation integrity
  And prevents overbooking
```

| Field               | Value                                       |
| ------------------- | ------------------------------------------- |
| Story Points        | 5                                           |
| Priority            | Should Have                                 |
| Target sprint       | Sprint 2                                    |
| Assigned to         | Not specified                               |
| Status              | Ready                                       |
| Dependencies        | HU-TURISTA-004, HU-AGENCIA-007, HU-AUTH-002 |
| Affected service(s) | Reservation / Availability                  |

---

### HU-AGENCIA-009 — Manage reservations {#HU-AGENCIA-009}

**Epic:** EP-004

> **As** a travel agency representative
> **I want** to approve or cancel reservation requests
> **so that** I can manage reservations according to the availability of my tourist services.

**Acceptance Criteria:**

```gherkin
Scenario 1: Approve reservation
  Given a reservation request is pending
  When the agency approves the request
  Then the reservation status is updated to approved

Scenario 2: Cancel reservation
  Given a reservation request exists
  When the agency cancels the request
  Then the reservation status is updated to cancelled

Scenario 3: Invalid reservation action
  Given the reservation cannot be processed in its current state
  When the agency attempts an invalid action
  Then the system rejects the action
  And keeps the current reservation state
```

| Field               | Value          |
| ------------------- | -------------- |
| Story Points        | 5              |
| Priority            | Should Have    |
| Target sprint       | Sprint 2       |
| Assigned to         | Not specified  |
| Status              | Ready          |
| Dependencies        | HU-TURISTA-008 |
| Affected service(s) | Reservation    |

---

### HU-TURISTA-010 — View reservation history {#HU-TURISTA-010}

**Epic:** EP-005

> **As** a tourist
> **I want** to consult my reservation history
> **so that** I can review the tourist services I have previously requested.

**Acceptance Criteria:**

```gherkin
Scenario 1: View reservation history
  Given the tourist is authenticated and has reservation records
  When the tourist accesses the reservation history
  Then the system displays the tourist's reservations

Scenario 2: No reservation history
  Given the tourist has no reservation records
  When the tourist accesses the reservation history
  Then the system displays an appropriate empty-history message

Scenario 3: Access another user's history
  Given the tourist is authenticated
  When the tourist attempts to access another tourist's reservation history
  Then the system denies access
```

| Field               | Value                       |
| ------------------- | --------------------------- |
| Story Points        | 3                           |
| Priority            | Should Have                 |
| Target sprint       | Sprint 2                    |
| Assigned to         | Not specified               |
| Status              | Ready                       |
| Dependencies        | HU-TURISTA-008, HU-AUTH-002 |
| Affected service(s) | Reservation / Tourist       |

---

### HU-TURISTA-011 — Rate and review tourist services {#HU-TURISTA-011}

**Epic:** EP-005

> **As** a tourist
> **I want** to rate and review a tourist service
> **so that** I can share my experience and provide information that can help other tourists evaluate offers.

**Acceptance Criteria:**

```gherkin
Scenario 1: Submit review
  Given the tourist has an applicable reservation
  When the tourist submits a valid rating and review
  Then the system records the review

Scenario 2: Invalid review
  Given the review information is incomplete or invalid
  When the tourist submits the review
  Then the system displays validation errors
  And does not save the invalid review

Scenario 3: Administrative moderation
  Given a review has been submitted
  When administrative moderation is required
  Then the Administrator can moderate the review according to platform rules
```

| Field               | Value                       |
| ------------------- | --------------------------- |
| Story Points        | 5                           |
| Priority            | Should Have                 |
| Target sprint       | Sprint 2                    |
| Assigned to         | Not specified               |
| Status              | Ready                       |
| Dependencies        | HU-TURISTA-008, HU-AUTH-002 |
| Affected service(s) | Review / Administration     |

---

### HU-ADMIN-012 — View platform statistics {#HU-ADMIN-012}

**Epic:** EP-006

> **As** a system administrator
> **I want** to view platform statistics
> **so that** I can monitor the activity of agencies, tourists and reservations.

**Acceptance Criteria:**

```gherkin
Scenario 1: View dashboard statistics
  Given the Administrator is authenticated
  When the Administrator accesses the dashboard
  Then the system displays the available platform statistics

Scenario 2: Unauthorized access
  Given a user does not have the Administrator role
  When the user attempts to access the administration dashboard
  Then the system denies access
```

| Field               | Value          |
| ------------------- | -------------- |
| Story Points        | 5              |
| Priority            | Could Have     |
| Target sprint       | Sprint 2       |
| Assigned to         | Not specified  |
| Status              | Ready          |
| Dependencies        | HU-AUTH-002    |
| Affected service(s) | Administration |

---

### HU-ADMIN-013 — Generate PDF reports {#HU-ADMIN-013}

**Epic:** EP-006

> **As** a system administrator
> **I want** to generate PDF reports
> **so that** I can obtain summarized platform information for administrative purposes.

**Acceptance Criteria:**

```gherkin
Scenario 1: Generate report
  Given the Administrator is authenticated
  When the Administrator requests an available report
  Then the system generates the report in PDF format

Scenario 2: Unauthorized report access
  Given the user does not have the Administrator role
  When the user attempts to generate an administrative report
  Then the system denies the request
```

| Field               | Value                     |
| ------------------- | ------------------------- |
| Story Points        | 5                         |
| Priority            | Could Have                |
| Target sprint       | Sprint 2                  |
| Assigned to         | Not specified             |
| Status              | Ready                     |
| Dependencies        | HU-ADMIN-012, HU-AUTH-002 |
| Affected service(s) | Administration / Reports  |

---

### HU-ADMIN-014 — Manage featured tourist plans {#HU-ADMIN-014}

**Epic:** EP-006

> **As** a system administrator
> **I want** to manage featured tourist plans
> **so that** relevant tourism offers can receive greater visibility on the platform.

**Acceptance Criteria:**

```gherkin
Scenario 1: Feature a tourist plan
  Given the Administrator is authenticated and a published tourist plan exists
  When the Administrator marks the plan as featured
  Then the system identifies the plan as featured

Scenario 2: Remove featured status
  Given a tourist plan is currently featured
  When the Administrator removes its featured status
  Then the system no longer identifies the plan as featured

Scenario 3: Unauthorized access
  Given the user does not have the Administrator role
  When the user attempts to manage featured plans
  Then the system denies access
```

| Field               | Value                       |
| ------------------- | --------------------------- |
| Story Points        | 3                           |
| Priority            | Could Have                  |
| Target sprint       | Sprint 2                    |
| Assigned to         | Not specified               |
| Status              | Ready                       |
| Dependencies        | HU-AUTH-002, HU-AGENCIA-003 |
| Affected service(s) | Administration              |

---

## Rules for writing HUs

### 1. The role matters

Each HU identifies the specific platform role:

* Administrator
* Travel agency representative
* Tourist

The generic role **"user"** is not used when a more specific role is available.

### 2. The benefit justifies the work

Each story contains a business-oriented **"so that"** statement describing the value provided by the functionality.

### 3. Acceptance Criteria are verifiable

The acceptance criteria use **Given / When / Then** scenarios so that they can later be verified manually or converted into automated tests.

### 4. One HU = one unit of value

The stories are divided by individual business capabilities so that each one can be implemented and verified within a sprint.

---

## Ready-to-copy HU template

````markdown
### HU-00X — [Name] {#HU-00X}

**Epic:** EP-00X

> **As** [role]
> **I want** [action]
> **so that** [benefit]

**Acceptance Criteria:**

\```gherkin
Scenario 1: [name]
  Given [context]
  When  [action]
  Then  [result]
\```

| Field | Value |
|-------|-------|
| Story Points | |
| Priority | |
| Target sprint | |
| Status | Backlog |
| Dependencies | |
````

---

## Correlations

* Full template with DoD checklist → `04-requirements/_template-hu.md`
* Non-functional requirements → `04-requirements/non-functional.md`
* Traceability matrix → `04-requirements/traceability-matrix.md`
* Product backlog → `03-product/product-backlog.md`
* Functional requirements → `04-requirements/functional.md`
* API contracts derived from these HUs → `07-api/contracts/openapi/`
* Microservices → `09-microservices/services/`

---

## Source of Truth

The **Huila Travel Expedition SRS** is the primary source for the functional scope and acceptance criteria of these user stories.

Where the SRS does not define an exact API endpoint, event, implementation technology or operational workflow, this backlog does not treat those details as mandatory implementation decisions.
