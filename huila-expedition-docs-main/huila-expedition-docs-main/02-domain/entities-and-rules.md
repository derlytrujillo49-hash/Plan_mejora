# Domain Entities and Business Rules

## 1. Core Entities

### Agency (Provider)
* **Attributes:** `id`, `trade_name`, `nit` (Tax ID), `rnt` (National Tourism Registry), `email`, `phone`, `address`, `municipality`, `social_media` (JSON/links), `status` (*Pending*, *Approved*, *Inactive*).
* **Description:** Entity representing the local travel agency responsible for publishing and managing tourism packages.

### Tourism Plan (Service)
* **Attributes:** `id`, `agency_id`, `title`, `description`, `itinerary`, `destination_municipality`, `tourism_type` (category), `duration`, `image_gallery` (max. 10 images), `status` (*Active*, *Inactive*).
* **Description:** Tourism offer or package published by an agency for browsing and comparison.

### Rate
* **Attributes:** `id`, `plan_id`, `user_type` (*Adult*, *Child*, *Group* / *Season*), `price` (positive amount), `conditions`.
* **Description:** Configuration of differentiated costs for purchasing a tourism plan.

### Availability Calendar
* **Attributes:** `id`, `plan_id`, `date`, `max_capacity`, `reserved_spots`, `date_status` (*Available*, *Unavailable*, *Full*).
* **Description:** Daily tracking of offered capacity/inventory to prevent overbooking.

### Booking
* **Attributes:** `id`, `tourist_id`, `plan_id`, `service_date`, `number_of_people`, `total_amount`, `booking_status` (*Pending*, *Approved*, *Cancelled*), `accepted_terms` (boolean), `acceptance_timestamp`. * **Description:** A booking request submitted by a tourist to secure a spot in a specific plan.

### Review and Rating
* **Attributes:** `id`, `booking_id`, `tourist_id`, `plan_id`, `rating` (1 to 5 stars), `comment`, `moderation_status` (*Pending*, *Approved*, *Rejected*).
* **Description:** An evaluation left by the customer after completing their tourism experience.

---

## 2. Business Rules

* **RN-01 (RNT Verification):** Every newly registered agency must enter its National Tourism Registry (RNT) number and remain in *Pending Approval* status until the platform administrator validates its authenticity.

* **RN-02 (Deletion of Plans with Bookings):** An agency cannot delete a tourism plan that has active bookings (*Pending* or *Approved*). The system must issue an alert blocking the action.

* **RN-03 (Overselling / Overbooking Prevention):** When a booking request is registered, the system must check available capacity in the `Availability Calendar` in real-time. If capacity is exhausted, the date will automatically be marked as *Full*, and new requests will be rejected.

* **RN-04 (Authentication and Security Lockouts):** After 5 consecutive failed login attempts, the account will be temporarily locked for 15 minutes. Sessions will expire due to inactivity after 30 minutes.

* **RN-05 (Legal Acceptance of Terms / Data Protection):** No booking can be processed without explicit acceptance of the Terms and Conditions and the Personal Data Protection Policy (Law 1581 of 2012). The system must store the timestamp (exact date and time) of the acceptance. * **RN-06 (Review Eligibility):** Only tourists with a previously *Approved* and completed booking may rate and leave comments on a tour package.

* **RN-07 (Content Moderation):** All user-submitted reviews undergo a manual moderation process by the Administrator to prevent offensive language or spam before becoming public.

* **RN-08 (Image Optimization and Upload):** Each tour package allows the upload of up to 10 images in valid formats (JPG, PNG, WEBP). The system must automatically compress the images, reducing their final size to a maximum of 500 KB without a perceptible loss in quality.
