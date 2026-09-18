# Domain Map — Huila Travel Expedition

## Bounded Contexts

### 1. Agency Management

**Responsibility:** Manage the registration, information, and services offered by travel agencies.

**Main entities:** Agency, Tourist Plan, Destination.

**Owning team:** Huila Travel Expedition Development Team.

---

### 2. Tourist Plan Management

**Responsibility:** Manage the creation, editing, deletion, and publication of tourist plans offered by agencies.

**Main entities:** Tourist Plan, Destination, Tourism Type, Rate.

**Owning team:** Huila Travel Expedition Development Team.

---

### 3. Tourist Search and Comparison

**Responsibility:** Allow tourists to search, filter, view, and compare available tourist offers.

**Main entities:** Tourist, Tourist Plan, Destination, Tourism Type.

**Owning team:** Huila Travel Expedition Development Team.

---

### 4. Reservation Management

**Responsibility:** Manage reservation requests made by tourists and the approval or cancellation actions performed by agencies.

**Main entities:** Reservation, Tourist, Tourist Plan, Agency.

**Owning team:** Huila Travel Expedition Development Team.

---

### 5. User and Administration Management

**Responsibility:** Manage users, roles, agency validation, review moderation, and administrative information.

**Main entities:** User, Agency, Tourist, Administrator, Review.

**Owning team:** Huila Travel Expedition Development Team.

---

### 6. Reviews and Ratings

**Responsibility:** Manage ratings and reviews submitted by tourists about tourist services.

**Main entities:** Review, Tourist, Tourist Plan.

**Owning team:** Huila Travel Expedition Development Team.

## Relationship Map

```text
                 ┌───────────────────────┐
                 │   Agency Management   │
                 └───────────┬───────────┘
                             │
                             ▼
                 ┌───────────────────────┐
                 │ Tourist Plan          │
                 │ Management            │
                 └───────┬───────┬───────┘
                         │       │
              ┌──────────▼───┐   ▼
              │ Tourist      │ ┌──────────────────┐
              │ Search and   │ │ Reservation      │
              │ Comparison   │ │ Management       │
              └──────────────┘ └────────┬─────────┘
                                        │
                                        ▼
                              ┌──────────────────┐
                              │ Reviews and      │
                              │ Ratings          │
                              └──────────────────┘

                 ┌────────────────────────────┐
                 │ User and Administration   │
                 │ Management                │
                 └────────────────────────────┘
```

## Relationship Table

| Context A                          | Relationship | Context B                     | Description                                                                                             |
| ---------------------------------- | ------------ | ----------------------------- | ------------------------------------------------------------------------------------------------------- |
| Agency Management                  | upstream     | Tourist Plan Management       | Agencies provide the information required to manage their tourist plans.                                |
| Tourist Plan Management            | upstream     | Tourist Search and Comparison | Published tourist plans provide the information tourists search and compare.                            |
| Tourist Search and Comparison      | upstream     | Reservation Management        | A tourist can request a reservation from a tourist plan.                                                |
| Reservation Management             | upstream     | Reviews and Ratings           | Reservations establish the relationship between the tourist and the service that can later be reviewed. |
| User and Administration Management | shared       | Agency Management             | Administrators participate in agency validation and management.                                         |
| User and Administration Management | shared       | Reviews and Ratings           | Administrators can moderate reviews submitted on the platform.                                          |

## Domain Boundaries

The main domain boundaries are established around agency management, tourist plan management, tourist search and comparison, reservations, users, and reviews.

These boundaries separate business responsibilities and facilitate a possible future organization of the system's components.

# Entities and Business Rules — Huila Travel Expedition

## Entity: Agency

**Belongs to:** Agency Management
**Identifier:** agency_id

### Attributes

| Attribute           | Type    | Description                     | Required | Rules                                    |
| ------------------- | ------- | ------------------------------- | -------- | ---------------------------------------- |
| agency_id           | Integer | Unique agency identifier        | Yes      | Cannot be duplicated                     |
| name                | String  | Agency name                     | Yes      | Must be registered                       |
| RNT                 | String  | National Tourism Registry       | Yes      | Must be validated by the administrator   |
| contact_information | String  | Agency contact information      | Yes      | Must allow communication with the agency |
| social_media        | String  | Agency social media information | No       | Can be updated                           |

