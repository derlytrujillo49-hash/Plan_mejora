# Navigation Map

> Defines the screen structure of the system, how screens connect to each other, and what routes
> exist. It is the reference when frontend and backend discuss what endpoints exist
> or how to reach a feature.

---

## Frontend route structure

/                           → Home / landing page (Huila Travel Expedition)├── /auth│   ├── /login              → Authentication form (Access to agency panel, traveler account, or admin dashboard)│   ├── /register           → New user registration (Options: I am a traveler / I have an agency)│   └── /forgot-password    → Password recovery│├── /dashboard              → Main panel (authenticated)│   ├── /overview           → Summary and key metrics (Statistics based on user role)│   └── /notifications      → Notification center│├── /planes                 → Tour plans list (Search and filters by municipality, price, duration)│   ├── /new                → Creation form (Role only: Agency)│   └── /:id│       ├── /               → Resource detail (Experience detail, itinerary, rates, and availability calendar)│       └── /edit           → Edit form (Role only: Agency)│├── /reservas               → Booking history list (Past and active bookings)│   └── /:id                → Detail (Complete booking voucher overview and status)│├── /admin                  → Administration panel (role: Platform Administrator)│   ├── /agencias           → Agency verification (RNT validation and approval status)│   └── /reseñas            → Manual moderation of content and inappropriate reviews│└── /profile                → Authenticated user's profile (Contact info, social media, and website link)
---

## Screen map

| Screen | Route | Component | Minimum role | Backend service |
|--------|-------|-----------|--------------|----------------|
| Home | / | HomePage | Public | — |
| Login | /auth/login | LoginPage | Public | Registration and Authentication Module |
| Register | /auth/register | RegisterPage | Public | Registration and Authentication Module |
| Dashboard | /dashboard | DashboardPage | Tourist / Traveler | Bookings Module |
| Tour plans list | /planes | PlanesListPage | Public | Search and Filters Module |
| Tour plans detail | /planes/:id | PlanesDetailPage | Public | Tour Plans Management Module |
| Create Tour plans | /planes/new | PlanesFormPage | Agency | Tour Plans Management Module |
| Admin panel | /admin | AdminDashboard | Administrator | Administration and Reports Module |

---

## Main user flows

### Flow 1 — Search, Selection, and Booking Request

Landing (/) or Filters (/planes)│▼ Select a specific planPlan Detail (/planes/:id)│▼ Fill out booking form (Date, People, Rate) and accept terms (RF20)Booking Request│├── Available slot in calendar ──► Request Sent (Status: Pending)│└── Full slot in calendar ──────► UI Warning Alert (Prevents overbooking)
*Related HUs:* HU-11, HU-12, HU-16

### Flow 2 — Authentication

Landing (/)│▼ Click "Sign in"Login (/auth/login)│├── Valid credentials ──► Dashboard (/dashboard) based on role (Administrator, Agency, Tourist)│└── Invalid credentials ► Login with error message in Spanish (locked out on 5th attempt for 15 min)
*Related HUs:* HU-02, HU-04

---

## Navigation rules

| Rule | Description |
|------|-------------|
| Authentication | Routes under /dashboard, /planes/new, /planes/:id/edit, /reservas, /admin redirect to /auth/login if no session |
| Authorization | Routes under /admin redirect to /dashboard if the user does not have the Administrator role. Routes under /planes/new redirect to /dashboard if the user is not an Agency. |
| 404 | Undefined routes show the 404 screen with a link to dashboard |
| Confirmation | Destructive actions (delete plan, cancel reservation) show a confirmation dialog modal before executing |

---

## Correlations

- Design system (visual components) → 12-ux-ui/design-system.md
- Wireframes → 12-ux-ui/wireframes.md
- Frontend API contracts → 07-api/contracts/openapi/
- Roles and permissions → 00-governance/security-policy.md
