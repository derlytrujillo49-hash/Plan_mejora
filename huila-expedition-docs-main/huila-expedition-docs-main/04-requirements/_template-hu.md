# HU-AGENCIA-001: Register travel agency

> **ID convention:** `HU-[SERVICE_ABBREVIATION]-[NNN]`

---

## Story

**As** a travel agency representative
**I want** to register my travel agency on Huila Travel Expedition
**So that** I can access the platform and manage my tourist plans and services.

---

## Acceptance criteria

* [ ] **AC1:** Given that the agency is not registered, when the representative enters the required registration information, then the system validates the information and creates the agency registration.

* [ ] **AC2:** Given that all required information is valid, when the representative submits the registration form, then the system stores the agency information securely and confirms the registration.

* [ ] **AC3:** Given that required information is missing or invalid, when the representative submits the registration form, then the system displays the corresponding validation errors and does not complete the registration.

* [ ] **AC4:** Given that the agency is already registered, when the representative attempts to register the same agency again, then the system rejects the duplicate registration.

* [ ] **AC5:** Given that the agency requires administrative verification, when the registration is completed, then the agency remains pending verification until the Administrator validates the required information, including the RNT when applicable.

---

## Technical notes

The registration process must validate the information entered by the agency representative and protect the stored data according to the security requirements defined in the SRS.

**Responsible service(s):** Agency / Authentication

**Endpoint(s) implemented:** `POST /api/agencies/register` *(proposed endpoint; the SRS does not define the final API route)*

**Events generated:** `AgencyRegistered` *(proposed event; the SRS does not define a final event architecture)*

**Required permissions:** Public registration. Administrative verification is required after registration.

---

## Definition of Done (DoD)

> This HU can only be closed when it meets the team's full DoD.
> See: [`00-governance/definition-of-done.md`](../../00-governance/definition-of-done.md)

**Additional checks specific to this HU:**

* [ ] Registration form validation has been tested.
* [ ] Invalid or incomplete information has been tested.
* [ ] Duplicate agency registration has been tested.
* [ ] Agency information is stored correctly.
* [ ] Passwords are not stored in plain text.
* [ ] The implementation is consistent with RF1 and the security requirements of the SRS.

---

## Estimation and priority

| Field         | Value    |
| ------------- | -------- |
| Story Points  | 5        |
| Priority      | High     |
| Target sprint | Sprint 1 |
| Dependencies  | None     |
