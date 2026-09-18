# System Overview - HUILA TRAVEL EXPEDITION

> This is the first page someone new reads. They must be able to understand the system in 5 minutes.

---

## What is HUILA TRAVEL EXPEDITION?

HUILA TRAVEL EXPEDITION (HTE) es un sitio web local diseñado para centralizar la oferta turística del departamento del Huila. La plataforma funciona como un espacio común donde las agencias de viajes de la región pueden publicar, administrar y promocionar sus paquetes y experiencias, permitiendo a los turistas nacionales e internacionales buscar, comparar y seleccionar planes de manera organizada en un solo lugar.

## Problem it solves

*Before the system:* Muchas agencias de viajes locales del Huila no contaban con sitios web propios ni plataformas digitales efectivas para comercializar sus servicios, lo que limitaba drásticamente su visibilidad y competitividad. Por otro lado, los turistas debían consultar múltiples páginas informales o redes sociales de forma dispersa, generando pérdidas de tiempo y dificultades para identificar la mejor opción según precios o servicios.

*With the system:* El proceso se centraliza y optimiza por completo. Las agencias locales obtienen un espacio común para competir en igualdad de condiciones digitales, independientemente de si tienen web propia o no. Los viajeros acceden a un catálogo unificado, confiable y fácil de comparar mediante filtros avanzados, mejorando drásticamente su experiencia de planificación y fomentando la economía y cultura regional del Huila.

## Main users

| Role | Description | What they do in the system |
|------|-------------|--------------------------|
| Administrador de Plataforma | Equipo gestor interno del sistema con formación técnica. | Valida las agencias mediante su Registro Nacional de Turismo (RNT), modera las reseñas recibidas y supervisa las estadísticas globales de la plataforma. |
| Agencia Local (Proveedor) | Agencia de viajes regional registrada con conocimiento digital básico. | Registra y edita sus planes turísticos, administra sus calendarios de disponibilidad en tiempo real para evitar sobreventas y gestiona la aprobación de reservas. |
| Viajero (Usuario Final) | Turista nacional o internacional sin perfil técnico específico. | Busca, filtra y compara experiencias turísticas por municipio o precio, envía solicitudes de reserva y califica los servicios realizados. |

## Technology stack

| Layer | Technology | Justification |
|-------|-----------|---------------|
| Frontend | Blade Templates + Bootstrap / Tailwind CSS | Permite construir una interfaz intuitiva, accesible y con un diseño adaptivo (Mobile-First) óptimo para pantallas desde 320px sin añadir sobrecarga al servidor. |
| Backend | Laravel 10+ (PHP 8.2+) | Proporciona una arquitectura de monolito modular robusta, segura y con herramientas nativas eficientes para el control de accesos, rate-limiting y hashing de contraseñas. |
| Database | MySQL 8.0 | Motor relacional estándar que garantiza la integridad referencial de los datos y el uso de transacciones ACID para evitar sobreventas concurrentes en los cupos de los planes. |
| Message broker | N/A (Monolito síncrono) | El alcance inicial definido en el SRS se enfoca en procesos síncronos optimizados directos, reduciendo costes de infraestructura en el hosting compartido inicial. |
| Infrastructure | Servidor Apache/Nginx (Fase inicial: Hosting compartido / Escalado: VPS) | Se inicia con una configuración ligera (1 vCPU, 1 GB RAM) para optimizar costos de despliegue, con capacidad de migración simple a un entorno VPS en menos de 8 horas. |

## Current status

- *Phase:* In initial development (Ficha ADSO 3239188).
- *Current version:* v0.1.0 (Initial architectural setup and governance documentation).
- *Last release:* September 14, 2026.
- *Next milestone:* Complete the base MySQL database relational schema and deploy core authentication modules.

## Project contacts

| Role | Name | Contact |
|------|------|---------|
| Tech Lead + Project Leader | Manuel Felipe Caviedes Cordero | manufelipecaviedes@gmail.com |
| Backend & Logic Developer | Luisa Fernanda Ortega Hernandez | luisaortegahernandez97@gmail.com |
| Backend & Logic Developer | Juan Diego Oteca Pedreros | joteca3@gmail.com |
| Database & Frontend Developer | Derly Natalia Trujillo Santofimio | derlytrujillo49@gmail.com |