### Business Rules (Invariants)

* [ ] An agency must be registered to publish its tourist plans.
* [ ] The RNT must be validated by the administrator.
* [ ] An agency can only manage its own tourist plans.

### Behaviors (Domain Methods)

* `register()`: Registers a new agency.
* `updateInformation()`: Updates agency information.
* `validateRNT()`: Validates the agency's tourism registry.

---

## Entity: TouristPlan

**Belongs to:** Tourist Plan Management
**Identifier:** plan_id

### Attributes

| Attribute    | Type    | Description                               | Required | Rules                                    |
| ------------ | ------- | ----------------------------------------- | -------- | ---------------------------------------- |
| plan_id      | Integer | Unique tourist plan identifier            | Yes      | Cannot be duplicated                     |
| name         | String  | Tourist plan name                         | Yes      | Must be registered                       |
| description  | String  | Description of the plan                   | Yes      | Must provide information about the offer |
| price        | Decimal | Plan price                                | Yes      | Must be a valid value                    |
| duration     | String  | Plan duration                             | Yes      | Must be specified                        |
| destination  | String  | Municipality or tourist destination       | Yes      | Must belong to the Huila tourism scope   |
| tourism_type | String  | Tourism classification                    | Yes      | Must have a category                     |
| images       | String  | Images related to the destination or plan | No       | Must correspond to the plan              |

### Business Rules (Invariants)

* [ ] A tourist plan must belong to a registered agency.
* [ ] The plan must contain basic information before being published.
* [ ] Price and duration must be defined.
* [ ] A deleted plan must not appear as an available offer.

### Behaviors (Domain Methods)

* `create()`: Creates a new tourist plan.
* `edit()`: Modifies the tourist plan information.
* `delete()`: Deletes the tourist plan.
* `publish()`: Makes the tourist plan available for consultation.

---

## Entity: Tourist

**Belongs to:** Tourist Search and Comparison
**Identifier:** user_id

### Attributes

| Attribute | Type    | Description            | Required | Rules                    |
| --------- | ------- | ---------------------- | -------- | ------------------------ |
| user_id   | Integer | Unique user identifier | Yes      | Cannot be duplicated     |
| name      | String  | Tourist name           | Yes      | Must be registered       |
| email     | String  | Email address          | Yes      | Must be valid            |
| password  | String  | Access credential      | Yes      | Must be securely managed |

### Business Rules (Invariants)

* [ ] The tourist must log in to access functions that require authentication.
* [ ] The tourist can consult available offers.
* [ ] The tourist can request reservations for available tourist plans.

### Behaviors (Domain Methods)

* `searchPlans()`: Searches for available tourist offers.
* `filterPlans()`: Applies search filters.
* `requestReservation()`: Creates a reservation request.
* `ratePlan()`: Registers a rating or review.

---

## Entity: Reservation

**Belongs to:** Reservation Management
**Identifier:** reservation_id

### Attributes

| Attribute      | Type    | Description                   | Required | Rules                                |
| -------------- | ------- | ----------------------------- | -------- | ------------------------------------ |
| reservation_id | Integer | Unique reservation identifier | Yes      | Cannot be duplicated                 |
| status         | String  | Reservation status            | Yes      | Must represent a valid status        |
| date           | Date    | Reservation date              | Yes      | Must be registered                   |
| tourist        | Integer | User who makes the request    | Yes      | Must correspond to a tourist         |
| plan           | Integer | Reserved tourist plan         | Yes      | Must correspond to an available plan |
| agency         | Integer | Responsible agency            | Yes      | Must correspond to the plan's agency |

### Business Rules (Invariants)

* [ ] A reservation must be associated with a tourist and a tourist plan.
* [ ] An agency can approve or cancel a reservation request.
* [ ] The reservation status must be consistent with the action performed.

### Behaviors (Domain Methods)

