## MODELS - HUILA TRAVEL EXPEDITIONS

1. Database Architecture: Within the project's current scope and in compliance with SRS guidelines, the platform organizes its entities using a unified relational schema on a single centralized engine, isolating the transactional logic of each module through clear functional prefixes in the table names.
2. 2. Standard audit fields: All system tables mandatorily include the following fields for technical traceability control:
   3. id          VARCHAR(36) PRIMARY KEY, -- Formato UUID string para portabilidad
created_at  TIMESTAMP   NOT NULL DEFAULT CURRENT_TIMESTAMP,
updated_at  TIMESTAMP   NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
deleted_at  TIMESTAMP   NULL     DEFAULT NULL
3. Soft delete by default: Physical data deletion is not performed on business entities (such as agencies or packages) to preserve system history and reliability (RNF11). Soft deletion is implemented using the deleted_at column.
4. Naming conventions:
5. Tables: Lowercase, plural, and snake_case style (e.g., paquetes_turisticos).
6. Columns: Lowercase and descriptive (e.g., nombre_agencia).7.  Foreign Keys (FK): Format [singular_table_name]_id (e.g., agencia_id).
8. Indexes: Named using the prefix idx_[table_name]_[columns].

9. Service Domain: Huila Travel Expedition | MonolithDB Engine: MySQL 8.0 | Engine Justification: Full Transactional Support: Required to handle concurrent traffic of 30 to 50 simultaneous users during peak season (RNF3) and ensure the calendar safely deducts available slots to prevent overbooking (RF9). Optimized Relational Queries: Highly efficient handling of JOINs between tour packages and their associated service logistics tables (hotels, guides, transport, and destinations) to deliver load times of under 3 seconds (RNF1).

    Table: administrators | Purpose: Stores accounts for the internal management team with permissions to moderate content, approve agencies, and view global statistics (RF15, RF16).

   CREATE TABLE administradores (
  id              VARCHAR(36) PRIMARY KEY,
  cedula          INT         NOT NULL UNIQUE,
  nombre          VARCHAR(100) NOT NULL,
  telefono        INT         NOT NULL,
  email           VARCHAR(100) NOT NULL UNIQUE,
  password        VARCHAR(255) NOT NULL,
  status          VARCHAR(50) NOT NULL DEFAULT 'ACTIVE'
                  CHECK (status IN ('ACTIVE', 'INACTIVE')),
  
  -- Audit
  created_at      TIMESTAMP   NOT NULL DEFAULT CURRENT_TIMESTAMP,
  updated_at      TIMESTAMP   NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  deleted_at      TIMESTAMP   NULL     DEFAULT NULL
);

Table: agenciasPurpose: Almacena los perfiles de las agencias de viajes locales de la región, controlando su estado de aprobación legal mediante la verificación del NIT y RNT (RF1).

CREATE TABLE agencias (
  id              VARCHAR(36) PRIMARY KEY,
  nit_agencia     VARCHAR(50) NOT NULL UNIQUE,
  rnt_agencia     VARCHAR(50) NOT NULL UNIQUE,
  nombre_agencia  VARCHAR(150) NOT NULL,
  direccion       VARCHAR(150) NOT NULL,
  telefono        VARCHAR(50)  NOT NULL,
  email           VARCHAR(100) NOT NULL UNIQUE,
  password        VARCHAR(255) NOT NULL,
  status          VARCHAR(50) NOT NULL DEFAULT 'PENDIENTE'
                  CHECK (status IN ('PENDIENTE', 'APROBADA', 'RECHAZADA')),
  administrador_id VARCHAR(36) NULL REFERENCES administradores(id) ON DELETE RESTRICT,
  
  -- Audit
  created_at      TIMESTAMP   NOT NULL DEFAULT CURRENT_TIMESTAMP,
  updated_at      TIMESTAMP   NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  deleted_at      TIMESTAMP   NULL     DEFAULT NULL
);

CREATE INDEX idx_agencias_rnt ON agencias (rnt_agencia);
CREATE INDEX idx_agencias_deleted ON agencias (deleted_at) WHERE deleted_at IS NULL;

Table: paquetes_turisticos | Purpose: Contains the catalog of tourism experience offers published and self-managed by agencies in the department (RF4).

CREATE TABLE paquetes_turisticos (
  id              VARCHAR(36) PRIMARY KEY,
  nombre          VARCHAR(150) NOT NULL,
  precio          INT         NOT NULL,
  duracion_dias   INT         NOT NULL,
  categoria       VARCHAR(100) NOT NULL, -- Categorías de turismo (RF6)
  status          VARCHAR(50) NOT NULL DEFAULT 'ACTIVE'
                  CHECK (status IN ('ACTIVE', 'INACTIVE')),
  agencia_id      VARCHAR(36) NOT NULL REFERENCES agencias(id) ON DELETE CASCADE,
  
  -- Audit
  created_at      TIMESTAMP   NOT NULL DEFAULT CURRENT_TIMESTAMP,
  updated_at      TIMESTAMP   NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  deleted_at      TIMESTAMP   NULL     DEFAULT NULL
);

CREATE INDEX idx_paquetes_agencia ON paquetes_turisticos (agencia_id);
CREATE INDEX idx_paquetes_precio_duracion ON paquetes_turisticos (precio, duracion_dias);
CREATE INDEX idx_paquetes_deleted ON paquetes_turisticos (deleted_at) WHERE deleted_at IS NULL;

Table: destinos | Purpose: Stores the specific municipalities in Huila associated with each package to enable real-time public search filters (RF7).

