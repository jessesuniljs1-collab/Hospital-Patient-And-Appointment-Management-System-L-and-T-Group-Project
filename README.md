# Hospital Patient & Appointment Management System

A full-stack hospital patient and appointment management system built with Node.js, Express.js, MongoDB, and Mongoose. It provides patient registration, doctor management, appointment scheduling, availability management, digital prescriptions, medical history, billing, notifications, doctor search, reporting, and role-based access control.

---

## Features

- **Patient Registration & Authentication:** Secure patient registration and login using bcrypt password hashing and JSON Web Tokens (JWT).
- **Doctor & Department Management:** Hospital departments and doctor profiles with qualifications, experience, and consultation fees.
- **Weekly Doctor Availability:** Day-of-week working hours per doctor, with internal validation preventing overlapping slots on the same day.
- **Conflict-Free Appointment Booking:** Booking engine with real-time overlap detection preventing double bookings for doctors.
- **Appointment Status Lifecycle:** Controlled status progression (`Booked` → `Confirmed` → `Completed` / `Cancelled` / `No-show`).
- **Digital Prescriptions:** Electronic prescription records linked to completed appointments with medication details, dosage, and diagnosis.
- **Patient Medical History:** Protected chronological history of appointments and prescriptions with user ownership checks.
- **Department & Doctor Directory:** Publicly browsable department listings and doctor search filtered by specialization or department.
- **Notifications & Reminders:** Event-driven in-app notifications generated for bookings, confirmations, cancellations, completions, and prescriptions.
- **Consultation Billing:** Automatic billing record creation upon appointment completion, supporting payment status updates.
- **Administrative Reports:** MongoDB aggregation reports summarizing appointment metrics, department workloads, and doctor utilization.
- **Role-Based Access Control (RBAC):** Middleware-level authorization protecting routes for `Patient`, `Doctor`, and `Admin` roles.
- **Interactive Web Interface:** Single-page frontend with responsive layouts, modal workflows, and demo switching tools.
- **Integration Test Suite:** 40 automated Jest integration tests covering all critical backend services and business rules.
- **Postman API Collection:** Pre-configured collection for testing and verifying all API endpoints.

---

## User Roles

| Role | Permissions & Capabilities |
| :--- | :--- |
| **Patient** | • Create an account and manage profile details<br>• Search doctors by specialization and department<br>• View doctor availability and book appointments without conflicts<br>• View personal appointments and cancel upcoming bookings<br>• View personal prescriptions and medical history<br>• View billing records and notifications |
| **Doctor** | • Manage weekly availability slots (weekday, start time, end time)<br>• View assigned patient appointments<br>• Confirm and complete appointments<br>• Issue digital prescriptions for completed visits<br>• View permitted patient medical details |
| **Admin** | • Manage hospital departments (create, update, delete)<br>• Manage doctor profiles and assignments<br>• View all hospital appointments<br>• Update billing payment statuses (`Pending` → `Paid`)<br>• View administrative analytics reports |

---

## Quick Start

```bash
# Clone the repository
git clone https://github.com/jessesuniljs1-collab/Hospital-Patient-And-Appointment-Management-System-L-and-T-Group-Project.git
cd Hospital-Patient-And-Appointment-Management-System-L-and-T-Group-Project

# Install dependencies
npm install

# Configure environment variables
# Windows (CMD): copy .env.example .env
# Windows (PowerShell): Copy-Item .env.example .env
# macOS / Linux: cp .env.example .env

# Seed demonstration data
npm run seed

# Start the application
npm start
```

