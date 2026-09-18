# Security Policy

> Security is not a feature — it is a system property built from day one. This document defines the mandatory security practices for Huila Travel Expedition.
>
> Any deviation from these practices must be explicitly approved by the Tech Lead or the person responsible for technical decisions.

---

## Security Principles

1. **Defense in Depth:** Multiple security layers are used so that if one control fails, others can help contain the impact.
2. **Least Privilege:** Each user, role, and component must have only the permissions required to perform its responsibilities.
3. **Fail Secure:** When an error or security validation failure occurs, access must be denied by default.
4. **Security by Design:** Security controls must be considered during requirements, design, development, testing, and deployment.
5. **Zero Trust:** Every request must be validated according to its identity, role, and permissions, even inside the internal network.

---

## Authentication

### JWT (JSON Web Tokens)

Huila Travel Expedition uses secure authentication mechanisms for users accessing protected functionality.

| Property                 | Required value                                           |
| ------------------------ | -------------------------------------------------------- |
| Signing algorithm        | RS256 or HS256 with a strong secret of at least 256 bits |
| Access token expiration  | 1 hour (`exp`)                                           |
| Refresh token expiration | 7 days                                                   |
| Required claims          | `sub` (userId), `iat`, `exp`, `jti`                      |
| Client storage           | `httpOnly cookie` for web clients                        |

**Prohibited in the JWT payload:**

* Passwords.
* Password hashes.
* Payment or card information.
* Complete personal information that is not required for authorization.
* Sensitive authentication data.

### Refresh Token

Refresh tokens must:

* Be stored securely.
* Be stored in the database using a secure hash.
* Be rotated when used.
* Be invalidated when the user logs out.
* Be invalidated after a password change.
* Be revoked if suspicious or unauthorized reuse is detected.

---

## Authorization

### RBAC (Role-Based Access Control)

Huila Travel Expedition defines three main application roles according to the SRS:

| Role            | Description            | Main permissions                                                                       |
| --------------- | ---------------------- | -------------------------------------------------------------------------------------- |
| `ADMINISTRATOR` | Platform administrator | Verify agencies/RNT, moderate reviews, generate reports, manage platform configuration |
| `AGENCY`        | Travel agency user     | Manage agency information, tourist plans, calendars, inventory and reservations        |
| `TOURIST`       | Tourist or traveler    | Search and compare plans, request reservations, consult history and publish reviews    |

### Permission Model

Permissions must follow the structure:

```text
[resource]:[action]
```

Examples:

```text
agencies:create
agencies:read
agencies:update
agencies:verify

plans:create
plans:read
plans:update
plans:delete

reservations:create
reservations:read
reservations:update
reservations:cancel

reviews:create
reviews:read
reviews:moderate

reports:read
reports:export
```

### Authorization Validation

* The API Gateway or authentication layer validates the JWT signature and expiration.
* Each service validates the user's role and permissions for the requested operation.
* Roles may be included in the JWT as a claim such as:

```json
{
  "roles": ["AGENCY"]
}
```

* A user must not access resources belonging to another agency unless the operation is explicitly authorized.
* Administrative operations must be restricted to `ADMINISTRATOR`.

---

## Secure Communication

### Transmission

* **HTTPS is mandatory** in staging and production environments.
* TLS 1.2 is the minimum supported version.
* TLS 1.3 is recommended.
* Valid digital certificates must be used.
* HSTS must be enabled in production.

### Internal Service-to-Service Communication

When microservices communicate internally:

* Authentication between services must be implemented.
* Sensitive information must not be transmitted without encryption.
* mTLS may be used for service-to-service communication in production when the infrastructure supports it.
* Internal API keys or bearer tokens must be protected and rotated.

---

## Secret Management

Secrets must never be included directly in source code.

```text
✗ NEVER in source code
✗ NEVER in committed .env files
✗ NEVER in GitHub repositories
✗ NEVER in application logs
✗ NEVER in client error messages

✓ Environment variables
✓ Secure secret management systems
✓ Protected deployment configuration
✓ Kubernetes Secrets when applicable
```

