# Domain Map — Bounded Contexts (Huila Travel Expedition - HTE)

> The domain map is the central artifact of DDD (Domain-Driven Design). It defines the conceptual boundaries of the HTE system and the relationships between its bounded contexts.

---

## Before Completing This Document: Event Storming

**Event Storming** is a collaborative workshop used to model the domain before writing code. It lasts between 2 and 4 hours and involves the entire team (developers, PO, and tourism business experts).

* **Materials:** A large wall, sticky notes in 4 colors, and markers.

* **Color Convention:**
* **🟠 Orange — Domain Events:** Past business occurrences. Examples: `AgencyRegistered`, `BookingRequested`, `TourPlanPublished`. 
* **🔵 Blue — Commands:** Actions that trigger an event. Examples: `RegisterAgency`, `RequestBooking`, `PublishTourPlan`. 
* **🟡 Yellow — Actors:** Who executes the command. Examples: `Tourist`, `Travel Agency`, `Administrator`. 
* **🩷 Pink — External Systems / Integration:** External integration points. Examples: `SMTP Email`, `Map/GPS Service`.

* **Session Steps:**
1. **(30 min)** Post all business events in chronological order. 
2. **(30 min)** Identify the command or actor that triggers each event. 
3. **(45 min)** Group related events—each group represents a candidate Bounded Context. 
4. **(30 min)** Draw dependency relationships between contexts. 
5. **(30 min)** Discuss the resulting map and agree on the Ubiquitous Language.

---

## 1. Domain Overview

Huila Travel Expedition (HTE) is a centralized web platform designed to promote, manage, and boost tourism in the Huila department. It allows local tourism agencies to register, verify their RNT (National Tourism Registry), and publish their eco-tourism, adventure, and cultural tour plans. Likewise, it enables tourists to browse the catalog, filter experiences by municipality or category, submit booking requests, and rate the services received.

---

## 2. Identified Bounded Contexts

A **Bounded Context** is the explicit boundary within which a particular domain model has a consistent and unique meaning.

### Bounded Context 1: Agency Management

* **Context Details:**
* **Name:** Agency Management
* **Responsibility:** Managing the lifecycle, legal verification (RNT, NIT), profile, and accreditation status of travel agencies in Huila. 
* **Responsible Team:** HTE Core Team
* **Microservice(s):** `agency-service`
* **Database:** PostgreSQL (`hte_agencies_db`)
* **Key Ubiquitous Language:** Agency, RNT (National Tourism Registry), Verification, NIT, Accreditation.

* **Specific Terms (Ubiquitous Language):**
* **Agency:** A tourism service provider formally established in Huila. *(In the Authentication context, it is simply treated as a `User`)*. 
* **RNT:** Legal identifier with the Ministry of Commerce, Industry, and Tourism. *(Term exclusive to this context)*.

---

### Bounded Context 2: Tour Plan Catalog

* **Context Details:**
* **Name:** Tour Plan Catalog
* **Responsibility:** Managing the creation, classification by tourism type, images, itineraries, pricing, and publication of tourism offers. * **Responsible Team:** HTE Core Team
* **Microservice(s):** `tour-plan-service`
* **Database:** PostgreSQL + Redis (`hte_catalog_db`)
* **Key Ubiquitous Language:** Tour Plan, Category, Itinerary, Destination, Photo Gallery, Rate.

* **Specific Terms (Ubiquitous Language):**
* **Tour Plan:** A structured offering that includes activities, duration, meeting points, and price within Huila. *(In the Booking context, it is referred to as a `Bookable Product`)*. 
* **Category:** Type of tourism (Ecological, Archaeological, Adventure, Gastronomic).

---

### Bounded Context 3: Booking Management

* **Context Details:**
* **Name:** Booking Management
* **Responsibility:** Managing tourist booking requests, verifying slot availability, coordinating approvals/cancellations with agencies, and managing the calendar. 
* **Responsible Team:** HTE Core Team
* **Microservice(s):** `booking-service`
* **Database:** PostgreSQL (`hte_bookings_db`)
* **Key Ubiquitous Language:** Booking, Tourist, Slot, Booking Status (Pending, Approved, Cancelled), Availability Calendar.

* **Specific Terms (Ubiquitous Language):**
* **Booking:** A formal request for slot(s) for a specific date made by a tourist. 
* **Tourist:** End user requesting to experience a tour plan. *(In the Notifications context, this is handled as a `Recipient`)*.

---

### Bounded Context 4: Reviews and Ratings