* `createRequest()`: Creates a reservation request.
* `approve()`: Approves a reservation request.
* `cancel()`: Cancels a reservation.
* `checkStatus()`: Allows the user to check the reservation status.

---

## Entity: Review

**Belongs to:** Reviews and Ratings
**Identifier:** review_id

### Attributes

| Attribute | Type    | Description              | Required | Rules                                    |
| --------- | ------- | ------------------------ | -------- | ---------------------------------------- |
| review_id | Integer | Unique review identifier | Yes      | Cannot be duplicated                     |
| rating    | Integer | Rating value             | Yes      | Must be within the defined range         |
| comment   | String  | Tourist's opinion        | No       | Must correspond to the evaluated service |
| tourist   | Integer | Review author            | Yes      | Must correspond to a registered user     |
| plan      | Integer | Evaluated tourist plan   | Yes      | Must correspond to a tourist plan        |

### Business Rules (Invariants)

* [ ] A review must be associated with a tourist.
* [ ] A review must be related to a tourist plan.
* [ ] Reviews can be moderated by the administrator.

### Behaviors (Domain Methods)

* `create()`: Registers a new review.
* `rate()`: Registers the rating.
* `moderate()`: Allows an administrator to manage a review.

# Domain Events — Huila Travel Expedition

Domain events represent important facts that occur within the business domain. They are written in the past tense because they represent actions that have already occurred.

| Event                | Triggered by                          | Data                                         | Consumers                                  | Bounded Context                    |
| -------------------- | ------------------------------------- | -------------------------------------------- | ------------------------------------------ | ---------------------------------- |
| AgencyRegistered     | Registration of a new agency          | agency_id, name, RNT, date                   | User and Administration Management         | Agency Management                  |
| AgencyValidated      | Administrator validates the RNT       | agency_id, RNT, date                         | Agency Management, Tourist Plan Management | User and Administration Management |
| TouristPlanCreated   | Creation of a new tourist plan        | plan_id, agency_id, name, price, destination | Tourist Search and Comparison              | Tourist Plan Management            |
| TouristPlanUpdated   | Modification of a tourist plan        | plan_id, modified fields, date               | Tourist Search and Comparison              | Tourist Plan Management            |
| TouristPlanDeleted   | Deletion of a tourist plan            | plan_id, agency_id, date                     | Tourist Search and Comparison              | Tourist Plan Management            |
| ReservationRequested | Tourist submits a reservation request | reservation_id, tourist_id, plan_id, date    | Reservation Management, Agency Management  | Reservation Management             |
| ReservationApproved  | Agency approves a reservation request | reservation_id, agency_id, date              | Tourist, Reservation Management            | Reservation Management             |
| ReservationCancelled | A reservation is cancelled            | reservation_id, reason, date                 | Tourist, Reservation Management            | Reservation Management             |
| ReviewRegistered     | Tourist submits a review              | review_id, tourist_id, plan_id, rating       | Reviews and Ratings                        | Reviews and Ratings                |
| ReviewModerated      | Administrator moderates a review      | review_id, status, date                      | Reviews and Ratings                        | User and Administration Management |

## Initial Event Storming

As an initial approximation of the domain:

* 🟠 **Events:** AgencyRegistered, AgencyValidated, TouristPlanCreated, ReservationRequested, ReservationApproved, ReservationCancelled, ReviewRegistered.
* 🔵 **Commands:** Register agency, validate agency, create plan, request reservation, approve reservation, cancel reservation, register review.
* 🟡 **Actors:** Administrator, Agency, and Tourist.
* 🟣 **Policies:** Validate agencies before allowing certain operations and moderate reviews.
* 🟦 **External Systems:** Email services for reservation confirmations.

## Domain Rules Summary

The main domain rules are:

1. Agencies must be registered and validated to operate on the platform.
2. Tourist plans must be associated with an agency.
3. Tourists can search and filter available tourist plans.
4. Reservations must relate a tourist to a tourist plan and its corresponding agency.
5. Agencies can approve or cancel reservation requests.
6. Reviews are associated with tourists and tourist plans and can be moderated by the administrator.