### Secret Rotation

The team must establish a rotation process for:

* API keys.
* Database credentials.
* JWT signing secrets or keys.
* TLS certificates.
* External service credentials.

If a secret is suspected to be compromised, it must be changed immediately.

---

## Input Validation and Sanitization

### General Rules

1. **Never trust user input.** All data received from users must be validated.
2. **Validate at the application boundary.** Controllers or API endpoints must validate input before processing.
3. **Whitelist allowed values.** Only valid values and formats should be accepted.
4. **Reject invalid requests early.** Invalid data should return an appropriate `400 Bad Request` response.
5. **Sanitize data before displaying user-generated content.**

### SQL Injection Prevention

```typescript
// ✗ VULNERABLE
const result = await db.query(
  `SELECT * FROM agencies WHERE name = '${userInput}'`
);

// ✓ SAFE — use prepared parameters
const result = await db.query(
  'SELECT * FROM agencies WHERE name = $1',
  [userInput]
);
```

### XSS Prevention

```typescript
// ✗ VULNERABLE
element.innerHTML = userProvidedContent;

// ✓ SAFE
element.textContent = userProvidedContent;
```

If HTML content must be accepted, it must be sanitized using an approved security library.

### Validation Examples

The following data must be validated before processing:

* Agency information.
* RNT information.
* Tourist plan data.
* Prices and rates.
* Dates and availability.
* Reservation information.
* Reviews and comments.
* Contact forms.
* User credentials.

---

## Password Security

Passwords must:

* Never be stored in plain text.
* Be stored using a strong password hashing algorithm such as bcrypt or Argon2.
* Never appear in logs.
* Never be returned through API responses.
* Be transmitted only through HTTPS.
* Be protected against brute-force login attempts.

---

## OWASP Top 10 — Review Checklist

| Vulnerability                    | Implemented control                                |
| -------------------------------- | -------------------------------------------------- |
| A01: Broken Access Control       | RBAC and permission validation                     |
| A02: Cryptographic Failures      | HTTPS/TLS and secure password hashing              |
| A03: Injection                   | Prepared SQL parameters and input validation       |
| A04: Insecure Design             | Security considered during requirements and design |
| A05: Security Misconfiguration   | Secure configuration and environment variables     |
| A06: Vulnerable Components       | Dependency updates and vulnerability reviews       |
| A07: Authentication Failures     | JWT, password protection and brute-force controls  |
| A08: Software Integrity Failures | Dependency verification and controlled deployments |
| A09: Logging Failures            | Security events and controlled logging             |
| A10: SSRF                        | Validation and restriction of external URLs        |

---

## Audit and Security Logs

Security-relevant actions must be recorded for auditing and incident investigation.

### Events that should be recorded

```typescript
const SECURITY_EVENTS = [
  'auth.login.success',
  'auth.login.failure',
  'auth.password.changed',
  'auth.token.revoked',
  'auth.unauthorized_access_attempt',
  'agency.rnt.verification',
  'admin.role.changed',
  'reservation.unauthorized_access',
  'review.moderated'
];
```

### Required Fields

Security logs should contain:

* `userId` or `ANONYMOUS`.
* `sourceIp`, when available and appropriate.
* `action`.
* `resource`.
* `result` (`SUCCESS` / `FAILURE`).
* `timestamp`.

Logs must not contain:

* Passwords.
* JWT tokens.
* Card information.
* Unnecessary sensitive personal information.

---

## Vulnerability Process

### What to Do if a Vulnerability Is Found

1. Do not publish the vulnerability in the public repository.
2. Notify the Tech Lead or responsible technical person through a private channel.
3. Document the vulnerability in a restricted issue when possible.
4. Evaluate its severity and potential impact.
5. Define and implement the remediation.
6. Verify that the vulnerability has been resolved.
7. Document the solution when necessary.