CREATE TABLE destinos (
  id                  VARCHAR(36) PRIMARY KEY,
  nombre_destino      VARCHAR(100) NOT NULL, -- Ej: Villavieja, San Agustín, Yaguará
  paquete_turistico_id VARCHAR(36) NOT NULL REFERENCES paquetes_turisticos(id) ON DELETE CASCADE,
  
  -- Audit
  created_at          TIMESTAMP   NOT NULL DEFAULT CURRENT_TIMESTAMP,
  updated_at          TIMESTAMP   NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);

CREATE INDEX idx_destinos_paquete ON destinos (paquete_turistico_id);
CREATE INDEX idx_destinos_nombre ON destinos (nombre_destino);

Table: hotels | Purpose: Details logistical accommodation information linked as a benefit or service included in a specific tourism plan.

CREATE TABLE hoteles (
  id                  VARCHAR(36) PRIMARY KEY,
  nombre_hotel        VARCHAR(150) NOT NULL,
  direccion           VARCHAR(150),
  municipio           VARCHAR(100) NOT NULL,
  telefono            VARCHAR(50),
  paquete_turistico_id VARCHAR(36) NOT NULL REFERENCES paquetes_turisticos(id) ON DELETE CASCADE,
  
  -- Audit
  created_at          TIMESTAMP   NOT NULL DEFAULT CURRENT_TIMESTAMP,
  updated_at          TIMESTAMP   NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);

CREATE INDEX idx_hoteles_paquete ON hoteles (paquete_turistico_id);

Table: transportes | Purpose: Records operational data for the transport arranged to move travelers during expedition routes.

CREATE TABLE transportes (
  id                  VARCHAR(36) PRIMARY KEY,
  tipo_transporte     VARCHAR(100) NOT NULL, -- Ej: Campero 4x4, Lancha, Buseta
  capacidad           INT         NOT NULL,
  empresa             VARCHAR(100),
  paquete_turistico_id VARCHAR(36) NOT NULL REFERENCES paquetes_turisticos(id) ON DELETE CASCADE,
  
  -- Audit
  created_at          TIMESTAMP   NOT NULL DEFAULT CURRENT_TIMESTAMP,
  updated_at          TIMESTAMP   NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);

CREATE INDEX idx_transportes_paquete ON transportes (paquete_turistico_id);

Table: Guides. Purpose: Records the tour guides assigned to lead the activities for each travel package.

CREATE TABLE guias (
  id                  VARCHAR(36) PRIMARY KEY,
  nombre              VARCHAR(150) NOT NULL,
  cedula              INT         NOT NULL UNIQUE,
  numero_telefono     VARCHAR(50)  NOT NULL,
  paquete_turistico_id VARCHAR(36) NOT NULL REFERENCES paquetes_turisticos(id) ON DELETE CASCADE,
  
  -- Audit
  created_at          TIMESTAMP   NOT NULL DEFAULT CURRENT_TIMESTAMP,
  updated_at          TIMESTAMP   NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);

CREATE INDEX idx_guias_paquete ON guias (paquete_turistico_id);

Table: turistas | Purpose: Stores contact information and basic profiles of domestic or international travelers who interact with the website and request travel plans (RF16).

CREATE TABLE turistas (
  id              VARCHAR(36) PRIMARY KEY,
  nombre          VARCHAR(150) NOT NULL,
  cedula          INT         NOT NULL UNIQUE,
  cellular        VARCHAR(50)  NOT NULL,
  correo          VARCHAR(100) NOT NULL UNIQUE,
  
  -- Audit
  created_at      TIMESTAMP   NOT NULL DEFAULT CURRENT_TIMESTAMP,
  updated_at      TIMESTAMP   NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);

Table: reservations | Purpose: Critical transactional entity that captures slot requests made by tourists, subject to status validation by agencies (RF10, RF11).

CREATE TABLE reservas (
  id                  VARCHAR(36) PRIMARY KEY,
  fecha_reserva       DATE        NOT NULL,
  cantidad_personas   INT         NOT NULL,
  status              VARCHAR(50) NOT NULL DEFAULT 'PENDIENTE'
                      CHECK (status IN ('PENDIENTE', 'APROBADA', 'CANCELADA')),
  paquete_turistico_id VARCHAR(36) NOT NULL REFERENCES paquetes_turisticos(id) ON DELETE RESTRICT,
  turista_id          VARCHAR(36) NOT NULL REFERENCES turistas(id) ON DELETE CASCADE,
  terminos_aceptados  BOOLEAN     NOT NULL DEFAULT TRUE, -- Soporte legal obligatorio (RF20)
  
  -- Audit
  created_at          TIMESTAMP   NOT NULL DEFAULT CURRENT_TIMESTAMP,
  updated_at          TIMESTAMP   NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
  deleted_at          TIMESTAMP   NULL     DEFAULT NULL
);

CREATE INDEX idx_reservas_fecha_status ON reservas (fecha_reserva, status);
CREATE INDEX idx_reservas_turista ON reservas (turista_id);
CREATE INDEX idx_reservas_deleted ON reservas (deleted_at) WHERE deleted_at IS NULL;

Table: payments | Purpose: Securely stores an audit trail of the amounts or commercial vouchers assigned to confirmed reservations.

CREATE TABLE pagos (
  id                  VARCHAR(36) PRIMARY KEY,
  fecha_pago          DATE        NOT NULL,
  monto               INT         NOT NULL,
  metodo_pago         VARCHAR(50) NOT NULL, -- Tarjeta, PSE, En efectivo
  reserva_id          VARCHAR(36) NOT NULL REFERENCES reservas(id) ON DELETE RESTRICT,
  
  -- Audit
  created_at          TIMESTAMP   NOT NULL DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_pagos_reserva ON pagos (reserva_id);


CREATE INDEX idx_administradores_deleted ON administradores (deleted_at) WHERE deleted_at IS NULL;
