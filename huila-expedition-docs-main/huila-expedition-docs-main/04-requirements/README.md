# Functional Requirements

> Functional requirements define what Huila Travel Expedition must do. They are derived from the Huila Travel Expedition SRS and connected to the user stories and responsible services.

---

## Functional Requirements

| ID     | Module                         | Description                                                                                              | Source (HU)     | Priority |
| ------ | ------------------------------ | -------------------------------------------------------------------------------------------------------- | --------------- | -------- |
| RF-001 | Agency Management              | The system must allow travel agencies to register on the platform by providing the required information. | HU-AGENCIA-001  | High     |
| RF-002 | Authentication                 | The system must allow registered users to authenticate securely using their credentials.                 | HU-AUTH-002     | High     |
| RF-003 | Agency Management              | The system must allow agencies to update their contact information and social networks.                  | Not defined yet | High     |
| RF-004 | Tourist Plans                  | The system must allow agencies to create, edit and delete tourist plans.                                 | HU-AGENCIA-003  | High     |
| RF-005 | Tourist Plans / Media          | The system must allow agencies to upload images associated with tourist destinations and plans.          | Not defined yet | High     |
| RF-006 | Tourist Plans                  | The system must allow tourist plans to be classified according to the type of tourism.                   | Not defined yet | Medium   |
| RF-007 | Tourist Search                 | The system must allow tourists to search and filter tourist plans by municipality, price and duration.   | HU-TURISTA-005  | High     |
| RF-008 | Tourist Plans                  | The system must allow agencies to configure differentiated rates for their tourist offers.               | Not defined yet | Medium   |
| RF-009 | Availability                   | The system must allow agencies to manage the availability calendar of their tourist plans.               | HU-AGENCIA-007  | High     |
| RF-010 | Reservations                   | The system must allow tourists to request reservations for available tourist plans.                      | HU-TURISTA-008  | High     |
| RF-011 | Reservations                   | The system must allow agencies to approve or cancel reservation requests.                                | HU-AGENCIA-009  | High     |
| RF-012 | Notifications                  | The system must send confirmation emails for reservation processes.                                      | Not defined yet | Medium   |
| RF-013 | Reservations                   | The system must allow tourists to consult their reservation history.                                     | HU-TURISTA-010  | Medium   |
| RF-014 | Reviews                        | The system must allow tourists to rate and review tourist plans or services.                             | HU-TURISTA-011  | Medium   |
| RF-015 | Administration                 | The system must provide administrators with a dashboard containing platform statistics.                  | HU-ADMIN-012    | Medium   |
| RF-016 | Authentication / Authorization | The system must restrict platform functionality according to the role assigned to each user.             | HU-AUTH-002     | High     |
| RF-017 | Administration                 | The system must allow administrators to manage featured tourist plans.                                   | HU-ADMIN-014    | Low      |
| RF-018 | Administration / Reports       | The system must allow administrators to generate reports in PDF format.                                  | HU-ADMIN-013    | Low      |
| RF-019 | Support                        | The system must provide a contact or support form for users to submit requests or inquiries.             | Not defined yet | Medium   |
| RF-020 | Authentication / Reservations  | The system must allow users to accept the platform's terms and conditions when required.                 | Not defined yet | High     |

---

## Requirements by Module

### Agency Management

| ID     | Requirement                                           |
| ------ | ----------------------------------------------------- |
| RF-001 | Agency registration                                   |
| RF-003 | Update agency contact information and social networks |
| RF-004 | Create, edit and delete tourist plans                 |

### Authentication and Authorization

| ID     | Requirement                     |
| ------ | ------------------------------- |
| RF-002 | Secure user authentication      |
| RF-016 | Role-based access               |
| RF-020 | Terms and conditions acceptance |

### Tourist Plans and Search

| ID     | Requirement                 |
| ------ | --------------------------- |
| RF-005 | Destination and plan images |
| RF-006 | Tourism type classification |
| RF-007 | Search and filtering        |
| RF-008 | Differentiated rates        |

### Availability and Reservations

| ID     | Requirement                          |
| ------ | ------------------------------------ |
| RF-009 | Availability calendar                |
| RF-010 | Reservation requests                 |
| RF-011 | Reservation approval or cancellation |
| RF-013 | Reservation history                  |

### Reviews

| ID     | Requirement         |
| ------ | ------------------- |
| RF-014 | Ratings and reviews |

### Administration and Reports

| ID     | Requirement              |
| ------ | ------------------------ |
| RF-015 | Dashboard and statistics |
| RF-017 | Featured tourist plans   |
| RF-018 | PDF reports              |

### Support and Notifications

| ID     | Requirement                     |
| ------ | ------------------------------- |
| RF-012 | Reservation confirmation emails |
| RF-019 | Support/contact form            |

---

## Functional Requirements by Platform Role

### Administrator

The Administrator must be able to:

* Manage platform statistics.
* Generate PDF reports.
* Manage featured tourist plans.
* Moderate reviews according to the platform rules.
* Verify travel agencies and their required information.
* Access administrative functions according to role permissions.

### Travel Agency Representative

The Travel Agency Representative must be able to:

* Register the agency.
* Manage agency contact information and social networks.
* Create, edit and delete tourist plans.
* Upload destination images.
* Classify tourist plans by tourism type.
* Configure differentiated rates.
* Manage plan availability.
* Manage reservation requests.

### Tourist

The Tourist must be able to:

* Consult published tourist plans.
* Search and filter offers.
* Compare tourist offers.
* View plan information.
* Request reservations.
* Consult reservation history.
* Rate and review tourist services.
* Use the support/contact functionality.

---

## Requirement Rules

1. Every functional requirement has a unique identifier using the `RF-XXX` format.
2. Requirements are derived from the Huila Travel Expedition SRS.
3. Each requirement is associated with a user story when a corresponding HU has already been defined.
4. Requirements without a HU are explicitly marked as **Not defined yet** instead of inventing a user story.
5. Requirements must be testable and verifiable.
6. Functional requirements describe system behavior and not implementation details.
7. Technical implementation decisions such as exact API endpoints or microservice architecture are documented in their corresponding sections.

---

## Traceability

Functional requirements are connected to:

* User Stories → `03-product/user-stories.md`
* Product Backlog → `03-product/product-backlog.md`
* Non-Functional Requirements → `04-requirements/non-functional.md`
* Traceability Matrix → `04-requirements/traceability-matrix.md`
* API Contracts → `07-api/contracts/openapi/`
* Microservices → `09-microservices/services/`
* Testing Strategy → `11-quality/testing-strategy.md`

The traceability matrix must be updated whenever a new HU or test case is created.

---

## Source of Truth

The **Huila Travel Expedition SRS** is the primary source for these functional requirements.

Requirements that do not yet have a corresponding user story are identified as **Not defined yet**. They should be covered by additional user stories before implementation and testing.
