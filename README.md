# 🎓 College Event Management System — MongoDB NoSQL Project

A comprehensive, production-grade College Event Management System built on **Node.js, Express.js, and MongoDB (via Mongoose)**. This project demonstrates advanced NoSQL database design, schema validation rules, compound and text indexing, references and relationships, role-based access control (RBAC), and multi-stage **MongoDB Aggregation Pipelines**.

---

## 🌟 Key Highlights & MongoDB Capabilities

- **Pure NoSQL Design:** Uses MongoDB documents, references (`ObjectId`), embedded subdocuments, and compound unique indexes instead of SQL-style relational tables.
- **Advanced Aggregation Pipelines:** Multi-stage pipelines utilizing `$match`, `$lookup`, `$unwind`, `$group`, `$project`, `$facet`, `$addFields`, `$cond`, `$sort`, and `$dateToString`.
- **Personalized Recommendations Engine:** Recommendation system driven entirely by MongoDB aggregation scoring based on student interests, past participation history, and event popularity weights.
- **Double-Booking & Conflict Detection:** Automatic venue availability verification preventing overlapping schedules and exceeding room capacities.
- **Zero-Config Execution:** Works with local MongoDB, MongoDB Atlas, and includes an automatic in-memory MongoDB fallback runner for testing.
- **Modern Interactive Dashboard:** Built-in web interface with one-click test personas, live aggregation analytics, interactive QR code passes, and attendance check-in.

---

## 🏗️ Architecture & Project Structure

```
college-event-management/
│
├── server/
│   ├── config/
│   │   └── database.js               # MongoDB connection & Memory Server fallback
│   ├── models/
│   │   ├── User.js                   # Users collection with role validation & bcrypt
│   │   ├── Event.js                  # Events collection with text & compound indexes
│   │   ├── Registration.js           # Registrations with compound unique index (eventId+studentId)
│   │   ├── Venue.js                  # Venues collection with capacity constraints
│   │   ├── Feedback.js               # Multi-dimensional feedback & sentiment
│   │   ├── Notification.js           # User notifications with compound read indexes
│   │   ├── OrganizerVerification.js  # University accreditation workflow
│   │   ├── PlatformFeedback.js       # Bug reports & feature suggestions
│   │   └── EventSuggestion.js        # Community student ideas & upvoting
│   ├── controllers/                  # Handlers for CRUD, auth, check-in, reports
│   ├── routes/                       # Express REST endpoints
│   ├── middleware/                   # JWT verification, RBAC, central error handler
│   ├── services/                     # QR code, venue conflicts, recommendations
│   ├── utils/
│   │   ├── seedData.js               # Rich realistic database seed dataset
│   │   └── seedRunner.js             # Standalone seed executor
│   ├── validations/
│   │   └── schemas.js                # Input validators & regex rules
│   ├── app.js                        # Express application instance & static routing
│   └── server.js                     # HTTP server startup & graceful shutdown
│
├── frontend/
│   ├── index.html                    # Single-Page Web Dashboard
│   ├── css/
│   │   └── style.css                 # Modern dark theme styling
│   └── js/
│       └── app.js                    # Dynamic UI controllers & API bindings
│
├── .env                              # Environment configuration
├── .env.example
├── package.json
└── README.md                         # Detailed documentation
```

---

## 🗄️ MongoDB Collections & Document Schemas

### 1. `users` Collection
Stores user profiles for Admins, Organizers, and Students.

```json
{
  "_id": "ObjectId('650000000000000000000010')",
  "name": "Tejas Ahire",
  "email": "student@example.com",
  "password": "$2a$10$hashed_password_string",
  "phone": "+919876543220",
  "role": "student",
  "interests": ["Workshop", "Sports", "Technology"],
  "preferences": {
    "notificationEnabled": true,
    "emailNotifications": true,
    "preferredCategories": ["Workshop", "Seminar"]
  },
  "isVerified": true,
  "isActive": true,
  "createdAt": "2026-09-01T10:00:00.000Z",
  "updatedAt": "2026-09-01T10:00:00.000Z"
}
```
* **Indexes:** `email` (Unique), `role: 1`, `isActive: 1`
* **Validation:** Email regex validation, phone 10–15 digits, role enum `['admin', 'organizer', 'student']`. Soft delete using `isActive: false`.

---

### 2. `events` Collection
Contains campus events, scheduling, budget breakdowns, and statistics.

