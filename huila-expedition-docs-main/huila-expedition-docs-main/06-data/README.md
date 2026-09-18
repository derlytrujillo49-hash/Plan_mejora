# 06 — Data — Huila Travel Expedition

This module centralizes the architecture, persistence policies, data dictionary, and schema migration strategies for the *Huila Travel Expedition* system, ensuring optimal performance on shared hosting or VPS environments (RNF3, RNF12).

---

## Fundamental principle in our system

> *Modular Data Isolation with Unified Consistency.*

Although the system initially operates under a monolithic core in *Laravel 10+, the data is strictly segmented by modular responsibilities in **MySQL 8.0* across our bounded contexts (AgencyManagement, TourPlanManagement, ReservationBooking). No module interferes with another's business rules, ensuring that any future transition toward microservices or external synchronizations (Channel Manager) remains direct and requires no rewriting of the core codebase (RNF12).

---

## What is here and how to fill it in

### models.md ⭐
Detailed relational data models broken down by technical service module.
*Content:* Explicit table schemas in MySQL 8.0, foreign keys (FK), soft delete controls (deleted_at), and performance indexes designed for fast queries under 3 seconds (RNF1).

*Mapped Modules:*
- *Agency Module* (administradores, agencias)
- *Plans and Destinations Module* (paquetes_turisticos, destinos, hoteles, transportes, guias)
- *Reservations and Feedback Module* (turistas, reservas, pagos)

### data-dictionary.md ⭐
Exact meaning, nullability constraints, and data types across the entire system.
*Content:* An in-depth data dictionary, specifically focused on critical business fields prone to ambiguity, such as moderation states (status), legal identifiers (nit_agencia, rnt_agencia), and legal compliance flags (terminos_aceptados).

### modeling-conventions.md
Naming conventions, style rules, and database auditing guidelines.
*Content:* Strict enforcement of lowercase snake_case for tables and columns. Implementation of *UUID string (VARCHAR 36)* format identifiers to protect against enumeration attacks. Mandatory inclusion of audit fields (created_at, updated_at) and standard enablement of Soft Delete via deleted_at.

### normalization-assessment.md
Normalization analysis and data performance justification.
*Content:* Evaluation of database tables under the Third Normal Form (3NF). Technical justification for decoupling supporting logistical tables (hoteles, transportes, guias) to prevent data redundancy inside the core paquetes_turisticos entity.

### migration-strategy.md
Sequential strategy for updating and migrating data schemas.
*Content:* Exclusive use of the native *Laravel Database Migrations* engine controlled through chronological timestamps (YYYY_MM_DD_HHMMSS). Forward-only migration policies and safe execution of migrations in production environments via automated deployments.

---

## Correlations with other sections

| This section is fed by... | And feeds into... |
|---------------------------|-------------------|
| 02-domain/domain-events.md → Generated domain events. | Traceability and business state persistence in reservas and pagos. |
| Software Requirements Specification (SRS) → Business RF1 to RF20. | Definition of mandatory attributes, constraints, and nullability in models.md. |
| models.md | Travel, Agency, and Administrator front-end User Interfaces. |
| models.md | Eloquent Models and Controllers in the Laravel Backend. |

---

## Important data decisions in our project

### SQL over NoSQL
For Huila Travel Expedition, *MySQL 8.0* (SQL Relational Engine) was selected due to:
- *Strict ACID Transactions:* An indispensable requirement to ensure that the workflow of requesting, approving reservations, and updating calendar inventory takes place without collisions or concurrent oversales (RF9, RF11).
- *Native Referential Integrity:* Protects database integrity using foreign key constraints (ON DELETE RESTRICT) so a tour plan cannot be physically deleted if active or pending customer reservations are tied to it.

### Data Consistency
As a modular architecture based on Laravel's Eloquent ORM, data consistency between modules is *immediate and protected by database transactions* (DB::transaction). Even if automated SMTP email notifications fail (RF12) or asynchronous interactions trigger retries, the relational state of the reservation inside MySQL remains safe and clean from corruption.

---

## Questions this section answers

- *What data does the system handle?* Corporate identities and legal validation of travel agencies, logistical and location inventories of adventure/ecotourism plans in Huila, and the complete transaction history of tourist reservation requests.
- *Why was MySQL 8.0 chosen?* For its proven stability in shared hosting environments (RNF3), native support for ACID transactions, and speed in processing complex query indexing.
- *How is the schema updated without breaking the system?* Through Laravel's versioned migration files that alter the database incrementally, completely avoiding manual table modifications in production.
- *Who is the owner of each piece of data?* Each Laravel controller holds the authorship and access permissions based on roles (Administrador, Agencia, Turista) validated by security Middlewares (RF16).
