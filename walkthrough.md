# Walkthrough — P02 Hospital Patient & Appointment Management System

The complete **P02 — Hospital Patient & Appointment Management System** has been fully implemented, seeded, tested, and verified in the workspace for the **Christ University 5th Semester CIA-3 Advanced JavaScript Backend Frameworks** evaluation.

---

## 1. Project Overview & Highlights

- **Framework & Runtime:** Node.js v22 & Express.js v4.21
- **Database & ODM:** MongoDB & Mongoose v8.6
- **Security:** JWT token authentication, `bcryptjs` 12-salt password hashing, role & ownership checking
- **Validation:** Server-side request validation with `express-validator` on every endpoint
- **All 13 Modules Implemented:** Zero dummy endpoints, zero placeholders
- **Test Suite:** 40/40 automated integration tests passing (Jest + Supertest)
- **Live Verification:** 20/20 end-to-end HTTP API tests passing against active server
- **Interactive UI:** Complete single-page frontend with 1-click evaluator login

---

## 2. Directory & File Structure

```text
Hospital-Patient-And-Appointment-Management-System-L-and-T-CIA-3-Group-Project/
├── config/
│   ├── db.js                     # Resilient MongoDB connector (auto-embedded fallback)
│   └── env.js                    # Centralized environment variable management
├── models/
│   ├── User.js                   # Base user with role enum & bcrypt pre-save hook
│   ├── Patient.js                # Medical profile, blood group, allergies
│   ├── Doctor.js                 # Specialization, consultation fee, availability slots
│   ├── Department.js             # Hospital medical departments
│   ├── Appointment.js            # Slot times, reason, status enum, cancellation
│   ├── Prescription.js           # Multi-drug prescriptions linked to Completed visits
│   ├── Billing.js                # Consultation billing per appointment
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
├── .env                          # Local environment variables
├── .env.example                  # Environment template
├── .gitignore                    # Git ignore configuration
├── package.json                  # Dependencies & npm scripts
├── README.md                     # Comprehensive university project documentation
└── server.js                     # Express application entrypoint
```

---

## 3. All 13 P02 Modules Implementation Status

| # | Module Name | Implementation Details | Status |
|---|---|---|:---:|
| 1 | **Patient Registration & Authentication** | Bcrypt hashing, JWT generation, DOB, gender, blood group, protected routes | **Complete** |
| 2 | **Doctor Profile & Department Management** | Admin CRUD, doctor-to-department assignment, public listings | **Complete** |
| 3 | **Doctor Availability Slots** | Start < End validation, same-day slot overlap rejection (`409 OVERLAPPING_SLOTS`) | **Complete** |
| 4 | **Appointment Booking Engine** | Double booking prevention, doctor availability verification, past date rejection (`409 APPOINTMENT_CONFLICT`) | **Complete** |
| 5 | **Appointment Status Workflow** | State machine: `Booked` → `Confirmed` → `Completed` / `Cancelled` / `No-show`. Invalid transitions rejected (`400 INVALID_STATUS_TRANSITION`) | **Complete** |
| 6 | **Digital Prescription Module** | Multi-drug prescription issued by doctor linked strictly to `Completed` appointment | **Complete** |
| 7 | **Patient Medical History** | Chronological record of visits, prescriptions, and billing with strict ownership enforcement (`403 OWNERSHIP_VIOLATION`) | **Complete** |
| 8 | **Department & Specialization Directory** | Public read access filtering doctors and departments without leaking sensitive fields | **Complete** |
| 9 | **Notifications & Reminders** | In-app notification records generated on booking, confirmation, and prescription dispatch | **Complete** |
| 10 | **Billing Summary per Visit** | Auto-generated on visit completion (`Pending`), updated by Admin (`Paid`) | **Complete** |
| 11 | **Search Doctors by Specialization** | Query filtering by `specialization`, `department`, and `available` | **Complete** |
| 12 | **Admin Dashboard & Reports** | MongoDB aggregation pipelines (`$group`, `$match`) for daily counts, department load, and doctor utilization | **Complete** |
| 13 | **Role-Based Access Control (RBAC)** | Strict middleware enforcement for `Patient`, `Doctor`, and `Admin` | **Complete** |

---

## 4. Test Verification Results

### Automated Integration Test Suite (`npm test`)
Ran 40 integration tests using Jest + Supertest:

