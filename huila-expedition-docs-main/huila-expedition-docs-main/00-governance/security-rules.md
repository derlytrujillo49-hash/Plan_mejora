# Technical Security Rules

> Mandatory technical controls that apply to the code and technical components of Huila Travel Expedition.
> These rules complement the `security-policy.md` document with concrete implementation practices and are based on the project's SRS.

---

## Security Requirements from the SRS

Security is an essential technical requirement of Huila Travel Expedition.

The SRS establishes:

* **RT03 — Security of information:** protection of user and transaction data through encryption, robust authentication, audit logging, and access controls.
* **RF2 — Secure authentication:** users must authenticate securely to access the corresponding functionality.
* **RF16 — Role-based access control:** the system must restrict functionality according to the defined roles.
* **RF20 — Terms and conditions:** the system must record the user's acceptance before confirming a reservation.
* **RNF7 — HTTPS:** communication between client and server must be encrypted using HTTPS.
* **RNF8 — Password security:** passwords must not be stored in plain text and must use Laravel's native hashing mechanism.
* **RNF9 — Personal data protection:** personal data must be handled according to Law 1581 of 2012.
* **Database security:** user and reservation information must be protected and access must be controlled.
* **Data integrity:** transactions and locking mechanisms must prevent overbooking during concurrent reservations.
* **Backup and recovery:** periodic backups must be performed and restoration must be possible in less than 4 hours.

---

## A01 — Broken Access Control

Huila Travel Expedition has three main roles:

```text
ADMINISTRATOR
AGENCY
TOURIST
```

Each role must only access the functions corresponding to its responsibilities.

### Example

```php
// The authenticated user must be used
// instead of trusting an ID sent by the client.

$userId = auth()->id();
```

**Rules:**

* Every protected endpoint MUST require authentication.
* The system MUST verify the user's role before allowing restricted operations.
* An administrator can access administrative functions.
* An agency can manage its own information, plans, calendars, inventory and reservations.
* A tourist can manage the functions corresponding to the tourist role.
* An agency MUST NOT modify another agency's plans or reservations.
* A tourist MUST NOT access the administration or agency management panels.
* Authorization must be validated on the server, not only in the frontend.
* Access to reservations and personal information must be restricted to authorized users.

These controls implement **RF16 — Control of access according to defined roles**.

---

## A02 — Cryptographic Failures

The SRS establishes security of information through encryption and secure authentication.

### Passwords

Passwords MUST:

* Never be stored in plain text.
* Be protected using Laravel's native password hashing mechanism.
* Never appear in logs.
* Never be returned in API responses.
* Only be transmitted through HTTPS.

Example:

```php
use Illuminate\Support\Facades\Hash;

$hashedPassword = Hash::make($password);
```

### HTTPS

According to **RNF7**, all communication between the client and server must use HTTPS with a valid SSL certificate.

The system must:

* Use HTTPS in deployed environments.
* Redirect HTTP requests to HTTPS.
* Maintain a valid SSL certificate.
* Protect credentials and personal information during transmission.

### Sensitive Information

The system must not expose:

* Passwords.
* Authentication credentials.
* Unnecessary personal information.
* Sensitive reservation information to unauthorized users.

## The SRS references **Law 1581 of 2012**, **Law 1480 of 2011**, and PCI-DSS as part of the project's legal and security considerations.

## A03 — Injection

All information received from users must be validated before being processed.

This includes:

* Agency registration information.
* User credentials.
* Tourist plans.
* Prices.
* Dates.
* Reservation information.
* Reviews.
* Contact forms.
* RNT information.

### SQL

```php
// ❌ BAD
$query = "SELECT * FROM agencies WHERE name = '$name'";
```

```php
// ✅ GOOD
$agency = DB::table('agencies')
    ->where('name', $name)
    ->first();
```

**Rules:**

* Do not concatenate untrusted user input directly into SQL queries.
* Use Laravel's query builder, Eloquent ORM, or parameterized queries.
* Validate data before storing it in the database.
* Validate numeric fields such as prices and inventory.
* Validate dates before processing reservations.

---

## A04 — Insecure Design

Security must be considered when designing each functionality.

Particular attention must be given to:

### User information

Personal information must only be accessible according to the user's role and permissions.

### Agency information

Agency data must be protected and the agency must only modify its own information.

### RNT verification

The Administrator is responsible for verifying the agency's RNT before the agency is fully enabled on the platform.

### Reservations

Reservation operations must verify:

```text
Authenticated user
        ↓
Role and permissions
        ↓
Reservation or resource ownership
        ↓
Availability
        ↓
Operation
```

