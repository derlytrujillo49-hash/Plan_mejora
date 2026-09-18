# System Scope - HUILA TRAVEL EXPEDITION

> Scope prevents scope creep and aligns expectations.
> It is equally important to define what the system does NOT do as what it does.
> Review this document at the start of each planning cycle.

---

## In Scope

What the system *DOES build and maintain*:

### MVP Features

| # | Feature | Description | Responsible module |
|---|---------|-------------|---------------------|
| 1 | Registro y Autenticación de Agencias | Formulario en línea que permite el registro con validaciones de NIT y RNT en tiempo real, quedando en estado 'pendiente' para verificación (RF1, RF2, HU-01). | Subsystem 3: Agencias / Módulo Auth |
| 2 | Gestión de Planes Turísticos y Cupos | Permite a las agencias crear, editar y eliminar planes con clasificación por categorías, subida de hasta 10 imágenes con autocompresión e inventario con calendario para evitar sobreventas (RF4, RF5, RF6, RF9). | Subsystem 3: Agencias / Módulo Planes |
| 3 | Búsqueda, Filtros y Reservas síncronas | Permite a los turistas buscar y comparar planes por municipio, precio y duración, y enviar una solicitud de reserva directa con aceptación obligatoria de términos (RF7, RF10, RF20). | Subsystem 2: Turistas / Módulo Reservas |
| 4 | Panel Administrativo y Calificaciones | Panel para que el Administrador valide agencias, modere reseñas de estrellas y visualice estadísticas generales de la plataforma (RF14, RF15, RF16). | Subsystem 1: Administración |

### Included integrations

| External system | Integration type | Purpose |
|----------------|-----------------|---------|
| Servidor de Correo (SMTP) | Protocolo SMTP con TLS | Envío automático de correos de confirmación transaccionales a turistas y agencias en menos de 2 minutos tras cambios en las reservas (RF12, HU-14). |
| API de WhatsApp Business | API REST (JSON) | Envío opcional de notificaciones de soporte y contacto rápido con canales de soporte de agencias (Módulo Soporte). |

### Environments being built

| Environment | Purpose |
|-------------|---------|
| Local | Desarrollo en las computadoras del equipo ADSO utilizando servidores locales (XAMPP / Laragon). |
| Development (dev) | Integración continua en la rama dev de GitHub para pruebas del equipo de desarrollo. |
| Production / Delivery | Despliegue de la versión final en hosting compartido para validación técnica por parte de la instructora. |

---

## Out of Scope

What the system *does NOT build* in this version and why:

| # | What is out of scope | Reason | Future version? |
|---|---------------------|--------|----------------|
| 1 | Pasarela de pagos en línea e intermediación financiera | Excluido explícitamente en el alcance inicial del SRS para enfocar el MVP en un modelo meramente informativo y comparativo regional. | Sí — Fase de escalado |
| 2 | Sincronización automática externa (Channel Manager) | El ecosistema inicial es de autogestión directa en la plataforma por falta de APIs públicas unificadas de las agencias locales del Huila. | Sí — Requerimiento a futuro |
| 3 | Aplicación Móvil Nativa (iOS / Android) | Fuera de presupuesto de tiempo del ciclo formativo; se mitiga utilizando diseño responsivo móvil-first (RNF4). | Sí — Planeado a futuro |

### What another system / team handles (and why not us)

| Feature | Who builds it | Why not us |
|---------|--------------|-----------|
| Procesamiento de transacciones bancarias | Bancos de las agencias / Canales externos | El sistema es de acceso público comparativo y no almacena datos financieros ni asume responsabilidades PCI-DSS de recaudo. |

---

## Scope assumptions

> These assumptions are taken to be true. If they change, the scope must be renegotiated.

| # | Assumption | Consequence if false |
|---|-----------|---------------------|
| 1 | Las agencias locales del Huila son responsables de proveer y mantener actualizado su propio contenido (fotos, textos, precios, inventario). | El sistema perdería confiabilidad y requeriría un equipo de carga de datos masivo. |
| 2 | El volumen de tráfico concurrente inicial se mantendrá bajo los 50 usuarios simultáneos en hosting compartido. | Se degradaría el rendimiento de 3 segundos (RNF1) obligando a migrar anticipadamente a un VPS. |
| 3 | Los turistas y agencias cuentan con acceso a internet estable y navegadores web modernos para usar la plataforma. | El diseño mobile-first y las validaciones en tiempo real no funcionarían correctamente. |

---

## Constraints

| Type | Description |
|------|-------------|
| *Time* | El incremento de software inicial debe entregarse cumpliendo el ciclo de Sprints de la ficha ADSO (Año Académico 2026). |
| *Budget* | Limitado al esfuerzo de desarrollo de la etapa práctica de la ficha, sin inversión inicial de capital para APIs pagas. |
| *Technology* | Obligatorio el uso del stack del programa: Backend en Laravel 10+, PHP 8.2+, Base de Datos MySQL 8.0 y Frontend adaptivo. |
| *Regulatory* | Cumplimiento estricto en Colombia de la Ley 1581 de 2012 (Habeas Data), Ley 1480 de 2011 (Estatuto del Consumidor) y verificación de RNT (Ley 2068 de 2020). |
| *Team* | 4 desarrolladores aprendices con dedicación parcial al proyecto (Manuel Caviedes, Luisa Ortega, Juan Oteca, Natalia Trujillo). |

---

## External dependencies

| Dependency | Team / Provider | Required date | Status |
|-----------|----------------|--------------|--------|
| Verificación del Registro Nacional de Turismo (RNT) | Ministerio de Comercio, Industria y Turismo (MinCIT) | Fase de despliegue | 🟢 Disponible (Consulta pública manual por Administrador) |
| Servidor de correo saliente SMTP | Proveedor de hosting contratado | Fase de pruebas de Sprint 3 | 🟡 En proceso de configuración |

---

## How to update the scope

The scope can change, but the change has a process:

1. Document the proposed change in this file.
2. Evaluate the impact on schedule, database schemas, and overall development effort.
3. Obtain approval from the Project Leader (Manuel Caviedes) and Instructor Karol Daniela Correa Trujillo.
4. Update the requirements catalog in 04-requirements/user-stories.md.

---

## Correlations

- System overview and problem context → 01-context/overview.md
- Term glossary and acronyms → 01-context/glossary.md
- Definition of Done (Checks for features) → 00-governance/definition-of-done.md