```text
PASS tests/api.test.js (8.993 s)
  1. Authentication & User Registration (Module 1)
    √ should register an initial Admin user successfully (201)
    √ should reject registration with duplicate email (409)
    √ should register Patient 1 successfully with medical profile (201)
    √ should register Patient 2 successfully (201)
    √ should reject registration on invalid input (400 VALIDATION_ERROR)
    √ should login successfully with valid credentials (200)
    √ should reject login with wrong password (401)
    √ should reject request when token is missing on protected route (401)
  2. Departments & Specialization Directory (Module 2 & 8)
    √ should create a department as Admin (201)
    √ should create Neurology department as Admin (201)
    √ should reject department creation from Patient (403 Forbidden)
    √ should list departments publicly without authentication (200)
  3. Doctor Profiles & Availability Slots (Module 2, 3 & 11)
    √ should create Doctor 1 as Admin (201)
    √ should update doctor availability slots successfully (200)
    √ should REJECT overlapping availability slots on the same day (409 OVERLAPPING_SLOTS)
    √ should REJECT invalid slot times where start >= end (400 INVALID_SLOT_TIME)
    √ should search doctors by specialization (public 200)
  4. Appointment Booking & Conflict Detection (Module 4)
    √ should book an appointment within doctor availability (201)
    √ should REJECT double booking for overlapping slot with same doctor (409 APPOINTMENT_CONFLICT)
    √ should REJECT booking outside doctor availability (400 SLOT_NOT_AVAILABLE)
    √ should REJECT booking on past date (400 INVALID_DATE)
  5. Appointment Status Workflow (Module 5)
    √ should transition Booked → Confirmed by Doctor (200)
    √ should transition Confirmed → Completed by Doctor (200)
    √ should REJECT invalid transition Completed → Booked (400 INVALID_STATUS_TRANSITION)
    √ should REJECT invalid transition Completed → Confirmed (400)
  6. Digital Prescription Module (Module 6)
    √ should create prescription for Completed appointment (Doctor only 201)
    √ should REJECT prescription creation by Patient (403 Forbidden)
    √ should REJECT duplicate prescription for the same appointment (409 DUPLICATE_PRESCRIPTION)
  7. Patient Medical History & Ownership Protection (Module 7 & 13)
    √ should allow Patient to retrieve their own profile and ID (200)
    √ should allow Patient to view their own medical history (200)
    √ should REJECT Patient 2 trying to access Patient 1 history (403 OWNERSHIP_VIOLATION)
  8. Billing & Consultation Payments (Module 10)
    √ should have auto-created billing on appointment completion (200)
    √ should allow Admin to update billing payment status to Paid (200)
    √ should REJECT Patient updating billing status (403 Forbidden)
  9. Notifications & Reminders (Module 9)
    √ should retrieve notifications for patient (200)
    √ should mark all notifications as read (200)
  10. Admin Reports with Aggregation (Module 12)
    √ should generate appointment statistics report for Admin (200)
    √ should generate department load report for Admin (200)
    √ should generate doctor utilization report for Admin (200)
    √ should REJECT doctor or patient accessing admin reports (403 Forbidden)

Test Suites: 1 passed, 1 total
Tests:       40 passed, 40 total
Time:        9.101 s
```

### Live Server End-to-End Verification (`npm run verify`)
Ran against `http://localhost:5000/api`:

```text
🏥 ====================================================
   P02 HOSPITAL MANAGEMENT SYSTEM — LIVE API VERIFICATION
====================================================

  ✅ [PASS] Health Check API returns 200 & success:true
  ✅ [PASS] Admin Login returns 200 & JWT token
  ✅ [PASS] Doctor Login returns 200 & JWT token
  ✅ [PASS] Patient 1 Login returns 200 & JWT token
  ✅ [PASS] Patient 2 Login returns 200 & JWT token
  ✅ [PASS] Public Departments listing returns 5 seeded departments
  ✅ [PASS] Doctor search by specialization returns cardiology specialists
  ✅ [PASS] Overlapping availability slots rejected with 409 OVERLAPPING_SLOTS
  ✅ [PASS] Appointment booking succeeds (201 Booked)
  ✅ [PASS] Double booking rejected with 409 APPOINTMENT_CONFLICT
  ✅ [PASS] Status transition Booked -> Confirmed succeeds (200)
  ✅ [PASS] Status transition Confirmed -> Completed succeeds (200)
  ✅ [PASS] Invalid transition Completed -> Booked rejected with 400 INVALID_STATUS_TRANSITION
  ✅ [PASS] Doctor issues prescription for Completed visit (201)
  ✅ [PASS] Patient views own medical history (200 with visits & prescriptions)
  ✅ [PASS] Ownership violation rejected with 403 OWNERSHIP_VIOLATION
  ✅ [PASS] Billing record automatically generated on visit completion (Pending status)
  ✅ [PASS] Notifications retrieved for patient events
  ✅ [PASS] Admin retrieves appointment aggregation reports (200)
  ✅ [PASS] Non-Admin blocked from reports with 403 FORBIDDEN

====================================================
Verification Summary: 20 PASSED, 0 FAILED
====================================================
```

---

## 5. Demo Seed Credentials

| Role | Name | Email | Password | Notes |
|---|---|---|---|---|
| **Admin** | System Admin | `admin@hospital.com` | `Admin@123` | Full admin & aggregation reports access |
| **Doctor** | Dr. Rajesh Sharma | `dr.sharma@hospital.com` | `Doctor@123` | Cardiology Specialist |
| **Doctor** | Dr. Priya Patel | `dr.patel@hospital.com` | `Doctor@123` | Neurology Specialist |
| **Patient** | John Doe | `patient1@example.com` | `Patient@123` | Has completed visit, prescription, and billing |
| **Patient** | Sarah Smith | `patient2@example.com` | `Patient@123` | Has upcoming booked appointment |

---

## 6. How to Run

```bash
# 1. Install dependencies
npm install

# 2. Run automated test suite
npm test

# 3. Seed database
npm run seed

# 4. Start the server
npm start
# Server will be running at http://localhost:5000

# 5. Run live end-to-end verification
npm run verify
```

The application is fully configured and ready for Christ University CIA-3 grading!
