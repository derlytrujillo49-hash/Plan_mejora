# Definition of Done (DoD) — Huila Travel Expedition (HTE)

> Una Historia de Usuario o Tarea Técnica está **DONE** cuando cumple con TODOS los criterios de esta lista de verificación. Si falta un solo criterio, la historia NO se considera finalizada y regresa al estado In Progress.

---

## Mandatory Checklist

### Code & Quality
* [ ] El código implementa todos los criterios de aceptación especificados en la historia de usuario.
* [ ] El código fue revisado y aprobado por al menos 1 integrante del equipo mediante Pull Request en GitHub.
* [ ] El código sigue los estándares del proyecto (supera las validaciones de Linter y Formateo en CI/CD).
* [ ] No se introduce deuda técnica sin registrarla explícitamente en `15-project-control/tech-backlog.md`.

### Testing
* [ ] Se escribieron pruebas unitarias para la lógica de negocio nueva o modificada.
* [ ] La cobertura de pruebas (coverage) se mantiene o supera la línea base del proyecto (mínimo 70%).
* [ ] Todas las pruebas pasan exitosamente de manera local y en el pipeline de Integración Continua (CI).
* [ ] Criterios de aceptación verificados manualmente o mediante pruebas automatizadas.

### Architecture & Contracts
* [ ] Los cambios no rompen la integración con otros servicios del ecosistema HTE.
* [ ] Si la API cambia: contrato OpenAPI actualizado en `07-api/contracts/`.
* [ ] Si el modelo de datos cambia: modelo del servicio actualizado en su respectivo `data-model.md`.
* [ ] Si se agregan o modifican eventos de dominio: archivo `02-domain/domain-events.md` actualizado.

### Deployment & CI/CD
* [ ] La rama se integra sin conflictos a la rama principal (`main` o `dev`).
* [ ] El pipeline de CI/CD completa con éxito la compilación y ejecución de tests automatizados.
* [ ] Despliegue exitoso en el entorno de pruebas o staging.
* [ ] Pruebas básicas de humo (smoke tests) aprobadas en el entorno desplegado.

### Documentation
* [ ] Archivo `README.md` del servicio actualizado si cambió la interfaz pública o la configuración.
* [ ] Registro de decisiones arquitectónicas (ADR) creado o actualizado si se tomó una decisión técnica significativa.

---

## Allowed Exceptions

Las siguientes excepciones son válidas únicamente con la aprobación explícita del Tech Lead o del equipo completo:

* Omisión de pruebas End-to-End (E2E) por limitaciones técnicas o de entorno (requiere registrar el riesgo en `15-project-control/risks.md`).
* Documentación técnica no crítica postergada por entrega urgente (requiere crear una tarea en `15-project-control/tech-backlog.md`).

---

## What is NOT a Done Criterion

* "El código funciona en mi máquina" — Debe estar integrado en el repositorio remoto.
* "Funciona en local" — Debe funcionar e integrarse correctamente en el entorno de desarrollo/staging.
* "El Product Owner/Instructor lo aprobó verbalmente" — Debe cumplir con los criterios técnicos comprobables del repositorio.
