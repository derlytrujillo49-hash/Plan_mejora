# Git Conventions

> **Read this document before making your first commit on the project.**

## Branch Strategy

```text
main        ← Production. Merge from release only. Always stable.
  └── dev   ← Continuous integration. Merge from features.
        └── feat/[description]    ← One branch per feature or user story
        └── fix/[description]     ← One branch per bug fix
        └── chore/[description]   ← Documentation, infrastructure, or dependency changes
        └── hotfix/[description]  ← Urgent fixes directly to main
```

### Rules

* Nobody commits directly to `main` or `dev`.
* Every task must use one branch and one Pull Request.
* One branch must represent one task.
* Different features or tasks must not be mixed in the same branch.
* Branches must be deleted after the Pull Request is merged.

## Branch Naming Format

```text
[type]/[description-in-kebab-case]
```

### Examples

```text
feat/agency-registration
feat/tourist-plan-management
feat/reservation-management
fix/reservation-status
chore/update-documentation
hotfix/security-validation
```

## Commit Format

Huila Travel Expedition uses **Conventional Commits**.

```text
[type]([scope]): [lowercase description, imperative mood, no trailing period]

[optional body — explain WHY, not what]

[optional footer — issue/user story references]
```

### Commit Types

| Type       | When to use                                            |
| ---------- | ------------------------------------------------------ |
| `feat`     | New functionality                                      |
| `fix`      | Bug fix                                                |
| `docs`     | Documentation only                                     |
| `style`    | Formatting or whitespace changes without logic changes |
| `refactor` | Code refactoring without behavior changes              |
| `test`     | Adding or modifying tests                              |
| `chore`    | Tooling, dependencies, or CI changes                   |
| `perf`     | Performance improvements                               |

### Examples

```text
feat(agency): add agency registration

fix(reservation): correct reservation status validation

docs(domain): update business rules

docs(governance): update git conventions

chore(project): update documentation structure
```

## Pull Request Policy

* **Size:** Maximum 400 lines of code, excluding tests. If larger, split the work.
* **Reviewers:** Minimum 1 approval before merging.
* **Review time:** The reviewer has a maximum of 24 business hours.
* **Template:** Use `.github/pull_request_template.md`.
* **Green CI:** All pipeline checks must pass before merging.

## Merge Policy

* Use **Squash and Merge** for features to keep the `dev` history clean.
* Use **Merge Commit** for releases to `main` to preserve the full history.
* Do not use **Rebase and Merge** because it can create confusion in shared history.

## Tags and Versioning

The project follows **Semantic Versioning (SemVer)**:

```text
MAJOR.MINOR.PATCH
```

Example:

```text
v1.2.0
```

When releasing a production version:

```bash
git tag -a v1.2.0 -m "Release v1.2.0: add reports module"
git push origin v1.2.0
```

## Documentation Commits

Documentation changes must use the `docs` commit type.

Examples:

```bash
git commit -m "docs(domain): update domain map"
```

```bash
git commit -m "docs(requirements): update user stories"
```

```bash
git commit -m "docs(governance): update git conventions"
```

Documentation commits should contain only documentation changes related to the stated purpose of the commit.

