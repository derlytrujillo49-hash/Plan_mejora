
# Project Glossary - HUILA TRAVEL EXPEDITION

> Define here all technical and business terms used in the project.
> This is the official dictionary — if there is ambiguity, this document wins.
> Add terms throughout the project, not only at the start.

---

## How to use this glossary

1. Before using a technical or business term in code, docs, or conversations: look it up here.
2. If it's not there: add it with its definition.
3. If there is disagreement about the definition: discuss it as a team and update this document.

---

## Domain terms

| Term | Definition | Notes / Synonyms |
|------|-----------|-----------------|
| Páginas Web | Sitios en línea que proporcionan información y recursos sobre diversas ofertas o paquetes de turismo. | Sitios turísticos |
| Travel Tech | Tecnología turística. Conjunto de soluciones digitales aplicadas al sector del turismo. | N/A |
| Agregador | Plataforma que centraliza, compara y facilita la búsqueda de servicios de múltiples proveedores en un solo lugar. | Plataforma centralizadora |
| OTA (Online Travel Agency) | Agencia de Viajes en Línea. Plataformas internacionales como Booking.com, Airbnb o Despegar.com que comercializan servicios de alojamiento y experiencias. | Agencias globales |
| Motor de Reserva | Software lógico integrado que permite buscar disponibilidad en tiempo real y completar una solicitud de reserva en línea. | Motor HTE |
| Pasarela de Pago | Servicio externo seguro que autoriza y procesa transacciones financieras en línea de forma segura (tarjetas de crédito, PSE). | Pasarelas (Wompi, PayU, MercadoPago) |

---

## Technical terms of the project

| Term | Definition |
|------|-----------|
| Base de Datos | Conjunto estructurado de información alojado en MySQL 8.0 que permite el almacenamiento y la gestión eficiente de datos del sistema con integridad referencial. |
| Cronograma | Plan detallado elaborado en las fases de planificación que establece las actividades programadas con sus tiempos de inicio y finalización. |
| Historia de Usuario (HU) | Descripción breve y ágil de una funcionalidad del sistema expresada desde la perspectiva del usuario o rol que la necesita (HU-01 a HU-20). |
| Story Points (SP) | Unidad de estimación relativa utilizada por el equipo ADSO para medir el esfuerzo, complejidad y riesgo de desarrollo de una historia de usuario. |
| Monolito Modular | Arquitectura de software donde todos los componentes del sistema (Frontend y Backend en Laravel 10+) se compilan y ejecutan juntos, divididos lógicamente en subsistemas. |
| Middleware | Capa intermedia de software en Laravel utilizada para interceptar, validar y filtrar las solicitudes HTTP, controlando accesos por roles o límites de intentos. |
| Idempotencia | Property of an operation (like booking requests) to produce the same clean result without duplicates if executed multiple times under concurrent traffic. |
| Control de Acceso (RBAC) | Modelo de seguridad que restringe las acciones del sistema basándose en tres roles predefinidos: Administrador, Agencia y Turista. |

---

## Acronyms

| Acronym | Meaning |
|---------|---------|
| HTE | HUILA TRAVEL EXPEDITION (Sigla identificadora de la plataforma) |
| RNT | Registro Nacional de Turismo (Habilitación legal obligatoria para agencias en Colombia) |
| SRS | Software Requirements Specification / Especificación de Requisitos de Software |
| RF | Requisito Funcional (Funcionalidades esenciales del sistema) |
| RNF | Requisito No Funcional (Atributos de calidad: rendimiento, seguridad, usabilidad) |
| PII | Personally Identifiable Information / Información de Identificación Personal (Protegida por Ley 1581) |
| API | Application Programming Interface / Interfaz de Programación de Aplicaciones |
| CRUD | Create, Read, Update, Delete (Operaciones básicas de persistencia en MySQL) |
| ADR | Architecture Decision Record / Registro de Decisión Arquitectónica |
| PR | Pull Request / Solicitud de Extracción en GitHub |
| DoD | Definition of Done / Definición de Terminado |
| DoR | Definition of Ready / Definición de Listo
