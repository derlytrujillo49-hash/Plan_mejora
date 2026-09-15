# Definition of Ready (DoR)

> A User Story is **Ready** when the entire team can begin developing it in the upcoming sprint without needing to resolve fundamental questions midway through the cycle. If a story does not meet this DoR, it must return to the refinement phase.

---

## DoR Checklist

Before moving a User Story to the **"Ready for Sprint"** status, verify the following aspects:

### Clarity and Wording
* [ ] The story follows the standard format: **As a [role], I want to [action], so that [benefit]**.
* [ ] The specified role is clear and concrete (e.g., *Travel Agency*, *Registered Tourist*, *Administrator*, rather than a generic "User").
* [ ] The expected benefit is evident, measurable, and adds real value to the product.

### Acceptance Criteria
* [ ] It contains verifiable acceptance criteria, preferably expressed in the **Given / When / Then** format.
* [ ] The criteria cover both the main flow (*happy path*) and key error cases or exceptions.
* [ ] The criteria are testable via automated tests or objective manual tests.
* [ ] Ambiguities are avoided (e.g., instead of "the response must be fast," it specifies "response time must be less than 3 seconds").

### Dependencies
* [ ] All external dependencies (third-party services, APIs, infrastructure) have been identified.
* [ ] Blocking dependencies are resolved, or a viable contingency plan has been defined.
* [ ] If it depends on another User Story, that preceding story is already in the **Done** state or is well underway.

### Estimation
* [ ] The development team has estimated the story using Story Points (SP).
* [ ] There is a consensus that the story can comfortably be completed within a single sprint.
* [ ] If the estimate exceeds a reasonable limit (> 8 SP), the story has been split into smaller stories. ### Technical Preparation
* [ ] Necessary access, credentials, and environments are available to begin development.
* [ ] If new endpoints are involved, API specifications or contracts (OpenAPI/Swagger) are defined.
* [ ] Database or data model changes are documented.
* [ ] Impact on other existing components or modules has been identified and assessed.

### Non-Functional Requirements (NFRs)
* [ ] Applicable performance parameters (e.g., response times, load) are defined.
* [ ] Security criteria (authentication, authorization, input validation and sanitization) are addressed.
* [ ] Traceability and observability requirements (logs, key metrics) are included.

---

## Common Reasons Why a Story is NOT "Ready"

| Identified Issue | Recommended Corrective Action |
| :--- | :--- |
| **Ambiguous or incomplete requirements** | Schedule a short refinement session (30 min) with the Product Owner/Analyst. |
| **Missing acceptance criteria** | The Product Owner must detail the criteria before the Planning session. |
| **Unidentified dependencies** | The Tech Lead evaluates the architecture and documents necessary dependencies. |
| **Story is too large (> 8 SP)** | Apply *Story Splitting* techniques to break it down into smaller deliverables. |
| **No access to environments or APIs** | Request or configure credentials prior to sprint execution. |
| **Undefined API contract** | Agree on the request/response structure (JSON) between frontend and backend before starting. |

---

## Comparison: DoR vs. DoD

| Criterion | Definition of Ready (DoR) | Definition of Done (DoD) |
| :--- | :--- | :--- |
| **When it applies** | **Before** starting the story (Planning / Refinement). | **Upon completion** of the story's implementation. | |
| **Verified by** | The development team together with the Product Owner. | The technical team during the increment review. |
| **Primary purpose** | To ensure the team starts without blockers or ambiguities. | To ensure the deliverable is functional, secure, and deployable. |

---

## Related Documentation

* **Definition of Done (DoD):** `00-governance/definition-of-done.md`
* **User Story Template:** `04-requirements/_template-hu.md`
* **User Story Backlog:** `04-requirements/user-stories.md`
