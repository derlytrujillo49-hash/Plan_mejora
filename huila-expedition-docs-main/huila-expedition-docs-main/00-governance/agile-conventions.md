# Agile Team Conventions — Huila Travel Expedition (HTE)

> Define la dinámica de trabajo del equipo durante los ciclos de desarrollo. Acuerdo firmado por el equipo previo al primer sprint.

---

## Sprint Structure

* **Duration:** 2 semanas
* **Sprint Start:** Lunes (08:00 AM)
* **Sprint End:** Viernes de la semana 2 (05:00 PM)
* **Current Sprint:** Sprint 1 — 2026-09-14 a 2026-09-25
* **Estimated Capacity:** 25 story points por sprint

---

## Ceremonies

### Sprint Planning
* **When:** Primer lunes del sprint — 08:30 AM
* **Duration:** Máximo 2 horas
* **Who:** Equipo de desarrollo, Scrum Master y Product Owner
* **Goal:** Seleccionar historias de usuario del Backlog, comprometerse con el alcance del sprint y desglosarlas en tareas técnicas
* **Output Artifact:** Sprint Backlog actualizado en GitHub Projects

### Daily Stand-up
* **When:** De lunes a viernes — 09:00 AM
* **Duration:** Máximo 15 minutos
* **Format:**
  1. ¿Qué hice ayer para contribuir al objetivo del sprint?
  2. ¿Qué haré hoy para contribuir al objetivo del sprint?
  3. ¿Tengo algún bloqueo técnico o dependencia externa?
* **Rule:** Las discusiones técnicas profundas se realizan después del daily en la sesión de *After-Daily*, no durante la reunión.

### Sprint Review
* **When:** Último viernes del sprint — 03:00 PM
* **Duration:** Máximo 45 minutos
* **Who:** Equipo HTE + Product Owner / Instructor SENA
* **Goal:** Demostrar el incremento de software funcional (historias finalizadas que cumplen el DoD) y recibir retroalimentación

### Sprint Retrospective
* **When:** Último viernes del sprint — 04:00 PM (posterior a la Review)
* **Duration:** Máximo 45 minutos
* **Format:** Qué funcionó bien, qué podemos mejorar y compromisos de acción
* **Rule:** Cada retrospectiva debe generar al menos 1 acción de mejora concreta asignada con un responsable y fecha límite

### Backlog Refinement
* **When:** Miércoles de la segunda semana — 02:00 PM
* **Duration:** Máximo 1 hora
* **Goal:** Detallar, priorizar y estimar historias de usuario para el siguiente sprint
* **Exit Criterion:** La historia de usuario cumple estrictamente con el Definition of Ready (DoR)

---

## Estimation

### Scale

* **1 Point (Trivial):** Ajustes menores, tareas de documentación o cambios sencillos realizados en pocas horas.
* **2 Points (Small):** Tarea técnica simple completada en aproximadamente un día.
* **3 Points (Medium):** Tarea de complejidad moderada que requiere entre 2 y 3 días de desarrollo.
* **5 Points (Large):** Funcionalidad compleja que toma casi la totalidad del sprint.
* **8 Points (Very Large):** Funcionalidad muy grande; debe dividirse obligatoriamente en historias más pequeñas.
* **13 Points (Epic):** Épica o requerimiento amplio; requiere subdivisión estricta antes de ingresar al sprint.

* **Technique:** Planning Poker
* **Tool:** Planning Poker Online

### Estimation Rule
* Si existe una discrepancia de 2 o más niveles en la votación (por ejemplo, entre 3 y 8 puntos), los participantes exponen sus puntos de vista antes de realizar una nueva votación.
* Ninguna historia de usuario estimada en 8 o 13 puntos puede ingresar al sprint sin haber sido dividida en tareas menores.

---

## Backlog Tool

* **Tool:** GitHub Projects
* **Board URL:** https://github.com/orgs/HTE-Project/projects/1

### Board Columns

* **Backlog:** Historias de usuario pendienet de refinamiento y estimación.
* **Ready:** Historias priorizadas y listas para ingresar al sprint (cumplen DoR).
* **In Progress:** Tareas activamente en desarrollo por un integrante del equipo.
* **In Review:** Cambios enviados mediante Pull Request a espera de revisión de código.
* **Done:** Funcionalidades que cumplen totalmente con el Definition of Done (DoD) y han sido integradas a `main`.

---

## Team Velocity

* **Sprint 1:** Por definir
* **Sprint 2:** Por definir
* **Sprint 3:** Por definir
* **Average:** Por definir

---

## Related Documents

* Definition of Ready → `00-governance/definition-of-ready.md`
* Definition of Done → `00-governance/definition-of-done.md`
* Risk Management → `15-project-control/risks.md`
* Technical Debt Backlog → `15-project-control/tech-backlog.md`
