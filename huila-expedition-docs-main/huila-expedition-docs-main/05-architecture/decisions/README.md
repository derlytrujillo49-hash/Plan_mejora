# ADRs — Architecture Decision Records

ADRs document important architectural decisions. Each file = one decision.

## How to create an ADR

1. Copy `_template-adr.md`
2. Name it `ADR-NNN-short-title.md` (e.g.: `ADR-001-message-broker.md`)
3. Fill it in completely — especially the evaluated alternatives
4. Once accepted, the status is **permanent** (it is not deleted, it is "Superseded" by another ADR)

## Possible statuses

* `Proposed` — under discussion
* `Accepted` — approved by the team
* `Rejected` — evaluated and discarded (document why)
* `Superseded` — superseded by ADR-NNN (indicate which one)

## ADR register

| #       | Title                                                           | Status   | Date       |
| ------- | --------------------------------------------------------------- | -------- | ---------- |
| ADR-001 | Architectural Style — Modular Monolith + Hexagonal Architecture | Proposed | 2026-09-17 |

### ADR-001

**File:** `ADR-001-architectural-style.md`

**Decision:** Use **Modular Monolith + Hexagonal Architecture** as the proposed initial architectural style for Huila Travel Expedition.

**Reason:** The SRS defines a web platform with several functional areas but does not require independent microservices, an API Gateway, a message broker or database-per-service. The modular monolith provides logical separation without introducing unnecessary distributed complexity.

**Status:** Proposed — pending team approval.
