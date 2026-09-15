# Definition of Done (DoD) — Huila Travel Expedition (HTE)

> A User Story or Technical Task is **DONE** when it meets ALL the criteria on this checklist. If even a single criterion is missing, the story is NOT considered complete and reverts to the "In Progress" state.

---

## Mandatory Checklist

### Code & Quality
* [ ] The code implements all acceptance criteria specified in the user story.
* [ ] The code has been reviewed and approved by at least one team member via a GitHub Pull Request.
* [ ] The code follows project standards (passes Linter and Formatting checks in CI/CD).
* [ ] No technical debt is introduced without explicitly logging it in `15-project-control/tech-backlog.md`.

### Testing
* [ ] Unit tests have been written for new or modified business logic.
* [ ] Test coverage maintains or exceeds the project baseline (minimum 70%).
* [ ] All tests pass successfully both locally and in the Continuous Integration (CI) pipeline.
* [ ] Acceptance criteria verified manually or via automated tests.

### Architecture & Contracts
* [ ] Changes do not break integration with other services in the HTE ecosystem.
* [ ] If the API changes: OpenAPI contract updated in `07-api/contracts/`.
* [ ] If the data model changes: service model updated in its respective `data-model.md`.
* [ ] If domain events are added or modified: `02-domain/domain-events.md` file updated.

### Deployment & CI/CD
* [ ] The branch merges without conflicts into the main branch (`main` or `dev`).
* [ ] The CI/CD pipeline successfully completes the build and automated test execution.
* [ ] Successful deployment to the test or staging environment.
* [ ] Basic smoke tests passed in the deployed environment. ### Documentation
* [ ] Service `README.md` file updated if the public interface or configuration has changed.
* [ ] Architectural Decision Record (ADR) created or updated if a significant technical decision was made.

---

## Allowed Exceptions

The following exceptions are valid only with the explicit approval of the Tech Lead or the entire team:

* Omission of End-to-End (E2E) tests due to technical or environment limitations (requires logging the risk in `15-project-control/risks.md`).
* Non-critical technical documentation postponed due to an urgent delivery (requires creating a task in `15-project-control/tech-backlog.md`).

---

## What is NOT a "Done" Criterion

* "The code works on my machine" — It must be integrated into the remote repository.
* "It works locally" — It must work and integrate correctly in the development/staging environment.
* "The Product Owner/Instructor approved it verbally" — It must meet the repository's verifiable technical criteria.