```json
{
  "_id": "ObjectId('650000000000000000000201')",
  "organizerId": "ObjectId('650000000000000000000002')",
  "title": "Full-Stack Modern AI & LLM Systems Workshop",
  "description": "An intensive hands-on masterclass diving into Large Language Models, RAG architectures...",
  "category": "Workshop",
  "startDate": "2026-10-04T12:00:00.000Z",
  "endDate": "2026-10-04T18:00:00.000Z",
  "venueId": "ObjectId('650000000000000000000102')",
  "capacity": 50,
  "poster": {
    "url": "https://images.unsplash.com/photo-1618005182384-a83a8bd57fbe?w=800",
    "format": "jpg",
    "size": 142000
  },
  "budget": {
    "total": 45000,
    "breakdown": [
      { "item": "Guest Speaker Honorarium", "amount": 25000 },
      { "item": "Cloud Computing Credits", "amount": 15000 },
      { "item": "Refreshments", "amount": 5000 }
    ]
  },
  "status": "published",
  "statistics": {
    "views": 340,
    "registrations": 8,
    "attendance": 0
  },
  "createdAt": "2026-10-01T08:00:00.000Z",
  "updatedAt": "2026-10-01T08:00:00.000Z"
}
```
* **Indexes:**
  - `organizerId: 1`, `category: 1`, `startDate: 1`, `venueId: 1`, `status: 1`
  - **Compound Unique Index:** `{ organizerId: 1, title: 1, startDate: 1 }` (Prevents duplicate event scheduling by the same department)
  - **Text Index:** `{ title: 'text', description: 'text' }` (Enables full-text search)
* **Validation:** Title (2–200 chars), Description (min 50 chars), Category enum, `startDate < endDate`, min 1 hour duration, positive capacity and non-negative budget.

---

### 3. `venues` Collection
Campus auditoriums, labs, and sports arenas.

```json
{
  "_id": "ObjectId('650000000000000000000101')",
  "name": "Main University Auditorium",
  "location": {
    "building": "Central Block",
    "floor": 1,
    "room": "AUD-101"
  },
  "capacity": 500,
  "amenities": ["4K Projector", "Central AC", "Dolby Sound System", "Gigabit WiFi"],
  "images": ["https://images.unsplash.com/photo-1517457373958-b7bdd4587205?w=800"],
  "isActive": true
}
```
* **Validation & Business Rule:** `capacity > 0`. System checks that event capacity does not exceed venue capacity and prevents overlapping time reservations.

---

### 4. `registrations` Collection
Manages student RSVP, QR code pass data, waitlists, and check-in timestamps.

```json
{
  "_id": "ObjectId('650000000000000000000301')",
  "eventId": "ObjectId('650000000000000000000201')",
  "studentId": "ObjectId('650000000000000000000010')",
  "status": "registered",
  "rsvpConfirmed": true,
  "qrCode": "{\"regId\":\"...\",\"eId\":\"...\",\"sId\":\"...\",\"code\":\"9f2b8a\"}",
  "registeredAt": "2026-10-01T09:30:00.000Z",
  "checkIn": {
    "checkedIn": false,
    "checkedInAt": null
  }
}
```
* **Compound Unique Index:** `{ eventId: 1, studentId: 1 }` (Enforces that a student cannot register twice for the same event).
* **Auto Waitlisting & Promotion:** When event capacity is full, status is set to `waitlisted`. When an existing registrant cancels, the earliest waitlisted student is automatically promoted to `registered` and notified.

---

### 5. `feedback` Collection
Collects multi-dimensional ratings and qualitative comments from attendees.

```json
{
  "_id": "ObjectId('650000000000000000000401')",
  "eventId": "ObjectId('650000000000000000000206')",
  "studentId": "ObjectId('650000000000000000000010')",
  "ratings": {
    "eventQuality": 5,
    "organization": 5,
    "venue": 4,
    "contentRelevance": 5,
    "overallValue": 5
  },
  "overallRating": 5,
  "comments": "Exceptional workshop! The hands-on exercises were clear and directly practical.",
  "suggestions": "Would love an advanced part 2 on Atlas Vector Search.",
  "sentiment": "positive"
}
```
* **Compound Unique Index:** `{ eventId: 1, studentId: 1 }` (Ensures only one feedback per attendee per event).
* **Attendance Enforcement:** Feedback can only be submitted if `status === 'checked-in'`.

---

## 📊 MongoDB Aggregation Pipelines

### 1. Attendance Analytics by Category
Calculates total registrations, check-in count, no-shows, and percentage turnout per category:

```javascript
[
  {
    $lookup: {
      from: 'events',
      localField: 'eventId',
      foreignField: '_id',
      as: 'event'
    }
  },
  { $unwind: '$event' },
  {
    $group: {
      _id: '$event.category',
      totalRegistrations: { $sum: 1 },
      checkedIn: {
        $sum: { $cond: [{ $eq: ['$status', 'checked-in'] }, 1, 0] }
      },
      noShows: {
        $sum: { $cond: [{ $eq: ['$status', 'no-show'] }, 1, 0] }
      },
      cancelled: {
        $sum: { $cond: [{ $eq: ['$status', 'cancelled'] }, 1, 0] }
      }
    }
  },
  {
    $project: {
      category: '$_id',
      totalRegistrations: 1,
      checkedIn: 1,
      noShows: 1,
      cancelled: 1,
      attendanceRate: {
        $cond: [
          { $gt: ['$totalRegistrations', 0] },
          { $round: [{ $multiply: [{ $divide: ['$checkedIn', '$totalRegistrations'] }, 100] }, 2] },
          0
        ]
      }
    }
  },
  { $sort: { attendanceRate: -1 } }
]
```

