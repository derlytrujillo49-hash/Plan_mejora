Domain Events — Huila Travel Expedition (HTE)
A domain event is an immutable fact that has occurred within the business. They serve as the cornerstone of asynchronous communication between bounded contexts within the HTE platform. Their names are always written in the past tense, using the ubiquitous language of the Huila tourism domain.

What is a domain event?
A domain event communicates that something significant has happened in the business. It is an immutable message describing a completed action or fact.

Correct examples (Events):

AgencyRegistered

TourPlanPublished

BookingRequested

BookingApproved

ReviewSubmitted

Incorrect examples:

RegisterAgency (This is a command, not an event)

UpdatePlan (Too generic — what changed?)

BookingEvent (Does not indicate what happened in the business)

Difference between Command and Event
Command:

Intent: Instruction to perform a business action.

Tense: Present.

Can it fail?: Yes (it can be rejected due to business rules or validations).

Event:

Intent: Notification of a fact that has already occurred.

Tense: Past.

Can it fail?: No (the event has already taken place in the system).

Turista
  │
  │  RequestBooking (comando)
  ▼
[Agregado: Booking]
  │
  │  BookingRequested (evento)
  ├───────────────────────────────────────────┐
  │                                           ▼
  │                                [Servicio: Notificaciones]
  │                                Envía correo inicial al turista y agencia
  │
  │  BookingRequested (evento)
  └───────────────────────────────────────────┐
                                              ▼
                                   [Servicio: Disponibilidad]
                                   Actualiza cupo del calendario de la agencia

Agencia
  │
  │  ApproveBooking (comando)
  ▼
[Agregado: Booking]
  │
  │  BookingApproved (evento)
  └───────────────────────────────────────────┐
                                              ▼
                                   [Servicio: Notificaciones]
                                   Envía correo de confirmación final con itinerario
Event Catalog
Event: Booking Requested
Event Properties:

Name: BookingRequested

Bounded Context: Booking Context

Aggregate: Booking

Trigger: The tourist completes and submits the booking request for a tour plan. Consumers: Notifications Service, Inventory/Availability Service

Channel (Topic): hte.bookings.booking-requested

Schema version: v1

Delivery guarantee: At-least-once

Payload (JSON Schema):

JSON
{
"eventId": "550e8400-e29b-41d4-a716-446655440000",
"eventType": "BookingRequested",
"aggregateId": "b10a8400-e29b-41d4-a716-446655440010",
"aggregateType": "Booking",
"occurredAt": "2026-09-15T10:30:00Z",
"version": 1,
"payload": {
"bookingId": "b10a8400-e29b-41d4-a716-446655440010",
"tourPlanId": "p20a8400-e29b-41d4-a716-446655440020",
"agencyId": "a30a8400-e29b-41d4-a716-446655440030",
"touristName": "Carlos Mendoza",
"touristEmail": "carlos.mendoza@example.com",
"touristPhone": "+573101234567",
"bookingDate": "2026-10-12",
"numberOfPeople": 3,
"totalPrice": 450000.00,
"termsAccepted": true
},
"metadata": {
"correlationId": "c40a8400-e29b-41d4-a716-446655440040",
"causationId": "cmd-req-booking-9988",
"userId": "u50a8400-e29b-41d4-a716-446655440050"
}
}
Consumer actions in response to this event:

Notification Service: Sends a request confirmation email to the tourist and alerts the agency. (It is idempotent: checks the eventId against its history of processed events).

Availability Service: Temporarily blocks the slot on the calendar. (It is idempotent: uses bookingId as the idempotency key).

Event: Booking approved
Event Properties:

Name: BookingApproved

Bounded Context: Booking context

Aggregate: Booking

Trigger: The agency manually approves the booking via its administrative dashboard. Consumers: Notifications Service, Tourist History Service

Channel (Topic): hte.bookings.booking-approved

Schema version: v1

Delivery guarantee: At-least-once

Payload (JSON Schema):

JSON
{
"eventId": "660e8400-e29b-41d4-a716-446655440001",
"eventType": "BookingApproved",
"aggregateId": "b10a8400-e29b-41d4-a716-446655440010",
"aggregateType": "Booking",
"occurredAt": "2026-09-15T11:15:00Z",
"version": 1,
"payload": {
"bookingId": "b10a8400-e29b-41d4-a716-446655440010",
"agencyId": "a30a8400-e29b-41d4-a716-446655440030",
"touristEmail": "carlos.mendoza@example.com",
"approvalNotes": "Booking confirmed. Meeting point: San Agustín Central Park, 7:00 AM"
},
"metadata": {
"correlationId": "c40a8400-e29b-41d4-a716-446655440040",
"causationId": "cmd-approve-booking-1122",
"userId": "a30a8400-e29b-41d4-a716-446655440030"
}
}
Standard envelope fields
All events emitted by the system must include the following fields in the envelope:

eventId (UUID): Unique event identifier used for idempotency.

eventType (string): Event name in PascalCase format.

aggregateId (UUID): Identifier of the aggregate that generated the event.

aggregateType (string): Aggregate type (e.g., Booking, TourPlan, Agency).

occurredAt (ISO 8601): Exact UTC timestamp of when the business event took place.

version (integer): Event schema version to manage its evolution.

payload (object): Event data specific to each type.

metadata.correlationId (UUID): Tracking ID to trace the transaction across microservices.

metadata.causationId (UUID): Identifier of the event or command that caused this event.

metadata.userId (UUID): Identifier of the user who initiated the action...
