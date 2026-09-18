# Documentation Rules

> These rules define how documentation is written, organized, and maintained for Huila Travel Expedition. Documentation that does not follow these rules may be rejected during code review.

## Fundamental Principle

> **"Documentation is code. If it is not up to date, it is broken."**

Any User Story (US) or requirement that changes the behavior of Huila Travel Expedition MUST include updates to the affected documentation.

Documentation must remain consistent with the Software Requirements Specification (SRS), especially regarding system roles, functional requirements, business rules, and system scope.

## Language Standard

| Artifact                    | Language             |
| :-------------------------- | :------------------- |
| **Source code**             | English              |
| **Code comments**           | English              |
| **Commit messages**         | English              |
| **Branch names**            | English              |
| **Markdown documentation**  | English              |
| **OpenAPI contracts**       | English              |
| **Frontend error messages** | English or localized |
| **Internal system logs**    | English              |

> **Rule:** The same language must be maintained throughout each artifact category. Mixing languages within the same category should be avoided.

## File Structure

Every documentation folder must contain a `README.md` explaining its purpose.

Content documents must use `kebab-case.md`.

Examples:

* `domain-map.md`
* `entities-and-rules.md`
* `product-backlog.md`
* `risk-register.md`

Templates must use the `_template-` prefix.

Examples:

* `_template-us.md`
* `_template-adr.md`

Architectural Decision Records (ADRs) must be numbered sequentially.

Example:

```text
ADR-001-database-selection.md
ADR-002-authentication-strategy.md
```

## Documentation Based on the SRS

The SRS is the main reference for the functional and non-functional documentation of Huila Travel Expedition.

The documentation must preserve the main concepts defined in the SRS:

* **Administrator:** Manages agencies, verifies RNT information, moderates reviews, generates reports, and manages platform configuration.
* **Agency:** Registers and manages tourist services, plans, calendars, inventory, reservations, and its own reports.
* **Tourist:** Searches, compares, and reserves tourist services and can publish reviews.

The SRS defines 20 functional requirements, including agency registration, authentication, tourist plan management, search filters, reservations, reviews, role-based access, reports, and support.

## What MUST Be Documented

| Content                                       | Location                                       |
| :-------------------------------------------- | :--------------------------------------------- |
| **Business rules and domain invariants**      | `02-domain/entities-and-rules.md`              |
| **Bounded contexts and domain relationships** | `02-domain/domain-map.md`                      |
| **Domain events**                             | `02-domain/domain-events.md`                   |
| **User stories and acceptance criteria**      | `04-requirements/user-stories.md`              |
| **Functional requirements**                   | `04-requirements/`                             |
| **Non-functional requirements**               | `04-requirements/`                             |
| **Architectural decisions**                   | `05-architecture/decisions/records/ADR-NNN.md` |
| **Data model changes**                        | `06-data/models.md`                            |
| **API contracts**                             | `07-api/contracts/openapi/`                    |
| **Operational procedures**                    | `13-operations/`                               |
| **Project risks**                             | `15-project-control/risks.md`                  |

## What NOT to Document

The following information should not be documented unnecessarily:

* Details that are already clear from the source code.
* Temporary experiments or decisions that will be removed.
* Internal implementation details of external libraries.
* Manual change logs, because Git history provides this information.
* Information that is not related to the Huila Travel Expedition domain or project requirements.

## Documentation Update Rules

Documentation MUST be updated when a change affects:

1. A system requirement.
2. A User Story.
3. A business rule.
4. A domain entity.
5. A domain event.
6. A user role or permission.
7. The data model.
8. An API contract.
9. An architectural decision.

For example, if a new reservation rule is introduced, the corresponding requirement, User Story, domain rule, and affected data or API documentation must also be reviewed.

## Section Owners

| Section               | Owner                     | Review Frequency            |
| :-------------------- | :------------------------ | :-------------------------- |
| `00-governance/`      | Tech Lead                 | Start of each sprint        |
| `02-domain/`          | Tech Lead + Product Owner | Upon domain changes         |
| `04-requirements/`    | Product Owner             | Every sprint                |
| `05-architecture/`    | Tech Lead                 | Upon each design decision   |
| `06-data/`            | Data/Backend Developer    | Upon data model changes     |
| `07-api/contracts/`   | Service Owner             | Upon each API change        |
| `09-microservices/`   | Service Owner             | Every deployment or release |
| `13-operations/`      | DevOps / On-call          | After operational incidents |
| `15-project-control/` | Tech Lead                 | Weekly review               |

## Document Format and Style

### Headings

* Use only one `# H1` heading per file.
* Use `## H2` for main sections.
* Use `### H3` for subsections.
* Do not use `#### H4` or deeper headings.
* Keep the document structure simple and easy to navigate.

### Tables

Use tables for:

* Comparisons.
* Requirement matrices.
* Logs.
* Structured information.
* Responsibility assignments.

Do not use tables when a simple list is clearer.

### Code Blocks

Always specify the programming language when showing code.

Example:

```typescript
const projectName: string = "Huila Travel Expedition";
```

## Consistency with Huila Travel Expedition

All documentation must use the same domain terminology established in the SRS.

The main roles are:

* **Administrator**
* **Agency**
* **Tourist**

The platform focuses on tourism services and plans offered by local agencies in Huila. The documentation must not introduce unrelated business concepts or requirements that are outside the defined project scope.

The SRS describes Huila Travel Expedition as a web platform intended to centralize and promote tourism offers from local agencies and facilitate search, comparison, and reservation of tourist services.

## Documentation Review

Before creating a Pull Request, the contributor must verify:

* [ ] Documentation is written in English.
* [ ] File names follow `kebab-case`.
* [ ] Headings follow the defined hierarchy.
* [ ] Changes are consistent with the SRS.
* [ ] Affected requirements or User Stories have been updated.
* [ ] Business rules are documented when applicable.
* [ ] No unnecessary or duplicated documentation has been added.
* [ ] Commit messages follow Conventional Commits.

## Governance Principle

Documentation is part of the Huila Travel Expedition project and must evolve together with the software.

Any approved change to the system must be evaluated to determine whether the corresponding documentation also needs to be updated.