---

### 2. Personalized Event Recommendations
Scores and ranks upcoming published events based on student interests and previous attendance:

```javascript
[
  {
    $match: {
      _id: { $nin: registeredEventIds },
      status: 'published',
      startDate: { $gte: new Date() }
    }
  },
  {
    $lookup: {
      from: 'venues',
      localField: 'venueId',
      foreignField: '_id',
      as: 'venue'
    }
  },
  { $unwind: { path: '$venue', preserveNullAndEmptyArrays: true } },
  {
    $addFields: {
      relevanceScore: {
        $add: [
          { $cond: [{ $in: ['$category', userInterests] }, 10, 0] },
          { $multiply: [{ $ifNull: ['$statistics.registrations', 0] }, 0.1] },
          { $multiply: [{ $ifNull: ['$statistics.views', 0] }, 0.01] }
        ]
      }
    }
  },
  { $sort: { relevanceScore: -1, startDate: 1 } },
  { $limit: 6 }
]
```

---

### 3. Venue Utilization Analysis
Aggregates total events hosted, capacity usage, and percentage utilization per campus venue:

```javascript
[
  {
    $lookup: {
      from: 'events',
      localField: '_id',
      foreignField: 'venueId',
      as: 'events'
    }
  },
  {
    $project: {
      name: 1,
      capacity: 1,
      totalEventsHosted: { $size: '$events' },
      avgEventCapacity: {
        $cond: [
          { $gt: [{ $size: '$events' }, 0] },
          { $avg: '$events.capacity' },
          0
        ]
      }
    }
  },
  {
    $project: {
      name: 1,
      capacity: 1,
      totalEventsHosted: 1,
      avgEventCapacity: { $round: ['$avgEventCapacity', 1] },
      capacityUtilizationRate: {
        $cond: [
          { $gt: ['$capacity', 0] },
          { $round: [{ $multiply: [{ $divide: ['$avgEventCapacity', '$capacity'] }, 100] }, 2] },
          0
        ]
      }
    }
  },
  { $sort: { totalEventsHosted: -1 } }
]
```

---

## ⚡ REST API Reference

### Authentication
- `POST /api/auth/register` — Register a new user (`student`, `organizer`, `admin`)
- `POST /api/auth/login` — Login and receive JWT token
- `GET  /api/auth/me` — Retrieve current authenticated profile

### Users
- `GET    /api/users` — Admin view of all users (supports `role`, `isActive`, `search`)
- `GET    /api/users/:id` — View user profile
- `PUT    /api/users/:id` — Update profile/preferences
- `DELETE /api/users/:id` — Soft-deactivate user account
- `PUT    /api/users/:id/reactivate` — Reactivate user account
- `GET    /api/users/:id/history` — Aggregated event history & registrations

### Events
- `GET    /api/events` — Query events (supports search, category, date, availability, sorting)
- `POST   /api/events` — Create event (Organizer/Admin)
- `GET    /api/events/:id` — Event details + aggregated ratings (increments view counter)
- `PUT    /api/events/:id` — Update event details
- `PUT    /api/events/:id/status` — Admin approve/reject/cancel event
- `DELETE /api/events/:id` — Soft archive event

### Registrations & Check-In
- `POST   /api/events/:eventId/register` — RSVP/Register for event (or placed on waitlist)
- `GET    /api/events/:eventId/registrations` — Attendee roster with status breakdown.
  Organizers of the event and admins receive the full operational roster (email, phone,
  registration ID, check-in staff, and every status including `cancelled`/`no-show`).
  Logged-in **students** receive a privacy-filtered roster: `name`, `rollNumber`,
  `department`, `course`, plus an `isMe` flag — restricted to rows that actually hold a
  seat (`registered` / `checked-in`). The `status` filter is ignored for students so it
  cannot be used to reveal cancelled or waitlisted rows. Other organizers get `403`.
- `GET    /api/registrations/:id` — View pass with QR code
- `PUT    /api/registrations/:id/check-in` — Check-in student attendance
- `DELETE /api/registrations/:id` — Cancel registration (triggers auto waitlist promotion)

### Feedback & Reviews
- `POST   /api/events/:eventId/feedback` — Submit feedback (Attended students only)
- `GET    /api/events/:eventId/feedback` — Aggregated ratings breakdown
- `PUT    /api/feedback/:id` — Edit feedback
- `DELETE /api/feedback/:id` — Delete feedback

