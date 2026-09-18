# 12 — UX/UI

> *What is this?* The user experience design: how the system looks, how it is navigated, and how it behaves from the end user's perspective.

## Why design comes before code
Changing a wireframe takes 5 minutes. Changing the code takes hours. Changing the code in production with real users can cost days and reputation. *Design first → implement later.*

---

## What is here and how to fill it in

### navigation-map.md ⭐ (Start here)
The map of all screens/pages and how they connect.

## Navigation map

### Public area (no authentication)
* / (Huila Travel Expedition Home)
* /auth/login
* /auth/register (Options: Traveler / Agency)
* /planes (Search results with filters by municipality, price, and duration)
* /planes/{id} (Detail sheet of the tourism experience and itinerary)

### Private area — Role: Tourist / Traveler
* /dashboard
* /reservas
* /reservas/list (History of past and active inquiries)
* /reservas/{id} (Detail of the request voucher and confirmation status)
* /profile (Personal account management)

### Private area — Role: Local Agency (Provider)
* /dashboard
* /planes
* /planes/new (Online form for creating a tourism plan)
* /planes/{id}/edit (Editing and update form)
* /reservas
* /reservas/list (Management panel for received requests for approval or cancellation)
* /profile (Update of contact information, social networks, and link to own website)

### Private area — Role: Platform Administrator
* /admin
* /agencias (Manual verification and validation of the National Tourism Registry - RNT)
* /reseñas (Manual moderation module for received ratings and comments)
* /overview (Administrative panel with general monthly statistics)

## Access matrix

| Screen | Tourist / Traveler | Local Agency | Administrator | Public |
| :--- | :---: | :---: | :---: | :---: |
| / (Home) | ✅ | ✅ | ✅ | ✅ |
| /auth/login / /auth/register | ❌ | ❌ | ❌ | ✅ |
| /planes/{id} (Plan Detail) | ✅ | ✅ | ✅ | ✅ |
| /dashboard (General) | ✅ | ✅ | ✅ | ❌ |
| /planes/new / /edit | ❌ | ✅ | ✅ | ❌ |
| /admin/* (Control Modules) | ❌ | ❌ | ✅ | ❌ |

---

### wireframes.md
Low-fidelity designs of the main screens. It contains the responsive and mobile-first visual structure (for screens starting from 320px wide) of the key components: the municipality search bar (Villavieja, San Agustín, Neiva, Rivera, Pitalito, La Plata), the featured plans section, and the reservation form with explicit acceptance of Law 1581 of 2012 (Data Protection).

### design-system.md
The project's design system: tokens, components, patterns. It contains the corporate palette based on the SENA Institutional Green (#39A900), typography adapted to the Bootstrap and Tailwind CSS frameworks, and the semantic states of the system (Available, Few slots left, No vacancy).

---

## Correlations with other sections

| This section is fed by... | And feeds into... |
| :--- | :--- |
| 04-requirements/user-stories.md → what flows exist (US-01 to US-20) | Screens implementing each US |
| 02-domain/entities-and-rules.md → what data to display (Agencies, Plans, Calendars, Bookings, Reviews) | Fields in wireframes and tables |
| 09-microservices/ → what APIs the frontend consumes (Laravel 10+ Backend REST API) | What data arrives at each screen |

---

## Questions this section must answer
* *How many screens does the system have?* It features the main page (Home), the authentication flow (Login/Registration/Recovery), the public search and plan detail flow, the private self-management panels for Agencies/Travelers, and the centralized control panel for the Administrator.
* *How does each type of user navigate?* Travelers browse freely and request reservations using an optimized checkout; agencies navigate through private real-time inventory forms; and the administrator manages requests using data tables and general statistical dashboards.
* *What visual components are repeated?* Buttons with synchronous/asynchronous loading states, descriptive plan cards with photos, red error messages underneath form fields, and dimmed-background modals for destructive confirmations are repeated throughout.
* *What is the system's visual language?* A clean, accessible, and responsive (mobile-first) design, governed by the identity of the Huila region ("desert of stars and green mountain") and chromatically aligned with SENA guidelines.
