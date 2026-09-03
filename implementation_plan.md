# P02 — Hospital Patient & Appointment Management System

Full backend system for a multi-department hospital: patient registration, doctor availability, appointment scheduling, prescriptions, medical history, billing, notifications, reports, and RBAC.

## Proposed Changes

### Phase 1 — Foundation (config, models, auth, middleware)

#### [NEW] `package.json`, `server.js`, `.env.example`, `.gitignore`
- Express app with MongoDB connection via Mongoose
- Environment: `PORT`, `MONGO_URI`, `JWT_SECRET`, `JWT_EXPIRES_IN`

#### [NEW] `config/db.js`, `config/env.js`
- Mongoose connection with error handling
- Centralized env config

#### [NEW] Models (8 files)
| Model | Collection | Key Fields | Indexes |
|---|---|---|---|
| `User` | users | name, email, passwordHash, role, phone | `email` (unique) |
| `Patient` | patients | userId (ref User), dob, gender, bloodGroup, medicalNotes | `userId` (unique) |
| `Doctor` | doctors | userId (ref User), departmentId (ref Department), specialization | `userId` (unique) |
| `Department` | departments | name, description | `name` (unique) |
| `Appointment` | appointments | patientId, doctorId, date, slot {start,end}, status, reason | `patientId`, `doctorId` |
| `Prescription` | prescriptions | appointmentId, doctorId, patientId, medicines[], notes, issuedAt | `appointmentId` |
| `Billing` | billing | appointmentId, patientId, amount, paymentStatus | `appointmentId` |
| `Notification` | notifications | userId, appointmentId, type, message, scheduledTime, status | `userId` |

**Relationships** — all via ObjectId references (no deep embedding of shared entities):
```
User ──1:1──> Patient
User ──1:1──> Doctor
Department ──1:N──> Doctor
Patient ──1:N──> Appointment
Doctor ──1:N──> Appointment
Appointment ──1:1──> Prescription
Appointment ──1:1──> Billing
Appointment ──1:N──> Notification
```

#### [NEW] Middleware (4 files)
- `auth.js` — JWT verification, attaches `req.user`
- `authorize.js` — `requireRole(...roles)` factory + `requireOwnership` helper
- `validate.js` — express-validator runner middleware
- `errorHandler.js` — centralized error handler (validation, cast, duplicate key, business-rule, 500)

---

### Phase 2 — Auth, Doctors, Departments, Availability, Appointments

#### [NEW] `controllers/authController.js`, `routes/authRoutes.js`, `validators/authValidator.js`
- `POST /api/auth/register` — register Patient (default) or Doctor/Admin
- `POST /api/auth/login` — bcrypt compare → JWT
- `GET /api/auth/me` — current user profile

#### [NEW] `controllers/doctorController.js`, `routes/doctorRoutes.js`, `validators/doctorValidator.js`
- CRUD for doctor profiles (Admin only for create/update/delete)
- `GET /api/doctors` — search by specialization, department, availability (public)
- `PUT /api/doctors/:id/slots` — doctor manages own availability
- Availability model: `availabilitySlots[{ day, startTime, endTime }]` embedded in Doctor
- **Overlap detection**: reject slots where same day has overlapping time ranges

#### [NEW] `controllers/departmentController.js`, `routes/departmentRoutes.js`, `validators/departmentValidator.js`
- CRUD (Admin for CUD, public read)

#### [NEW] `controllers/appointmentController.js`, `routes/appointmentRoutes.js`, `validators/appointmentValidator.js`
- `POST /api/appointments` — book (Patient or Admin)
- `GET /api/appointments` — filtered by role (own for Patient/Doctor, all for Admin)
- `GET /api/appointments/:id`
- `PUT /api/appointments/:id/status` — **status-transition engine**
- `DELETE /api/appointments/:id` — cancel where allowed

**Conflict detection**: Before booking, query existing appointments for same doctor + date + overlapping time window with non-terminal status → reject with 409.

**Status transition map**:
```
Booked    → Confirmed ✅  (Doctor/Admin)
Booked    → Cancelled ✅  (Patient/Admin)
Confirmed → Completed ✅  (Doctor)
Confirmed → Cancelled ✅  (Patient/Admin)
Confirmed → No-show   ✅  (Admin)
```
All other transitions → 400.