### Aggregation Reports
- `GET /api/reports/admin` — Multi-facet Admin KPI summary
- `GET /api/reports/attendance` — Attendance rate by category, month & event
- `GET /api/reports/student` — Student engagement leaderboard & interest distributions
- `GET /api/reports/organizer` — Organizer & department performance metrics
- `GET /api/reports/venues` — Venue capacity & utilization rates
- `GET /api/reports/recommendations` — Personalized event recommendation pipeline

---

## 🚀 Quick Start Guide

### 1. Install Dependencies
```bash
npm install
```

### 2. Configure Environment (`.env`)
Create or edit `.env`:
```env
PORT=5000
MONGODB_URI=mongodb://127.0.0.1:27017/college_events
USE_IN_MEMORY_DB_IF_LOCAL_FAILS=true
JWT_SECRET=supersecret_college_event_jwt_key_2026_xyz
JWT_EXPIRES_IN=7d
```

### 3. Seed Database
Populates 1 Admin, 3 Organizers, 10 Students, 5 Venues, 8 Events, 28 Registrations, Feedback & Notifications:
```bash
npm run seed
```

### 4. Start Server
```bash
npm start
```
The server will be available at: **`http://localhost:5000`**

---

## 🔑 Test User Personas & Credentials

| Role | Email | Password | Description |
| :--- | :--- | :--- | :--- |
| **Admin** | `admin@example.com` | `Admin@123` | Full access: reports, verifications, user status |
| **Organizer** | `cs_dept@example.com` | `Organizer@123` | CS Dept: creates events, manages attendees, check-in |
| **Organizer** | `ieee_chapter@example.com` | `Organizer@123` | IEEE Student Branch organizer |
| **Organizer** | `sports_council@example.com` | `Organizer@123` | Athletics & Sports Council organizer |
| **Student** | `student@example.com` | `Student@123` | Tejas Ahire (Attended events, QR passes, reviews) |
| **Student** | `pooja.patel@example.com` | `Student@123` | Pooja Patel (Cultural & Workshop participant) |

---

## 🧭 Inspecting with MongoDB Compass

1. Open **MongoDB Compass**.
2. Paste connection string: `mongodb://127.0.0.1:27017` (or your Atlas URI).
3. Connect and select database: **`college_events`**.
4. Explore collections: `users`, `events`, `registrations`, `venues`, `feedbacks`, `notifications`, `organizerverifications`.
5. Under **Indexes** tab, verify compound indexes on `registrations` (`{ eventId: 1, studentId: 1 }`), `feedbacks`, and text indexes on `events`.
6. Under **Aggregations** tab, run the sample pipelines detailed in the section above!

---

## ??? Validation, Security & Testing

Validation is enforced at **three layers** � frontend, backend/API, and the database (Mongoose schema rules + unique indexes).

### Backend security controls
| Control | Implementation |
| :--- | :--- |
| **Password storage** | bcrypt hashing (never stored, logged or returned) |
| **Authentication** | JWT Bearer tokens with expiry + secure logout |
| **CSRF** | Per-user HMAC token (`X-CSRF-Token`) required on every authenticated `POST/PUT/PATCH/DELETE` |
| **Rate limiting** | Login brute-force cap, IP login cap, account lockout, registration & global API limits |
| **RBAC** | `admin` / `organizer` / `student` enforced on every protected route |
| **XSS** | Input sanitisation + `escapeHtml()` on output + CSP header |
| **Injection** | Parameterised Mongoose queries; search terms are regex-escaped |
| **Security headers** | `nosniff`, `X-Frame-Options`, CSP, `Referrer-Policy`, COOP/CORP, HSTS (prod) |
| **File uploads** | MIME + extension + 5 MB limit, generated server-side filenames |
| **Error hygiene** | No stack traces, credentials or driver messages in responses |

### Department & course registration rule
`department` and `course` are validated against the `departments` collection (admin-managed) instead of free text.
Admins manage them from **Admin Management Console ? Departments & Courses**.

### Event capacity rule
When all seats are taken the API answers `409 � "Registration is full."` (events are never overbooked).

### Test commands
```bash
npm run validate   # 97 validation assertions across 15 groups (repeatable)
npm run smoke      # 32 end-to-end checks against a running server
npm test           # comprehensive system test
```

`npm run validate` clears artifacts from earlier runs first, so it can be executed repeatedly.

### Test credentials
| Role | Email | Password |
| :--- | :--- | :--- |
| Admin | `admin@example.com` | `Admin@123` |
| Organizer | `cs_dept@example.com` | `Organizer@123` |
| Organizer | `ieee_chapter@example.com` | `Organizer@123` |
| Student | `student@example.com` | `Student@123` |
#   C o l l a g e - E v e n t  
 