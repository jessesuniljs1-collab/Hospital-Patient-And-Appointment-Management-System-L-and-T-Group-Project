# P02 — Hospital Patient & Appointment Management System

**Christ University — 5th Semester CIA-3**  
**Course:** Advanced JavaScript Backend Frameworks (Node.js & Express.js)  
**Technology:** Node.js + Express.js + MongoDB + Mongoose  
**Domain:** Healthcare  
**Repository:** [https://github.com/jessesuniljs1-collab/Hospital-Patient-And-Appointment-Management-System-L-and-T-CIA-3-Group-Project.git](https://github.com/jessesuniljs1-collab/Hospital-Patient-And-Appointment-Management-System-L-and-T-CIA-3-Group-Project.git)

---

## Team Members

| Member | Name | Roll No. | Section | Primary Contribution |
|---|---|---|---|---|
| Member 1 | [NAME] | [ROLL NO.] | [SECTION] | Sprint 1 modules (Auth, Doctor & Dept Management, Availability, Booking Engine) |
| Member 2 | [NAME] | [ROLL NO.] | [SECTION] | Sprint 2 modules (Status Workflow, Digital Prescriptions, Medical History, Directory) |
| Member 3 | [NAME] | [ROLL NO.] | [SECTION] | Sprint 3 modules (Notifications, Billing Summary, Doctor Search, Admin Reports, RBAC) |
| Member 4 | [NAME] | [ROLL NO.] | [SECTION] | Integration, Schema Design, Postman Testing, README & PPT Consolidation |

---

## Team Contributions

### Member 1 — Sprint 1 / Foundation
* **Patient Registration & Authentication:** Bcrypt password hashing, JWT token generation, active status checks, medical profile capture.
* **Doctor Profile & Department Management:** Department CRUD, doctor creation with qualifications, consultation fees, and department assignments.
* **Doctor Availability Slots:** Configurable weekly slots with time format validation and start-before-end verification.
* **Appointment Booking Engine:** Double-booking prevention, patient availability conflict detection, and slot validation against doctor working hours.

### Member 2 — Sprint 2 / Core Workflow
* **Appointment Status Workflow:** Deterministic state machine (`Booked` → `Confirmed` → `Completed` / `Cancelled` / `No-show`) rejecting arbitrary changes.
* **Digital Prescription Module:** Multi-drug prescription generation linked strictly to `Completed` appointments with diagnosis and dosage instructions.
* **Patient Medical History:** Chronological patient history combining past appointments, digital prescriptions, and billing summaries.
* **Department & Specialization Directory:** Public directory endpoints filtering specialists without exposing password hashes or private fields.

### Member 3 — Sprint 3 / Reporting & Polish
* **Notifications & Reminders:** Event-driven notification generation for bookings, confirmations, reminders, and prescription dispatches.
* **Billing Summary per Visit:** Consultation billing linked to completed visits with payment status tracking (`Pending`, `Paid`, `Cancelled`).
* **Search Doctors by Specialization:** Search and filter engine by specialization, department name, and availability.
* **Admin Dashboard & Reports:** Multi-stage MongoDB `$group` and `$match` aggregation pipelines calculating daily appointment volumes, department patient loads, and doctor utilization rates.
* **Role-Based Access Control (RBAC):** Centralized authorization middleware enforcing role hierarchies and resource ownership.

### Member 4 — Integration & Documentation
* **Database Schema Design:** Entity relationship modeling, embedding vs. referencing decisions, and compound indexes.
* **Postman Testing:** Comprehensive Postman collection covering happy paths, 400 validation, 401 unauthenticated, 403 forbidden, 404 not-found, and 409 conflict test cases with automated token capture.
* **README / Documentation:** Comprehensive project guide, setup instructions, faculty demo flow, and API reference.
* **PPT Consolidation:** Slide deck preparation and architecture diagrams for presentation.

> The project was developed collaboratively by four team members. Primary ownership was divided according to the P02 project development structure, while all team members are expected to understand the complete system and all modules for the CIA-3 viva.

---

## Project Overview

### Problem Statement
In multi-department hospital environments, manual front-desk processes lead to high patient waiting times, schedule clashes, double-booked doctors, and disjointed medical documentation. Patients often struggle to discover specialist schedules, while doctors lack structured workflows to transition visits into prescriptions, billing records, and clinical logs.

### Objectives
* Automate patient registration with blood group, date of birth, emergency contacts, and medical history capture.
* Eliminate appointment double booking through time-range overlap collision algorithms.
* Enforce a strict appointment lifecycle state machine (`Booked` → `Confirmed` → `Completed` / `Cancelled` / `No-show`).
* Secure patient records and clinical actions using JSON Web Tokens (JWT) and Role-Based Access Control (RBAC).
* Provide doctors with digital prescription authoring linked to completed appointments.
* Generate consultation billing records automatically upon visit completion.
* Provide hospital administrators with real-time MongoDB aggregation reports on department loads and doctor utilization.

### User Roles & Permissions
* **Patient:** Registers, logs in, manages own profile, books appointments, cancels appointments where permitted, views own prescriptions, and reviews permitted medical history.
* **Doctor:** Manages weekly availability slots, views assigned appointments, confirms bookings, marks visits completed, issues digital prescriptions, and accesses permitted patient information.
* **Receptionist / Admin:** Manages departments, manages doctor profiles, performs walk-in bookings on behalf of patients, updates consultation billing statuses, and analyzes hospital-wide aggregation reports.

---

## Exact Required Modules (All 13 Implemented)

1. **Module 1 — Patient Registration & Authentication:** Bcrypt hashing (salt 12), JWT generation, profile capture, protected routes.
2. **Module 2 — Doctor Profile & Department Management:** Department CRUD, doctor creation with qualifications, consultation fees, and department assignments.
3. **Module 3 — Doctor Availability Slots:** Weekly slots with algorithmic same-day overlap rejection (`409 OVERLAPPING_SLOTS`).
4. **Module 4 — Appointment Booking Engine:** Time-range collision detection (`409 APPOINTMENT_CONFLICT`), availability verification, and past-date rejection.
5. **Module 5 — Appointment Status Workflow:** Strict transition validation engine rejecting illegal status jumps (`400 INVALID_STATUS_TRANSITION`).
6. **Module 6 — Digital Prescription Module:** Multi-drug prescription generation linked strictly to `Completed` appointments with diagnosis and dosage instructions.
7. **Module 7 — Patient Medical History:** Chronological patient history combining past appointments, digital prescriptions, and billing summaries with strict ownership enforcement (`403 OWNERSHIP_VIOLATION`).
8. **Module 8 — Department & Specialization Directory:** Public directory endpoints filtering specialists without exposing password hashes or private fields.
9. **Module 9 — Notifications & Reminders:** Event-driven notification generation for bookings, confirmations, reminders, and prescription dispatches.
10. **Module 10 — Billing Summary per Visit:** Consultation billing linked to completed visits with payment status tracking (`Pending`, `Paid`, `Cancelled`).
11. **Module 11 — Search Doctors by Specialization:** Search and filter engine by specialization, department name, and availability.
12. **Module 12 — Admin Dashboard & Reports:** Multi-stage MongoDB `$group` and `$match` aggregation pipelines calculating daily appointment volumes, department patient loads, and doctor utilization rates.
13. **Module 13 — Role-Based Access Control (RBAC):** Centralized authorization middleware enforcing role hierarchies (`Patient`, `Doctor`, `Admin`) and resource ownership.

---

## Technology Stack

* **Runtime Environment:** Node.js (v18+ / v20+ / v22+)
* **Backend Framework:** Express.js (v4.21.0)
* **Database:** MongoDB
* **Object Data Modeling (ODM):** Mongoose (v8.6.0)
* **Authentication:** JSON Web Tokens (`jsonwebtoken` v9.0.2)
* **Password Hashing:** `bcryptjs` (v2.4.3, Salt factor: 12)
* **Validation Middleware:** `express-validator` (v7.2.0)
* **Automated Integration Testing:** Jest (v29.7.0) + Supertest (v7.0.0) + `mongodb-memory-server` (v10.1.0)
* **API Testing & Verification:** Postman Collection v2.1
* **Frontend:** Modern Vanilla HTML5 / CSS3 / JavaScript (Zero external framework dependencies, Google Fonts Plus Jakarta Sans)

---

## Prerequisites

* **Node.js:** v18.0.0 or later (Node v20 or v22 recommended)
* **npm:** v9.0.0 or later
* **MongoDB:** Local installation (`mongodb://localhost:27017`) OR MongoDB Atlas connection string (an automatic embedded fallback engine is included for zero-config offline runs)
* **Git:** Installed on command line
* **Postman:** Optional but recommended for executing the test collection

---

## Cloning the Repository

```bash
git clone https://github.com/jessesuniljs1-collab/Hospital-Patient-And-Appointment-Management-System-L-and-T-CIA-3-Group-Project.git
cd Hospital-Patient-And-Appointment-Management-System-L-and-T-CIA-3-Group-Project
```

---

## Installation

Install all project dependencies:
```bash
npm install
```

This installs express, mongoose, jsonwebtoken, bcryptjs, express-validator, morgan, cors, and devDependencies (jest, supertest, nodemon, mongodb-memory-server).

---

## Environment Setup

Create `.env` from `.env.example`:

* **Windows Command Prompt (CMD):**
  ```cmd
  copy .env.example .env
  ```
* **PowerShell:**
  ```powershell
  Copy-Item .env.example .env
  ```
* **macOS / Linux:**
  ```bash
  cp .env.example .env
  ```

### Environment Variables Description
| Variable | Default Value | Description |
|---|---|---|
| `PORT` | `5000` | Port on which Express server listens |
| `MONGO_URI` | `mongodb://localhost:27017/hospital_management` | MongoDB connection string |
| `JWT_SECRET` | `p02_hospital_jwt_secret_key_2024` | Secret key used to sign and verify JWTs |
| `JWT_EXPIRES_IN` | `1d` | Lifetime of issued authentication tokens |
| `NODE_ENV` | `development` | Environment mode (`development` / `production` / `test`) |

> **Security Note:** Real secrets must never be committed to Git. The `.env` file is excluded via `.gitignore`.

---

## MongoDB Configuration

### Option A: Local MongoDB (Recommended)
1. Ensure the local MongoDB service is running on default port `27017`.
2. Keep `MONGO_URI=mongodb://localhost:27017/hospital_management` in `.env`.

### Option B: MongoDB Atlas (Cloud)
1. Create a free cluster on [MongoDB Atlas](https://www.mongodb.com/cloud/atlas).
2. Create a database user and allow your IP address under Network Access.
3. Obtain the connection string (`mongodb+srv://<username>:<password>@cluster.mongodb.net/hospital_management?retryWrites=true&w=majority`).
4. Update `MONGO_URI` in `.env`.

### Option C: Built-in Zero-Config Fallback
If no external MongoDB daemon is detected on `localhost:27017`, the application automatically launches a lightweight embedded MongoDB engine for evaluation, allowing instant offline execution without external service setup.

---

## Database Seeding

Populate the database with sample departments, doctors, patients, availability schedules, appointments across multiple statuses, prescriptions, billing records, and notifications:

```bash
npm run seed
```

### What Gets Seeded:
* **5 Departments:** Cardiology, Neurology, Orthopedics, Pediatrics, General Medicine.
* **1 Admin:** `admin@hospital.com`
* **2 Doctors:** Dr. Rajesh Sharma (Cardiology) & Dr. Priya Patel (Neurology) with non-overlapping weekly availability slots.
* **2 Patients:** John Doe (O+, mild hypertension) & Sarah Smith (B+, migraine history).
* **Sample Appointments:** Completed visit (with prescription & billing), Confirmed visit, Booked visit, and Cancelled visit.
* **Prescription:** 2 medicines (Amlodipine, Atorvastatin) linked to the Completed visit.
* **Billing:** Consultation fee (₹800, Paid via UPI) linked to the Completed visit.
* **Notifications:** 5 notifications across patient and doctor accounts.

---

## Demo Credentials for Evaluation

All demo passwords are encrypted using bcrypt (salt rounds = 12):

| Role | Name | Email | Password | Scope & Details |
|---|---|---|---|---|
| **Admin** | System Admin | `admin@hospital.com` | `Admin@123` | Hospital management, doctor creation, billing updates, aggregation reports |
| **Doctor** | Dr. Rajesh Sharma | `dr.sharma@hospital.com` | `Doctor@123` | Cardiology specialist, slot management, appointment completion, prescriptions |
| **Doctor** | Dr. Priya Patel | `dr.patel@hospital.com` | `Doctor@123` | Neurology specialist, slot management, appointments |
| **Patient** | John Doe | `patient1@example.com` | `Patient@123` | Patient with completed consultation, prescription, and billing history |
| **Patient** | Sarah Smith | `patient2@example.com` | `Patient@123` | Patient with upcoming booked appointment |

---

## Running the Application

### Start the Server
```bash
npm start
```
Or with live auto-reload during development:
```bash
npm run dev
```

The console will display:
```text
🏥 Hospital Management System API
   Environment: development
   Server running on port 5000
   API Base URL: http://localhost:5000/api
   Health Check: http://localhost:5000/api/health
```

### Access URLs
* **Interactive Frontend:** `http://localhost:5000/`
* **API Health Check:** `http://localhost:5000/api/health`
* **API Base URL:** `http://localhost:5000/api`

---

## Interactive Frontend Overview

The project includes an interactive Single-Page Application (SPA) served directly from the Express backend at `http://localhost:5000/`.

* **1-Click Evaluator Login:** Instant login chips for Admin, Doctors, and Patients on top of the screen.
* **Doctor Directory & Search:** Real-time search by specialization and department with live availability badges.
* **Appointment Booking Modal:** Date picker and start/end time slot selection with doctor schedule notices.
* **Doctor Clinical Desk:** Manage weekly availability slots, confirm bookings, mark visits completed, and issue digital prescriptions.
* **Digital Prescription Authoring:** Multi-medicine dynamic rows (name, dosage, frequency, duration, instructions).
* **Medical History Timeline:** Chronological clinical visits, linked digital prescriptions, and consultation billing summaries.
* **Admin Dashboard:** Real-time MongoDB aggregation metrics displaying total appointments, status breakdowns, department load tables, and doctor utilization rates.
* **Notifications Drawer:** In-app notification viewer with read markers.

---

## API Route Reference

### Authentication (`/api/auth`)
| Method | Endpoint | Access | Description |
|---|---|---|---|
| `POST` | `/api/auth/register` | Public | Register new Patient (or Doctor/Admin if caller is Admin) |
| `POST` | `/api/auth/login` | Public | Authenticate user & receive JWT |
| `GET` | `/api/auth/me` | Protected | Retrieve authenticated user profile |

### Departments (`/api/departments`)
| Method | Endpoint | Access | Description |
|---|---|---|---|
| `GET` | `/api/departments` | Public | List all active departments |
| `GET` | `/api/departments/:id` | Public | Get single department details |
| `POST` | `/api/departments` | Admin | Create a new department |
| `PUT` | `/api/departments/:id` | Admin | Update department name/description |
| `DELETE` | `/api/departments/:id` | Admin | Soft delete department |

### Doctors & Availability (`/api/doctors`)
| Method | Endpoint | Access | Description |
|---|---|---|---|
| `GET` | `/api/doctors` | Public | Search & list doctors (by specialization/dept) |
| `GET` | `/api/doctors/:id` | Public | Get doctor public profile |
| `POST` | `/api/doctors` | Admin | Create doctor profile with user account |
| `PUT` | `/api/doctors/:id` | Doctor / Admin | Update doctor profile details |
| `PUT` | `/api/doctors/:id/slots` | Doctor / Admin | Update availability slots (overlap checked) |
| `DELETE` | `/api/doctors/:id` | Admin | Deactivate doctor profile |

### Appointments & Workflow (`/api/appointments`)
| Method | Endpoint | Access | Description |
|---|---|---|---|
| `POST` | `/api/appointments` | Patient / Admin | Book appointment with conflict checking |
| `GET` | `/api/appointments` | Protected | List appointments (role-filtered) |
| `GET` | `/api/appointments/:id` | Protected | Get appointment details (ownership checked) |
| `PUT` | `/api/appointments/:id/status` | Protected | Advance appointment status via state machine |

### Prescriptions (`/api/prescriptions`)
| Method | Endpoint | Access | Description |
|---|---|---|---|
| `POST` | `/api/prescriptions` | Doctor | Issue prescription for Completed appointment |
| `GET` | `/api/prescriptions/:id` | Protected | Get prescription by ID (ownership checked) |
| `GET` | `/api/prescriptions/appointment/:id` | Protected | Get prescription for an appointment |
| `GET` | `/api/prescriptions/patient/:id` | Protected | Get all prescriptions for a patient |

### Patients & Medical History (`/api/patients`)
| Method | Endpoint | Access | Description |
|---|---|---|---|
| `GET` | `/api/patients/profile` | Patient | Get current patient's profile |
| `PUT` | `/api/patients/profile` | Patient | Update current patient's profile |
| `GET` | `/api/patients` | Admin | List all registered patients |
| `GET` | `/api/patients/:id/history` | Patient (Own) / Admin | Chronological history (Visits, Prescriptions, Billing) |

### Consultation Billing (`/api/billing`)
| Method | Endpoint | Access | Description |
|---|---|---|---|
| `POST` | `/api/billing` | Admin | Create manual billing record |
| `GET` | `/api/billing/:id` | Protected | Get billing record (ownership checked) |
| `PUT` | `/api/billing/:id` | Admin | Update payment status (`Paid`, `Cancelled`) |
| `GET` | `/api/billing/patient/:id` | Protected | Get all billing records for a patient |

### Notifications (`/api/notifications`)
| Method | Endpoint | Access | Description |
|---|---|---|---|
| `GET` | `/api/notifications` | Protected | Get user notifications and unread count |
| `PUT` | `/api/notifications/:id/read` | Protected | Mark single notification as read |
| `PUT` | `/api/notifications/read-all` | Protected | Mark all notifications as read |

### Administrative Reports (`/api/admin/reports`)
| Method | Endpoint | Access | Description |
|---|---|---|---|
| `GET` | `/api/admin/reports/appointments` | Admin Only | Daily/Weekly appointment volume aggregation |
| `GET` | `/api/admin/reports/departments` | Admin Only | Department patient load aggregation |
| `GET` | `/api/admin/reports/doctors` | Admin Only | Doctor utilization & completion rates aggregation |

---

## Critical Business Rules

### 1. Doctor Availability Overlap Rejection
When doctors define weekly availability:
* `startTime` must precede `endTime`.
* Slots on the same weekday cannot overlap:
  $$\text{Overlap} \iff (\text{start}_1 < \text{end}_2) \land (\text{start}_2 < \text{end}_1)$$
  Any detected overlap is rejected with `409 OVERLAPPING_SLOTS`.

### 2. Appointment Booking Collision Prevention
Before creating an appointment:
1. Rejects dates in the past (`400 INVALID_DATE`).
2. Validates that the slot falls within the doctor's availability on that weekday (`400 SLOT_NOT_AVAILABLE`).
3. Checks against active bookings (`Booked` or `Confirmed`) for that doctor:
   $$\text{Conflict} \iff (\text{reqStart} < \text{existingEnd}) \land (\text{existingStart} < \text{reqEnd})$$
   Collisions are rejected with `409 APPOINTMENT_CONFLICT`.
4. Checks for patient-side overlapping bookings.

### 3. Appointment Status Transition State Machine
The lifecycle strictly enforces allowed paths:
```
[Booked] ───────(Doctor/Admin)───────> [Confirmed] ───────(Doctor/Admin)───────> [Completed]
    │                                      │
 (Patient/Doctor/Admin)             (Patient/Doctor/Admin)
    │                                      │
    ▼                                      ▼
[Cancelled]                            [Cancelled]
                                           │
                                     (Doctor/Admin)
                                           │
                                           ▼
                                       [No-show]
```
* `Completed`, `Cancelled`, and `No-show` are terminal states.
* Invalid transitions (e.g. `Completed` → `Booked`, or Patient marking `Completed`) return `400 INVALID_STATUS_TRANSITION` or `403 FORBIDDEN`.

### 4. Digital Prescription Issuance Rules
* Can only be created by an authenticated `Doctor`.
* The target appointment must have status `Completed`.
* The doctor must be the assigned clinician for that visit.
* Only one prescription can be issued per appointment (`409 DUPLICATE_PRESCRIPTION`).

### 5. Medical History Ownership Protection
Patients requesting `/api/patients/:id/history` are verified against the token payload. Attempting to view another patient's medical history returns `403 OWNERSHIP_VIOLATION`.

### 6. Automated Consultation Billing
When an appointment transitions to `Completed`, a `Billing` record is automatically generated with `paymentStatus: 'Pending'` using the doctor's consultation fee.

---

## Database Schema & Mongoose Relationships

```
User (Base Identity)
 ├── 1:1 ──> Patient (Medical profile, blood group, allergies)
 └── 1:1 ──> Doctor (Department, fee, availability slots)

Department
 └── 1:N ──> Doctor (Linked via departmentId)

Patient
 └── 1:N ──> Appointment (Linked via patientId)

Doctor
 └── 1:N ──> Appointment (Linked via doctorId)

Appointment
 ├── 1:1 ──> Prescription (Linked via appointmentId, required Completed status)
 ├── 1:1 ──> Billing (Linked via appointmentId, auto-created upon completion)
 └── 1:N ──> Notification (Event notifications for patient and doctor)
```

### Indexes Implemented
* `users.email`: Unique index for fast authentication lookups.
* `patients.userId`: Unique index mapping Patient profile to User identity.
* `doctors.userId`: Unique index mapping Doctor profile to User identity.
* `doctors.departmentId`: Index for fast filtering of doctors by department.
* `doctors.specialization`: Index for search queries by specialty.
* `departments.name`: Unique index preventing duplicate department titles.
* `appointments.patientId`: Index for fast chronological patient history.
* `appointments.doctorId`: Index for fast doctor schedule queries.
* `appointments.date + appointments.doctorId`: Compound index for fast booking collision queries.
* `prescriptions.appointmentId`: Index for appointment lookup.
* `billing.appointmentId`: Index for billing lookups.
* `notifications.userId + notifications.isRead`: Compound index for user notification drawer queries.

---

## Project Folder Structure

```text
Hospital-Patient-And-Appointment-Management-System-L-and-T-CIA-3-Group-Project/
├── config/
│   ├── db.js                     # Resilient MongoDB connector (auto-embedded fallback)
│   └── env.js                    # Centralized environment configuration
├── models/
│   ├── User.js                   # Base user with role enum & bcrypt pre-save hook
│   ├── Patient.js                # Medical profile, blood group, allergies, address
│   ├── Doctor.js                 # Specialization, consultation fee, embedded slots
│   ├── Department.js             # Hospital departments with unique name index
│   ├── Appointment.js            # Slot times, reason, status enum, cancellation
│   ├── Prescription.js           # Multi-drug prescriptions linked to Completed visits
│   ├── Billing.js                # Consultation billing linked to appointments
│   └── Notification.js           # Event-driven in-app notifications & reminders
├── controllers/
│   ├── authController.js         # Register, Login, GetMe
│   ├── departmentController.js   # Department CRUD
│   ├── doctorController.js       # Doctor profiles, search, availability slots
│   ├── appointmentController.js  # Booking engine, conflict detection, workflow
│   ├── prescriptionController.js # Prescription generation & retrieval
│   ├── patientController.js      # Patient profile & chronological medical history
│   ├── billingController.js      # Consultation billing & payment status updates
│   ├── notificationController.js # Notifications retrieval & mark-as-read
│   └── reportController.js       # MongoDB aggregation reports (Admin)
├── routes/
│   ├── authRoutes.js             # /api/auth
│   ├── departmentRoutes.js       # /api/departments
│   ├── doctorRoutes.js           # /api/doctors
│   ├── appointmentRoutes.js      # /api/appointments
│   ├── prescriptionRoutes.js     # /api/prescriptions
│   ├── patientRoutes.js          # /api/patients
│   ├── billingRoutes.js          # /api/billing
│   ├── notificationRoutes.js     # /api/notifications
│   └── reportRoutes.js           # /api/admin/reports
├── middleware/
│   ├── auth.js                   # JWT Bearer authentication
│   ├── authorize.js              # requireRole() & requirePatientOwnership()
│   ├── validate.js               # express-validator result handler
│   └── errorHandler.js           # Centralized error handler & AppError class
├── validators/
│   ├── authValidator.js          # Registration & login schemas
│   ├── departmentValidator.js    # Department schemas
│   ├── doctorValidator.js        # Doctor create & slot validation schemas
│   ├── appointmentValidator.js   # Slot format & query schemas
│   ├── prescriptionValidator.js  # Medicines array validation schemas
│   ├── billingValidator.js       # Billing update schemas
│   └── patientValidator.js       # Profile update schemas
├── services/
│   └── notificationService.js    # Decoupled notification generator
├── seed/
│   ├── seed.js                   # Standalone seed runner
│   └── seedHelper.js             # Reusable database seeder module
├── scripts/
│   └── verify-api.js             # Live 20-point end-to-end verification script
├── tests/
│   └── api.test.js               # 40 Jest integration tests
├── postman/
│   └── P02-Hospital-System.postman_collection.json # Complete Postman collection
├── public/                       # Frontend Single-Page Interface
│   ├── index.html                # Responsive UI with 1-click evaluator login
│   ├── style.css                 # Custom healthcare design system
│   └── app.js                    # Live API client logic
├── .env.example                  # Environment template
├── .gitignore                    # Git ignore configuration
├── package.json                  # Dependencies & npm scripts
├── package-lock.json             # Dependency lockfile
├── README.md                     # Comprehensive project documentation
└── server.js                     # Express application entrypoint
```

---

## Postman Collection Instructions

A complete, pre-configured Postman collection is included at:
`postman/P02-Hospital-System.postman_collection.json`

### Importing & Running in Postman
1. Open Postman.
2. Click **Import** (top left) and select `postman/P02-Hospital-System.postman_collection.json`.
3. The collection contains 10 folders covering all 13 modules:
   * `1. Authentication` (Admin Login, Doctor Login, Patient Login, Validation errors, 401 Missing token)
   * `2. Departments` (Public list, Admin create, 403 Forbidden check)
   * `3. Doctors & Availability` (Search by spec/dept, Slot updates, 409 Overlap test)
   * `4. Appointments` (Booking, 409 Double booking conflict, 400 Out-of-availability)
   * `5. Status Workflow` (Booked → Confirmed → Completed, 400 Invalid transition)
   * `6. Digital Prescriptions` (Create on Completed visit, 403 Patient role, 409 Duplicate)
   * `7. Medical History & Profile` (Get profile, Update profile, 403 Ownership violation)
   * `8. Billing` (Get billing, Admin update to Paid, 403 Patient update)
   * `9. Notifications` (Get notifications, Mark read)
   * `10. Admin Reports` (Daily/Weekly aggregation, Department load, Doctor utilization, 403 Non-admin)
4. Execute **"1. Authentication -> Login Admin"**, **"Login Doctor"**, and **"Login Patient 1"**. Postman test scripts automatically capture JWT tokens and populate collection variables (`adminToken`, `doctorToken`, `patientToken`).

---

## Automated Testing

Run the automated integration test suite:
```bash
npm test
```

* **Test Framework:** Jest + Supertest with an isolated in-memory database instance.
* **Test Count:** 40 integration tests covering all 13 modules.
* **Results:** 40 passed, 0 failed (100% pass rate).

### Live API Verification Script
With the server running (`npm start`), execute the live 20-point verification test:
```bash
npm run verify
```
This tests real HTTP requests against `http://localhost:5000/api` for authentication, booking collisions, state transitions, prescriptions, medical history ownership, billing, and reports.

---

## Troubleshooting Guide

| Issue | Root Cause | Solution |
|---|---|---|
| `MongoDB Connection Error: connect ECONNREFUSED` | Local MongoDB service is not started | Start MongoDB service or let the application automatically use the built-in embedded MongoDB fallback. |
| `EADDRINUSE: address already in use :::5000` | Another process is occupying port 5000 | Change `PORT=5001` in `.env` or terminate the occupying process. |
| `AUTH_TOKEN_EXPIRED` (401) | JWT has expired after 1 day | Call `POST /api/auth/login` to obtain a fresh token. |
| `OWNERSHIP_VIOLATION` (403) | Patient tried to view another patient's medical history | Ensure the authenticated user matches the patient ID requested in the URL. |
| `APPOINTMENT_CONFLICT` (409) | Double booking attempt | Select an appointment time slot that does not overlap with an existing booking. |
| `OVERLAPPING_SLOTS` (409) | Doctor availability slots overlap on the same weekday | Ensure each availability slot on a given weekday has distinct, non-overlapping start/end times. |
| `INVALID_STATUS_TRANSITION` (400) | Attempted illegal jump (e.g. `Completed` → `Booked`) | Follow the valid state machine: `Booked` → `Confirmed` → `Completed`. |
| Database has no data | Database has not been seeded | Run `npm run seed` to populate all demo records. |

---

## Suggested Faculty Demonstration Flow

1. **Login as Admin:** Click the `Admin` button on the top banner (`admin@hospital.com` / `Admin@123`).
2. **Review Departments:** Navigate to the **Departments** tab to view the 5 active departments.
3. **Review Doctors & Availability:** Navigate to the **Doctors** tab; search by specialization `Cardiology` to see Dr. Rajesh Sharma and his weekly availability slots.
4. **Login as Patient:** Click the `John Doe (Patient 1)` button (`patient1@example.com` / `Patient@123`).
5. **Book an Appointment:** Click **+ Book New Appointment**, select Dr. Sharma, pick a valid date/time within his Monday availability (e.g., 09:30 - 10:00), and book.
6. **Demonstrate Conflict Detection:** Attempt to book an overlapping slot with the same doctor (e.g., 09:45 - 10:15) with Patient 2; observe the immediate `409 APPOINTMENT_CONFLICT` rejection toast.
7. **Login as Doctor:** Click the `Dr. Sharma` button (`dr.sharma@hospital.com` / `Doctor@123`) and open the **Doctor Desk** tab.
8. **Advance Workflow (Confirm):** Click **Confirm Booking** on the newly booked appointment. Status transitions to `Confirmed`.
9. **Advance Workflow (Complete):** Click **Mark Completed** after consultation. Status transitions to `Completed`.
10. **Issue Digital Prescription:** Click **Issue Prescription**, add medication details (e.g., Metoprolol, 25mg, Daily), and submit.
11. **Verify Automated Billing:** Switch back to **Patient 1**; open the **Medical History** tab to view the completed visit with the issued prescription and auto-generated billing record.
12. **Demonstrate Ownership Protection:** Try accessing Patient 1's history with Patient 2's token via API; observe the `403 OWNERSHIP_VIOLATION` block.
13. **Admin Reports:** Switch to **Admin**; open the **Admin & Reports** tab to view live MongoDB aggregation graphs and tables for department loads and doctor utilization.

---

## Final Project Statement

This repository contains the complete source code for the P02 Hospital Patient & Appointment Management System developed as a four-member CIA-3 group project. The repository includes the backend implementation, MongoDB/Mongoose models, authentication and authorization, business workflows, frontend, automated tests, seed data, Postman collection, and setup documentation. The project can be cloned and configured using the instructions provided in this README.