The system must protect against duplicate reservations and overbooking.

The SRS identifies inventory synchronization and concurrent reservations as important risks.

---

## A05 — Security Misconfiguration

The system must use secure configurations in each environment.

### Verification checklist

```text
□ Production errors do not expose technical details
□ Passwords are not included in configuration files
□ Database credentials are protected
□ HTTPS is configured in deployed environments
□ SSL certificate is valid
□ Development credentials are not used in production
□ Unnecessary services and ports are not exposed
□ Security updates are applied periodically
```

The SRS establishes **RT04 — Technological updating**, requiring periodic updates to maintain security patches and adapt to technological changes.

---

## A06 — Vulnerable Components

Project dependencies must be reviewed and updated periodically.

### Rules

* Security updates must be applied regularly.
* Laravel and PHP versions must remain supported.
* Third-party libraries must be reviewed before being incorporated.
* Vulnerable dependencies must be updated or replaced.
* Dependencies must not be included without a valid project need.
* Security patches must be prioritized according to their impact.

These practices support **RT04 — Technological updating** from the SRS.

---

## A07 — Identification and Authentication Failures

Authentication is an essential requirement of Huila Travel Expedition.

### RF2 — Secure Login

The system must:

* Require valid credentials.
* Validate email and password.
* Protect passwords using Laravel's native hashing.
* Restrict access after repeated failed authentication attempts.
* Expire inactive sessions according to the authentication implementation.
* Prevent unauthorized users from accessing protected areas.

The SRS specifically establishes that after **5 failed login attempts**, access is blocked for **15 minutes**, and the session expires after **30 minutes of inactivity**.

### Example

```text
Login attempt
      ↓
Validate credentials
      ↓
Valid?
 ┌────┴────┐
YES        NO
 ↓          ↓
Access     Count attempt
           ↓
      5 failed attempts?
           ↓
       Block 15 min
```

---

## A08 — Software and Data Integrity Failures

The integrity of system data is especially important for reservations, calendars and inventory.

The SRS requires mechanisms such as **ACID transactions and locking** to prevent overbooking during concurrent reservations.

### Rules

* Database transactions must be used for critical reservation operations.
* Concurrent reservation requests must be controlled.
* Inventory must be checked before confirming a reservation.
* Reservation status changes must be validated.
* Data relationships must maintain referential integrity.
* Backups must be performed periodically.
* Data restoration must be tested.

### Reservation flow

```text
Check availability
       ↓
Start transaction
       ↓
Validate inventory
       ↓
Create/update reservation
       ↓
Update inventory
       ↓
Commit transaction
```

If an error occurs:

```text
Error
 ↓
Rollback
 ↓
No inconsistent reservation
```

---

## A09 — Security Logging and Monitoring Failures

The SRS establishes **audit logging** as part of information security.

Security-relevant operations should be recorded, especially:

```text
auth.login.success
auth.login.failure
auth.password.changed
auth.unauthorized_access
agency.rnt.verification
reservation.created
reservation.approved
reservation.cancelled
review.moderated
admin.configuration.changed
```

### Minimum information

Security logs should record:

* User identifier when available.
* Action performed.
* Resource affected.
* Result.
* Date and time.

Logs MUST NOT contain:

* Passwords.
* Authentication credentials.
* Sensitive payment information.
* Unnecessary personal data.

The objective is to provide traceability without exposing confidential information.

---

## A10 — Server-Side Request Forgery (SSRF)

Huila Travel Expedition may use external services or APIs according to the project dependencies.

When the system receives an external URL from a user, the application must validate it before making a server-side request.

### Rules

* Validate external URLs.
* Only access trusted external services.
* Do not allow arbitrary server-side requests.
* Do not expose internal server resources through user-provided URLs.
* Use secure HTTPS connections for external services.

The SRS mentions possible external dependencies such as payment gateways and the WhatsApp API, although online payment management is outside the initial project scope.

---

## User Input Handling

All external input must be validated before reaching the business logic.

Examples include:

```text
HTTP body
Query parameters
Path parameters
Forms
Reservation data
Agency data
Tourist plan data
Reviews
Contact messages
```

### Example with Laravel validation

```php
$request->validate([
    'name' => 'required|string|max:100',
    'email' => 'required|email|max:255',
    'phone' => 'required|string|max:20',
]);
```

### Rule

```text
External Input
      ↓
Validation
      ↓
Sanitization
      ↓
Business Logic
      ↓
Database
```

Invalid information must be rejected before being processed.

This is consistent with the SRS, which requires real-time validation for registration and reservation forms.

---

## Secure Error Handling

The application must not expose internal technical information to users.