Access points:
- **Frontend Application:** [http://localhost:5000/](http://localhost:5000/)
- **API Base URL:** [http://localhost:5000/api](http://localhost:5000/api)
- **Health Endpoint:** [http://localhost:5000/api/health](http://localhost:5000/api/health)

---

## Requirements

- **Node.js:** v18.0.0 or higher
- **npm:** v9.0.0 or higher
- **MongoDB:** v5.0+ or the built-in development fallback
- **Git:** v2.30 or higher

---

## Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/jessesuniljs1-collab/Hospital-Patient-And-Appointment-Management-System-L-and-T-Group-Project.git
   cd Hospital-Patient-And-Appointment-Management-System-L-and-T-Group-Project
   ```

2. Install dependencies:
   ```bash
   npm install
   ```

---

## Environment Variables

Copy `.env.example` to `.env` and configure variables as needed:

| Variable | Default Value | Description |
| :--- | :--- | :--- |
| `PORT` | `5000` | Port on which the HTTP server listens |
| `MONGO_URI` | `mongodb://localhost:27017/hospital_management` | MongoDB connection connection string |
| `JWT_SECRET` | `your_jwt_secret_key_change_in_production` | Secret key for signing JSON Web Tokens |
| `JWT_EXPIRES_IN` | `1d` | Expiration window for issued JWT tokens |
| `NODE_ENV` | `development` | Application environment (`development` or `production`) |

---

## Database Setup

The application connects to MongoDB using `MONGO_URI` defined in `.env`.

- **Standard MongoDB:** Connects to any local or remote MongoDB instance (e.g., `mongodb://localhost:27017/hospital_management`).
- **Development Fallback:** In development mode (`NODE_ENV=development`), if no running MongoDB server is detected at `MONGO_URI`, `config/db.js` automatically initializes an embedded in-memory MongoDB instance (`mongodb-memory-server`). This allows local testing without a pre-installed database service.

---

## Seed Data

Populate the database with sample departments, doctors, availability schedules, patients, visits, prescriptions, billing records, and notifications:

```bash
npm run seed
```

### Demo Accounts

| Role | Email | Password | Details |
| :--- | :--- | :--- | :--- |
| **Admin** | `admin@hospital.com` | `Admin@123` | System Administrator |
| **Doctor** | `dr.sharma@hospital.com` | `Doctor@123` | Dr. Rajesh Sharma (Cardiology) |
| **Doctor** | `dr.patel@hospital.com` | `Doctor@123` | Dr. Priya Patel (Neurology) |
| **Patient** | `patient1@example.com` | `Patient@123` | John Doe |
| **Patient** | `patient2@example.com` | `Patient@123` | Sarah Smith |

---

## Demo

Follow this workflow to test the end-to-end system:

1. **Start the server:** Run `npm start` and open [http://localhost:5000/](http://localhost:5000/).
2. **Search doctors:** Browse specialists on the landing page or filter by specialization.
3. **Sign in as Patient:** Click `🧑 John Doe (Patient 1)` in the quick demo banner.
4. **Book an appointment:** Choose a doctor, pick a date and an available time slot, and submit.
5. **Sign in as Doctor:** Switch to `🩺 Dr. Sharma (Cardio)`, go to **Doctor Desk**, confirm the booking, and mark it `Completed`.
6. **Issue prescription:** In Doctor Desk, enter medications and diagnosis for the completed visit.
7. **View patient history & billing:** Switch back to Patient to review the issued prescription, updated medical history, and generated consultation invoice.
8. **Admin reports & billing:** Switch to `🔑 Admin` to view aggregation analytics under **Admin & Reports** and mark billing as `Paid`.

---

## API Documentation

Protected routes require: `Authorization: Bearer <token>`

### Authentication (`/api/auth`)
| Method | Endpoint | Access | Description |
| :--- | :--- | :--- | :--- |
| `POST` | `/api/auth/register` | Public | Register new patient account |
| `POST` | `/api/auth/login` | Public | Authenticate user and receive JWT |
| `GET` | `/api/auth/me` | Authenticated | Get current authenticated user |

### Departments (`/api/departments`)
| Method | Endpoint | Access | Description |
| :--- | :--- | :--- | :--- |
| `GET` | `/api/departments` | Public | List active hospital departments |
| `GET` | `/api/departments/:id` | Public | Get department details |
| `POST` | `/api/departments` | Admin | Create hospital department |
| `PUT` | `/api/departments/:id` | Admin | Update department details |
| `DELETE` | `/api/departments/:id` | Admin | Deactivate department |

### Doctors (`/api/doctors`)
| Method | Endpoint | Access | Description |
| :--- | :--- | :--- | :--- |
| `GET` | `/api/doctors` | Public | Search doctors by specialization/department |
| `GET` | `/api/doctors/:id` | Public | Get doctor profile and availability |
| `POST` | `/api/doctors` | Admin | Create doctor profile |
| `PUT` | `/api/doctors/:id` | Admin / Doctor | Update doctor profile |
| `PUT` | `/api/doctors/:id/availability` | Admin / Doctor | Update doctor weekly availability slots |
| `DELETE` | `/api/doctors/:id` | Admin | Remove doctor profile |

### Appointments (`/api/appointments`)
| Method | Endpoint | Access | Description |
| :--- | :--- | :--- | :--- |
| `POST` | `/api/appointments` | Patient / Admin | Book appointment with conflict checking |
| `GET` | `/api/appointments` | Authenticated | List appointments (filtered by user role) |
| `GET` | `/api/appointments/:id` | Authenticated | Get appointment details |
| `PATCH` | `/api/appointments/:id/status` | Authenticated | Update status (`Confirmed`, `Completed`, etc.) |

### Prescriptions (`/api/prescriptions`)
| Method | Endpoint | Access | Description |
| :--- | :--- | :--- | :--- |
| `POST` | `/api/prescriptions` | Doctor | Issue prescription for completed visit |
| `GET` | `/api/prescriptions/:id` | Authenticated | Get prescription by ID |
| `GET` | `/api/prescriptions/appointment/:appointmentId` | Authenticated | Get prescription for an appointment |
| `GET` | `/api/prescriptions/patient/:patientId` | Authenticated | Get all prescriptions for a patient |

### Patients & Medical History (`/api/patients`)
| Method | Endpoint | Access | Description |
| :--- | :--- | :--- | :--- |
| `GET` | `/api/patients/me` | Patient | Get current patient profile |
| `PUT` | `/api/patients/me/profile` | Patient | Update personal medical profile |
| `GET` | `/api/patients/:id/history` | Authenticated | Get medical history (ownership protected) |
| `GET` | `/api/patients` | Admin / Doctor | List registered patients |

### Billing (`/api/billing`)
| Method | Endpoint | Access | Description |
| :--- | :--- | :--- | :--- |
| `GET` | `/api/billing/:id` | Authenticated | Get billing invoice details |
| `GET` | `/api/billing/patient/:patientId` | Authenticated | Get invoices for a patient |
| `POST` | `/api/billing` | Admin | Create billing invoice |
| `PATCH` | `/api/billing/:id` | Admin | Update payment status (`Pending` → `Paid`) |

### Notifications (`/api/notifications`)
| Method | Endpoint | Access | Description |
| :--- | :--- | :--- | :--- |
| `GET` | `/api/notifications` | Authenticated | Get notifications and unread count |
| `PATCH` | `/api/notifications/:id/read` | Authenticated | Mark notification as read |
| `PATCH` | `/api/notifications/read-all` | Authenticated | Mark all notifications as read |

### Administrative Reports (`/api/admin/reports`)
| Method | Endpoint | Access | Description |
| :--- | :--- | :--- | :--- |
| `GET` | `/api/admin/reports/appointments` | Admin | Appointment metrics by period |
| `GET` | `/api/admin/reports/department-load` | Admin | Patient volume across departments |
| `GET` | `/api/admin/reports/doctor-utilization` | Admin | Doctor booking volume and completion rate |

### Health Check (`/api/health`)
| Method | Endpoint | Access | Description |
| :--- | :--- | :--- | :--- |
| `GET` | `/api/health` | Public | Check service status and uptime |

---

## Business Rules

1. **Appointment Conflicts:** A doctor cannot have overlapping active bookings (`Booked` or `Confirmed`) at the same date and time. Conflicting attempts return HTTP `409 APPOINTMENT_CONFLICT`.
2. **Availability Slot Overlaps:** A doctor's availability slots cannot overlap on the same day of the week, and start times must precede end times.
3. **Appointment Status Workflow:** Appointments follow valid state transitions:
   - `Booked` → `Confirmed` or `Cancelled`
   - `Confirmed` → `Completed`, `Cancelled`, or `No-show`
   - Terminal states (`Completed`, `Cancelled`, `No-show`) cannot be modified further.
4. **Prescription Eligibility:** Digital prescriptions can only be created for appointments in `Completed` status. Only the attending doctor may issue the prescription.
5. **Medical History Ownership:** Patients can only access their own medical history. Unauthorized access attempts return HTTP `403 OWNERSHIP_VIOLATION`.
6. **Automated Billing:** Marking an appointment as `Completed` automatically generates a consultation billing invoice with status `Pending`.
7. **Role-Based Protection:** Protected endpoints enforce user role checks (`Patient`, `Doctor`, `Admin`) at the route layer.

---

## Database Collections & Relationships

The system uses 8 Mongoose collections. Doctor weekly availability slots are embedded directly inside the `doctors` collection.

```mermaid
erDiagram
    USER {
        ObjectId _id PK
        string name
        string email
        string passwordHash
        string role
        string phone
        boolean isActive
    }

    PATIENT {
        ObjectId _id PK
        ObjectId userId FK
        date dob
        string gender
        string bloodGroup
        string medicalNotes
        string address
    }

    DOCTOR {
        ObjectId _id PK
        ObjectId userId FK
        ObjectId departmentId FK
        string specialization
        string qualification
        number experience
        number consultationFee
        array availabilitySlots
        boolean isAvailable
    }

    DEPARTMENT {
        ObjectId _id PK
        string name
        string description
        boolean isActive
    }

    APPOINTMENT {
        ObjectId _id PK
        ObjectId patientId FK
        ObjectId doctorId FK
        date date
        object slot
        string status
        string reason
        string notes
        string cancelledBy
    }

    PRESCRIPTION {
        ObjectId _id PK
        ObjectId appointmentId FK
        ObjectId doctorId FK
        ObjectId patientId FK
        array medicines
        string diagnosis
        string notes
        date issuedAt
    }

    BILLING {
        ObjectId _id PK
        ObjectId appointmentId FK
        ObjectId patientId FK
        number amount
        string paymentStatus
        string paymentMethod
        string description
        date paidAt
    }

    NOTIFICATION {
        ObjectId _id PK
        ObjectId userId FK
        ObjectId appointmentId FK
        string type
        string title
        string message
        boolean isRead
        string status
    }

    USER ||--o| PATIENT : "has"
    USER ||--o| DOCTOR : "has"
    DEPARTMENT ||--o{ DOCTOR : "contains"
    PATIENT ||--o{ APPOINTMENT : "books"
    DOCTOR ||--o{ APPOINTMENT : "handles"
    APPOINTMENT ||--o| PRESCRIPTION : "has"
    APPOINTMENT ||--o| BILLING : "has"
    USER ||--o{ NOTIFICATION : "receives"
    APPOINTMENT ||--o{ NOTIFICATION : "triggers"
```

### Collections Summary

- **`users`:** Accounts, hashed credentials, roles (`Patient`, `Doctor`, `Admin`), and contact details.
- **`patients`:** Patient demographics, date of birth, blood group, medical notes, address, and emergency contact.
- **`doctors`:** Doctor profiles, department links, qualifications, fees, and embedded weekly availability slots.
- **`departments`:** Hospital departments (e.g., Cardiology, Neurology, Orthopedics).
- **`appointments`:** Booked visits linking patients and doctors with date, time slot, status, and cancellation notes.
- **`prescriptions`:** Clinical prescriptions for completed appointments with medicine lists, dosages, and diagnosis.
- **`billing`:** Consultation fee invoices linked to appointments with payment statuses (`Pending`, `Paid`, `Cancelled`).
- **`notifications`:** User notification logs for booking confirmations, reminders, and prescription issuances.

---

## Project Structure

```
Hospital-Patient-And-Appointment-Management-System-L-and-T-Group-Project/
├── config/
│   ├── db.js                     # MongoDB connection & embedded fallback
│   └── env.js                    # Environment variable configuration
├── controllers/
│   ├── appointmentController.js  # Booking, conflict checks, status transitions
│   ├── authController.js         # Authentication, registration, current user
│   ├── billingController.js      # Invoicing and payment status updates
│   ├── departmentController.js   # Department management
│   ├── doctorController.js       # Doctor profiles, search, availability slots
│   ├── notificationController.js # Notifications retrieval & read markers
│   ├── patientController.js      # Patient profiles & chronological medical history
│   ├── prescriptionController.js # Prescription generation & retrieval
│   └── reportController.js       # Administrative aggregation reports
├── middleware/
│   ├── auth.js                   # JWT token authentication
│   ├── authorize.js              # Role-based & ownership authorization
│   ├── errorHandler.js           # Centralized API error handling
│   └── validate.js               # express-validator request validation
├── models/
│   ├── Appointment.js            # Appointment schema & indexes
│   ├── Billing.js                # Consultation billing schema
│   ├── Department.js             # Hospital department schema
│   ├── Doctor.js                 # Doctor schema & embedded availability
│   ├── Notification.js           # Notification schema
│   ├── Patient.js                # Patient profile schema
│   ├── Prescription.js           # Prescription schema
│   └── User.js                   # User account schema & bcrypt hashing
├── postman/
│   └── P02-Hospital-System.postman_collection.json # Exported Postman collection
├── public/
│   ├── app.js                    # Frontend client-side logic
│   ├── index.html                # Single-page web interface
│   └── style.css                 # Interface styles
├── routes/
│   ├── appointmentRoutes.js      # Appointment endpoints
│   ├── authRoutes.js             # Authentication endpoints
│   ├── billingRoutes.js          # Billing endpoints
│   ├── departmentRoutes.js       # Department endpoints
│   ├── doctorRoutes.js           # Doctor endpoints
│   ├── notificationRoutes.js     # Notification endpoints
│   ├── patientRoutes.js          # Patient endpoints
│   ├── prescriptionRoutes.js     # Prescription endpoints
│   └── reportRoutes.js           # Administrative reporting endpoints
├── scripts/
│   └── verify-api.js             # Live end-to-end API verification script
├── seed/
│   ├── seed.js                   # Standalone seed runner
│   └── seedHelper.js             # Demo data generator
├── services/
│   └── notificationService.js    # In-app event notification dispatcher
├── tests/
│   └── api.test.js               # 40 automated Jest integration tests
├── validators/
│   ├── appointmentValidator.js   # Booking & slot validation rules
│   ├── authValidator.js          # Registration & login validation
│   ├── billingValidator.js       # Payment validation rules
│   ├── departmentValidator.js    # Department validation rules
│   ├── doctorValidator.js        # Doctor & availability rules
│   ├── patientValidator.js       # Patient profile validation
│   └── prescriptionValidator.js   # Prescription structure validation
├── .env.example                  # Template environment variables
├── .gitignore                    # Git exclusion rules
├── package.json                  # Dependencies and execution scripts
├── package-lock.json             # Locked dependency tree
├── README.md                     # Project documentation
└── server.js                     # Express application entrypoint
```

---

## Testing

### Automated Test Suite
Run the full automated test suite containing 40 integration tests with Jest:

```bash
npm test
```

Covers:
- User registration, duplicate email handling, and validation errors
- Login authentication, password verification, and token generation
- Department management and public directory access
- Doctor profiles, availability slot updates, and slot overlap rejection
- Appointment booking with conflict rejection on overlapping slots
- Status workflow transitions (`Booked` → `Confirmed` → `Completed`) and invalid transition rejection
- Prescription generation restrictions (Doctor only, Completed appointments only)
- Patient medical history ownership protection (`403 OWNERSHIP_VIOLATION`)
- Automatic billing creation and payment status updates
- In-app notification creation and mark-as-read functionality
- Administrative aggregation reports and non-admin access restrictions

### Live API Verification
To execute live end-to-end verification against a running server:

```bash
# Terminal 1: Start server
npm start

# Terminal 2: Run verification script
npm run verify
```

---

## Postman Collection

An importable Postman collection is available at:
`postman/P02-Hospital-System.postman_collection.json`

### Usage:
1. Start the server (`npm start`).
2. In **Postman**, click **Import**.
3. Select `postman/P02-Hospital-System.postman_collection.json`.
4. The collection is configured with default environment variables (`baseUrl = http://localhost:5000/api`).
5. Execute requests individually or run the complete collection runner.

---

## Troubleshooting

| Problem | Cause | Solution |
| :--- | :--- | :--- |
| `MongoDB Connection Error` | Local MongoDB daemon is not running | Ensure MongoDB is running locally, verify `MONGO_URI` in `.env`, or allow the embedded development fallback to initialize in development mode. |
| `Port 5000 already in use` | Another process is occupying port 5000 | Update `PORT` in `.env` (e.g., `PORT=5001`) or stop the competing process. |
| `401 Unauthorized` | Missing or invalid JWT token | Log in via `/api/auth/login` to obtain a fresh token and pass it in the `Authorization: Bearer <token>` header. |
| `403 Forbidden` / `OWNERSHIP_VIOLATION` | Insufficient role permissions or accessing another user's records | Verify your user role matches the required operation (`Admin`, `Doctor`, or `Patient`) or check record ownership. |
| `409 APPOINTMENT_CONFLICT` | Overlapping appointment for the selected doctor | Select a different time slot or date when the doctor has no active bookings. |
| `409 OVERLAPPING_SLOTS` | Doctor availability slots overlap on the same day | Ensure availability slot start and end times do not intersect on the same day. |
| Database is empty | Database has not been seeded | Run `npm run seed` to generate demonstration data. |

---

## Contributions

This project was developed collaboratively across four main areas:
- **Core Setup & Scheduling:** Patient authentication, patient registration, doctor and department management, doctor availability, and appointment booking.
- **Clinical Workflow:** Appointment status workflow, digital prescriptions, medical history, and department/specialization directory.
- **Supporting Services & Reporting:** Notifications, billing, doctor search, reports, and role-based access control.
- **Integration & Documentation:** Database schema, API/Postman testing, documentation, and system integration.
