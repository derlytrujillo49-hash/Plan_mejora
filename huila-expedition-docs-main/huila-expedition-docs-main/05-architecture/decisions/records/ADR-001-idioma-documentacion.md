# ADR-001 — Documentation Language

| Field         | Value                        |
| ------------- | ---------------------------- |
| **ID**        | ADR-001                      |
| **Date**      | 2026-09-17                   |
| **Status**    | Proposed                     |
| **Authors**   | Huila Travel Expedition Team |
| **Reviewers** | Development team             |

---

## Context

Huila Travel Expedition is documented using Markdown files, technical documentation, requirements, architecture documents, Git conventions, API contracts and Architecture Decision Records (ADRs).

The project needs a consistent language for technical documentation and source code to avoid mixing terminology between different artifacts.

The team also works with technologies such as Laravel, PHP, MySQL, Redis, Git and other tools whose official documentation and technical terminology are mainly available in English.

A clear language convention helps maintain consistency between documentation, code, database names, Git branches, commits and technical concepts.

---

## Evaluated alternatives

### Alternative A — Everything in English (PROPOSED)

* **Pros:** Consistent with the terminology used by Laravel, PHP, Git and other technical tools.
* **Pros:** Makes technical documentation easier to compare with official documentation and external resources.
* **Pros:** Keeps code, database names and technical documentation consistent.
* **Pros:** Makes future collaboration with developers familiar with English technical terminology easier.
* **Cons:** May require additional effort from team members who are more comfortable with Spanish.

### Alternative B — Everything in Spanish

* **Pros:** Easier for the current Spanish-speaking team to understand.
* **Pros:** Business concepts can be documented naturally for the local project context.
* **Cons:** Technical terminology may differ from the terminology used by frameworks and libraries.
* **Cons:** Code and technical documentation could become less consistent with external technical resources.

### Alternative C — Split by artifact or layer

* **Pros:** Spanish could be used for business documentation and English for technical documentation or code.
* **Cons:** The same concept could have two different names.
* **Cons:** It can create inconsistencies between requirements, documentation and implementation.
* **Cons:** New team members would need to learn which language applies to each artifact.

---

## Decision

**Alternative A — Use English as the standard language for technical documentation and code.**

This decision is proposed for the Huila Travel Expedition project and must be reviewed and accepted by the team before becoming a formally adopted convention.

| Artifact                         | Language               | Reason                                         |
| -------------------------------- | ---------------------- | ---------------------------------------------- |
| Variables, functions and classes | English                | Consistency with programming terminology       |
| Database tables and columns      | English                | Consistency with application code              |
| Git commits                      | English                | Consistency with repository conventions        |
| Git branch names                 | English                | Consistent technical terminology               |
| Markdown technical documentation | English                | Consistent project documentation               |
| OpenAPI contracts                | English                | Standardized technical terminology             |
| ADRs                             | English                | Consistency with architecture documentation    |
| Technical logs                   | English                | Easier identification of technical events      |
| Business terminology             | English canonical term | Maintains consistency with the technical model |
| User-facing messages             | Spanish or localized   | Can be adapted to the final platform users     |

---

## Consequences

### Positive

* A consistent language is established for technical artifacts.
* Code and documentation can use the same canonical technical terminology.
* The team can consult external technical documentation without translating every concept.
* GitHub documentation remains consistent.
* Future contributors can identify the language convention from the beginning.

### Negative

* Team members with limited English proficiency may require additional effort.
* Some Spanish business concepts may require careful translation.
* The team must agree on the canonical English terminology for domain concepts.

### Mitigation

The team will maintain a glossary with the canonical terminology used by the project:

`01-context/glossary.md`

When a business term has more than one possible English translation, the team should define the selected term in the glossary before using it in the technical documentation or code.

---

## References

* Team documentation conventions → `00-governance/documentation-rules.md`
* Git conventions → `00-governance/git-conventions.md`
* Domain term glossary → `01-context/glossary.md`
* Architecture overview → `05-architecture/overview.md`
* Pattern guide → `05-architecture/pattern-guide.md`

---

## Status

**Proposed**

This ADR is proposed for team review. It should be changed to **Accepted** only after the Huila Travel Expedition team formally validates the documentation-language convention.