### ❌ Incorrect

```php
return response()->json([
    'error' => $exception->getMessage(),
    'stack' => $exception->getTraceAsString()
], 500);
```

### ✅ Correct

```php
return response()->json([
    'error' => 'INTERNAL_SERVER_ERROR',
    'message' => 'Ocurrió un error interno en el sistema.'
], 500);
```

Detailed technical information may be recorded internally for troubleshooting, but it must not be displayed to the user.

---

## Personal Data Protection

The SRS establishes compliance with **Law 1581 of 2012** for the protection of personal data.

The system must:

* Inform users about the treatment of their personal information.
* Obtain explicit acceptance where required.
* Record acceptance of terms and conditions.
* Restrict access to personal data.
* Avoid sharing personal data with unauthorized third parties.
* Protect user and agency information.

### RF20

Before confirming a reservation:

```text
User
 ↓
Terms and Conditions
 ↓
Explicit acceptance
 ↓
Date and time recorded
 ↓
Reservation confirmation
```

The SRS specifically requires the acceptance to be recorded with the date and time.

---

## Database Security

The SRS requires a robust database structure that protects the confidentiality and integrity of:

* Users.
* Agencies.
* Tourist plans.
* Calendars.
* Reservations.
* Payments/dependencies.
* Reviews.

The database must maintain:

* Referential integrity.
* Secure access.
* Transactional integrity.
* Protection against concurrent reservation conflicts.
* Periodic backups.
* Recovery procedures.

The SRS proposes **MySQL 8.0** as the main database technology, with PostgreSQL as an alternative and Redis for frequent searches.

---

## Backup and Recovery

The SRS establishes:

* Weekly database and system-file backups.
* Backup storage separate from the main server.
* Restoration in less than 4 hours.
* Notification of backup success or failure.

### Recovery flow

```text
System failure
      ↓
Identify latest valid backup
      ↓
Restore backup
      ↓
Verify database integrity
      ↓
Verify application
      ↓
Restore service
```

The recovery objective established by the SRS is **less than 4 hours**.

---

## Security Testing

Before deployment, the team must verify:

* [ ] Secure authentication works correctly.
* [ ] The three roles have differentiated access.
* [ ] Unauthorized users cannot access protected resources.
* [ ] Passwords are not stored in plain text.
* [ ] HTTPS works correctly in deployed environments.
* [ ] Forms validate user input.
* [ ] SQL injection protections are applied.
* [ ] Personal data is protected.
* [ ] Reservation concurrency is controlled.
* [ ] Database backups are working.
* [ ] Data can be restored in less than 4 hours.
* [ ] Security logs do not expose sensitive information.
* [ ] Dependencies are updated and reviewed.

---

## Correlations

* Security policy → `00-governance/security-policy.md`
* Security requirements → `04-requirements/non-functional.md`
* Authentication and authorization → `07-api/authentication.md`
* Architecture security decisions → `05-architecture/`
* Database models → `06-data/models.md`
* Microservices → `09-microservices/services/`
* Observability and security logs → `13-operations/observability.md`

---

## Technical Security Compliance Checklist

Before merging a security-sensitive change:

* [ ] The change is consistent with the SRS.
* [ ] Authentication is applied to protected functionality.
* [ ] RBAC is respected for Administrator, Agency and Tourist.
* [ ] Users can only access authorized information.
* [ ] Passwords are securely hashed.
* [ ] HTTPS is used in deployed environments.
* [ ] User input is validated.
* [ ] SQL queries do not directly concatenate untrusted input.
* [ ] Personal information is protected.
* [ ] RF20 terms acceptance is correctly recorded.
* [ ] Reservation concurrency is controlled.
* [ ] Database integrity is maintained.
* [ ] Backups are executed according to the project requirements.
* [ ] Recovery procedures meet the less-than-4-hour objective.
* [ ] Security events can be audited.
* [ ] Sensitive information is not exposed in logs or errors.
* [ ] Dependencies are periodically updated.
* [ ] No functionality outside the approved SRS scope has been introduced.

---

## Source of Truth

The **Huila Travel Expedition SRS** is the primary source for the project's security requirements.

This document translates the security requirements established in the SRS into technical implementation rules.

If a technical security decision is not explicitly defined in the SRS, it must be reviewed by the technical team before being established as a mandatory project rule.

The following SRS requirements are particularly relevant:

```text
RT03 — Security of information
RF2  — Secure authentication
RF16 — Role-based access control
RF20 — Terms and conditions
RNF7 — HTTPS
RNF8 — Password security
RNF9 — Personal data protection
RNF11 — Backup and recovery
```