### Remediation Priority

| Severity | Target remediation                    |
| -------- | ------------------------------------- |
| Critical | Immediate attention                   |
| High     | Current sprint or as soon as possible |
| Medium   | Planned in the next available sprint  |
| Low      | Planned during a security review      |

> Exact remediation times may be established by the team according to project maturity and available infrastructure.

---

## Data Protection

Huila Travel Expedition handles user and agency information that must be protected against unauthorized access or modification.

Security controls must be applied to:

* User accounts.
* Agency information.
* RNT information.
* Tourist plans.
* Reservations.
* Reviews and ratings.
* Contact information.
* Authentication data.

Access to personal information must follow the user's role and the minimum permissions required.

---

## Reservation Security

Because the SRS requires reservation management and inventory control, the system must protect against:

* Unauthorized reservation access.
* Duplicate reservations.
* Inventory inconsistencies.
* Concurrent reservation conflicts.
* Modification of reservations by unauthorized users.

Reservation operations must verify:

```text
Authenticated user
        ↓
User role
        ↓
Resource ownership or permission
        ↓
Availability
        ↓
Operation
```

Database transactions and concurrency controls must be used where necessary to reduce overbooking and inconsistent reservation data.

---

## External Services

Huila Travel Expedition may interact with external services for technical or future functionality.

External integrations must:

* Use secure HTTPS communication.
* Store credentials securely.
* Validate external responses.
* Avoid exposing secrets to clients.
* Apply timeouts and appropriate error handling.
* Log relevant security events without exposing sensitive data.

> Online payment management is outside the initial project scope according to the SRS. Therefore, payment credentials or payment processing must not be implemented as part of the current system unless the project scope is formally updated.

---

## Security Testing

Security must be considered during development and testing.

The team should verify:

* Authentication.
* Authorization and RBAC.
* Input validation.
* SQL injection prevention.
* XSS prevention.
* Password protection.
* Session/token expiration.
* Unauthorized access attempts.
* Reservation ownership and permissions.
* Dependency vulnerabilities.
* Secure configuration.

Security-related defects must be corrected according to their severity before production deployment.

---

## Correlations

* Security non-functional requirements → `04-requirements/non-functional.md`
* Authentication decisions → `05-architecture/decisions/`
* Data models → `06-data/models.md`
* API contracts → `07-api/contracts/openapi/`
* Authentication and authorization services → `09-microservices/services/`
* Security event observability → `13-operations/observability.md`
* General documentation rules → `00-governance/documentation-rules.md`

---

## Security Compliance Checklist

Before merging security-sensitive changes, verify:

* [ ] Authentication is required for protected resources.
* [ ] RBAC is implemented according to the SRS roles.
* [ ] Users cannot access unauthorized resources.
* [ ] Passwords are securely hashed.
* [ ] JWTs do not contain passwords or unnecessary sensitive information.
* [ ] HTTPS is used in non-local environments.
* [ ] Secrets are not committed to the repository.
* [ ] User input is validated.
* [ ] SQL injection protections are implemented.
* [ ] XSS protections are implemented.
* [ ] Security events are logged without exposing sensitive information.
* [ ] Dependencies are reviewed for known vulnerabilities.
* [ ] Reservation operations protect against unauthorized access and conflicts.
* [ ] External integrations use secure communication.
* [ ] Online payment functionality is not introduced into the initial scope.
* [ ] Security changes are consistent with the SRS.
* [ ] Documentation is written in English.
* [ ] Changes follow the project's Git and Pull Request conventions.

---

## Source of Truth

The **Huila Travel Expedition SRS** is the primary source for defining the system's roles, functional requirements, scope restrictions, and security-related technical requirements.

This security policy defines mandatory development and operational practices but must not contradict the approved SRS.

When a new security requirement or technology is introduced, the team must review its impact on the architecture, requirements, and documentation before implementation.
