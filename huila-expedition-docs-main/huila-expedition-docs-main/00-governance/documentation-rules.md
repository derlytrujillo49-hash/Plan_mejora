# Documentation Rules

> These rules determine how documentation is written, organized, and maintained in this project. Documentation that does not follow these rules may be rejected during code review.

---

## Fundamental Principle

> **"Documentation is code. If it is not up to date, it is broken."**

Any User Story (US) that modifies system behavior MUST include updates to the affected documents. The Definition of Done (DoD) requires this.

---

## Language Standard

| Artifact | Language |
| :--- | :--- |
| **Source code** (variables, functions, classes) | English |
| **Code comments** | English |
| **Commit messages** | English (*Conventional Commits*) |
| **Branch names** | English |
| **Markdown documentation** | English |
| **OpenAPI contracts** (descriptions) | English |
| **Error messages returned to the frontend** | English (or localized) |
| **Internal system logs** | English |

> **Rule:** Once a language is chosen for a category, it is binding for the entire project. Mixing languages ​​within the same category is grounds for rejecting the Pull Request (PR).

---

## File Structure

* Every folder in the repository has a `README.md` file explaining its purpose.
* Content documents use `kebab-case.md` formatting (e.g., `domain-map.md`, `risk-register.md`).
* Templates are prefixed with an underscore `_` so they appear first (e.g., `_template-us.md`, `_template-adr.md`).
* Architectural Decision Records (ADRs) are numbered sequentially: `ADR-001-short-title.md`.

---

## What to Document and What NOT to Document

### What MUST be documented

| Content | Location |
| :--- | :--- |
| **Non-obvious architectural decisions** | `05-architecture/decisions/records/ADR-NNN.md` |
| **Business rules and domain invariants** | `02-domain/entities-and-rules.md` |
| **Service API contracts** | `07-api/contracts/openapi/[service].yaml` |
| **Data model changes** | `06-data/models.md` |
| **Operational procedures** | `13-operations/` |
| **Identified risks** | `15-project-control/risks.md` |

### What NOT to document

* What the code itself clearly expresses (avoid redundant comments).
* Temporary decisions or experiments that will be reverted.
* Internal details of external libraries (they have their own official documentation).
* Manual change logs (use `git log` for this).

---

## Section Owners

| Section | Owner | Review Frequency |
| :--- | :--- | :--- |
| `00-governance/` | Tech Lead | Start of each sprint |
| `02-domain/` | Tech Lead + Product Owner | Upon domain changes |
| `04-requirements/` | Product Owner | Every sprint |
| `05-architecture/` | Tech Lead | Upon each design decision |
| `07-api/contracts/` | Service owner (developer) | Upon each API change |
| `09-microservices/` | Service owner (developer) | Every deployment/release |
| `13-operations/` | DevOps / On-call | After resolving each incident |
| `15-project-control/` | Tech Lead | Weekly review | ---

## Document Format and Style

### Headings
* `# H1`: Only one per file (corresponds to the main title).
* `## H2`: Main sections.
* `### H3`: Subsections.
* Do not use `#### H4` or deeper levels. If a more complex hierarchy is required, the document should be simplified or split.

### Tables
Use tables strictly for comparisons, logs, matrices, and structured data. Do not use tables for simple lists.

### Code Blocks
Always specify the language for the corresponding code block:

```typescript
const sampleVariable: string = "HTE Document Rule";