---

### Phase 3 — Prescriptions, History, Directory

#### [NEW] `controllers/prescriptionController.js`, `routes/prescriptionRoutes.js`, `validators/prescriptionValidator.js`
- `POST /api/prescriptions` — Doctor only, appointment must be `Completed`
- `GET /api/prescriptions/:id` — Doctor/Patient (ownership)
- `GET /api/prescriptions/appointment/:appointmentId`
- `GET /api/prescriptions/patient/:patientId` — Doctor/Admin/own Patient

#### [NEW] `controllers/patientController.js`, `routes/patientRoutes.js`
- `GET /api/patients/:id/history` — chronological visits + prescriptions + billing
- `GET /api/patients/profile` — own profile
- `PUT /api/patients/profile` — update own profile
- Ownership enforcement: Patient can only see own history

---

### Phase 4 — Notifications, Billing, Reports

#### [NEW] `controllers/notificationController.js`, `routes/notificationRoutes.js`
- Auto-generate reminders on appointment creation/status change
- `GET /api/notifications` — own notifications
- `PUT /api/notifications/:id/read` — mark read

#### [NEW] `controllers/billingController.js`, `routes/billingRoutes.js`, `validators/billingValidator.js`
- Auto-create billing on appointment completion (or manual Admin create)
- `GET /api/billing/:id`
- `PUT /api/billing/:id` — Admin updates paymentStatus
- `GET /api/billing/patient/:patientId`

#### [NEW] `controllers/reportController.js`, `routes/reportRoutes.js`
- `GET /api/admin/reports/appointments` — daily/weekly counts (aggregation pipeline, `?period=daily|weekly`)
- `GET /api/admin/reports/departments` — appointment count per department
- `GET /api/admin/reports/doctors` — utilization per doctor
- Admin only

---

### Phase 5 — Seed, Postman, Tests, README, Frontend

#### [NEW] `seed/seed.js`
Demo data (all passwords bcrypt-hashed):

| Role | Email | Password |
|---|---|---|
| Admin | admin@hospital.com | Admin@123 |
| Doctor | dr.sharma@hospital.com | Doctor@123 |
| Doctor | dr.patel@hospital.com | Doctor@123 |
| Patient | patient1@example.com | Patient@123 |
| Patient | patient2@example.com | Patient@123 |

Plus departments (Cardiology, Neurology, Orthopedics, Pediatrics, General Medicine), availability slots, sample appointments across statuses, prescriptions, billing, notifications.

#### [NEW] `postman/P02-Hospital-System.postman_collection.json`
12 folders, all endpoints, environment variables (`baseUrl`, `adminToken`, `doctorToken`, `patientToken`), auto-capture tokens from login, test scripts for status codes.

#### [NEW] `tests/` (Jest + Supertest)
Integration tests for auth, RBAC, appointment conflicts, status transitions, prescription restrictions, ownership.

#### [NEW] `README.md`
Full documentation per spec (schema, API table, business rules, setup, seed, credentials).

#### [NEW] `public/` — Optional minimal frontend
Single-page HTML/CSS/JS dashboards for login, patient, doctor, admin views. Calls real backend APIs. Clean modern UI.

---

## Key Design Decisions

1. **Doctor availability** embedded as array in Doctor document (small, doctor-owned, no cross-collection queries needed).
2. **Appointment slot** stored as `{ date, startTime, endTime }` — conflict detection via range overlap query on same doctor+date.
3. **Status transitions** enforced via a `VALID_TRANSITIONS` map in a service function — not raw assignment.
4. **Prescription** requires appointment status `Completed` — enforced in controller.
5. **Billing** auto-created when appointment moves to `Completed`, with status `Pending`.
6. **Notifications** auto-created as records on key events (booking, confirmation, reminders).
7. **Reports** use `$group` aggregation, not `$lookup` chains.

## Verification Plan

### Automated Tests
```bash
npm test          # Jest + Supertest integration tests
```

### Manual Verification
```bash
npm run seed      # Seed database with demo data
npm run dev       # Start server
# Import Postman collection → run all requests
```

I will run the server, execute seed, and test key flows via the API before declaring complete.
