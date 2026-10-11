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
Stores user profiles for Admins# 🎓 College Event Management System — MongoDB NoSQL Project

A full-stack College Event Management System built with **Node.js, Express.js, MongoDB, and Mongoose**. The project demonstrates NoSQL database design, schema validation, compound and text indexes, document references, role-based access control (RBAC), and advanced MongoDB aggregation pipelines.

The system supports event creation and management, student registrations, QR-code passes, attendance tracking, feedback, personalized recommendations, venue conflict detection, and analytics dashboards.

---

## 🌟 Key Features

- **NoSQL Database Design:** MongoDB documents, ObjectId references, embedded subdocuments, and compound unique indexes.
- **Advanced Aggregation:** Pipelines using `$match`, `$lookup`, `$unwind`, `$group`, `$project`, `$facet`, `$addFields`, `$cond`, `$sort`, and `$dateToString`.
- **Personalized Recommendations:** Ranks upcoming events using student interests, event popularity, and registration history.
- **Venue Conflict Detection:** Checks venue availability, overlapping schedules, and capacity restrictions.
- **Authentication and Authorization:** JWT authentication, password hashing with bcrypt, and role-based access for students, organizers, and administrators.
- **QR-Code Attendance:** Registration passes and event check-in functionality.
- **Waitlist Management:** Supports waitlisting and automatic promotion when a seat becomes available.
- **Analytics Dashboard:** Event statistics, attendance reports, venue utilization, and engagement metrics.
- **Database Options:** Local MongoDB, MongoDB Atlas, and optional in-memory MongoDB for testing.
- **Validation and Security:** Frontend and backend validation, Mongoose schema rules, unique indexes, rate limiting, and security headers.

## 🏗️ Project Architecture

```text
college-event-management/
├── server/
│   ├── config/
│   │   └── database.js
│   ├── models/
│   │   ├── User.js
│   │   ├── Event.js
│   │   ├── Registration.js
│   │   ├── Venue.js
│   │   ├── Feedback.js
│   │   ├── Notification.js
│   │   ├── OrganizerVerification.js
│   │   ├── PlatformFeedback.js
│   │   └── EventSuggestion.js
│   ├── controllers/
│   ├── routes/
│   ├── middleware/
│   ├── services/
│   ├── utils/
│   │   ├── seedData.js
│   │   └── seedRunner.js
│   ├── validations/
│   │   └── schemas.js
│   ├── app.js
│   └── server.js
├── frontend/
│   ├── index.html
│   ├── css/
│   │   └── style.css
│   └── js/
│       └── app.js
├── .env
├── .env.example
├── package.json
└── README.md
```

*Note: This structure documents the expected project layout. Some files or folders may vary in your actual implementation.*

---

## 🛠️ Technology Stack

| Technology | Purpose |
|---|---|
| Node.js | JavaScript runtime |
| Express.js | REST API and server routing |
| MongoDB | NoSQL database |
| Mongoose | Data models, validation, and indexes |
| bcrypt | Password hashing |
| JSON Web Token (JWT) | Authentication |
| HTML, CSS, JavaScript | Frontend dashboard |
| QR-code library | Registration passes and attendance |

---

## 🗄️ MongoDB Collections

### 1. Users

The `users` collection stores student, organizer, and administrator accounts.

```json
{
  "name": "Tejas Ahire",
  "email": "student@example.com",
  "password": "<bcrypt-hashed-password>",
  "phone": "+919876543220",
  "role": "student",
  "interests": ["Workshop", "Sports", "Technology"],
  "preferences": {
    "notificationEnabled": true,
    "emailNotifications": true,
    "preferredCategories": ["Workshop", "Seminar"]
  },
  "isVerified": true,
  "isActive": true
}
```

**Validation and indexes**

- Unique email index.
- Role restricted to `admin`, `organizer`, or `student`.
- Email and phone validation.
- Account deactivation using `isActive: false`.
- Passwords are stored as hashes, never as plaintext.

### 2. Events

The `events` collection stores event information, schedules, venues, budgets, posters, and statistics.

```json
{
  "organizerId": "ObjectId",
  "title": "Full-Stack Modern AI & LLM Systems Workshop",
  "description": "A hands-on workshop covering modern AI systems and LLM applications.",
  "category": "Workshop",
  "startDate": "2026-11-04T12:00:00.000Z",
  "endDate": "2026-11-04T18:00:00.000Z",
  "venueId": "ObjectId",
  "capacity": 50,
  "status": "published",
  "budget": {
    "total": 45000,
    "breakdown": [
      {
        "item": "Guest Speaker Honorarium",
        "amount": 25000
      },
      {
        "item": "Cloud Computing Credits",
        "amount": 15000
      },
      {
        "item": "Refreshments",
        "amount": 5000
      }
    ]
  },
  "statistics": {
    "views": 0,
    "registrations": 0,
    "attendance": 0
  }
}
```

**Validation and indexes**

- Title length: 2–200 characters.
- Event start time must precede the end time.
- Positive capacity and non-negative budget.
- Indexes on organizer, category, start date, venue, and status.
- Compound unique index on `{ organizerId: 1, title: 1, startDate: 1 }`.
- Text index on title and description for full-text search.

*The sample dates and statistics are illustrative. Ensure that the actual model validators match the documented rules.*

### 3. Venues

The `venues` collection stores campus locations and facilities.

```json
{
  "name": "Main University Auditorium",
  "location": {
    "building": "Central Block",
    "floor": 1,
    "room": "AUD-101"
  },
  "capacity": 500,
  "amenities": [
    "4K Projector",
    "Central AC",
    "Dolby Sound System",
    "Gigabit WiFi"
  ],
  "isActive": true
}
```

**Business rules**

- Venue capacity must be greater than zero.
- Event capacity cannot exceed venue capacity.
- Overlapping bookings for the same venue must be rejected.
- Inactive venues cannot be assigned to new events.

### 4. Registrations

The `registrations` collection tracks student RSVPs, waitlists, QR passes, and check-in records.

```json
{
  "eventId": "ObjectId",
  "studentId": "ObjectId",
  "status": "registered",
  "rsvpConfirmed": true,
  "qrCode": "<generated-qr-code-data>",
  "registeredAt": "2026-10-01T09:30:00.000Z",
  "checkIn": {
    "checkedIn": false,
    "checkedInAt": null
  }
}
```

**Business rules**

- Compound unique index on `{ eventId: 1, studentId: 1 }`.
- A student cannot register more than once for the same event.
- Full events may place additional students on a waitlist.
- Cancelling a registration can promote the earliest eligible waitlisted student.
- Attendance is recorded through the check-in process.

### 5. Feedback

The `feedbacks` collection stores event ratings and attendee comments.

```json
{
  "eventId": "ObjectId",
  "studentId": "ObjectId",
  "ratings": {
    "eventQuality": 5,
    "organization": 5,
    "venue": 4,
    "contentRelevance": 5,
    "overallValue": 5
  },
  "overallRating": 5,
  "comments": "The workshop was informative and practical.",
  "suggestions": "An advanced follow-up workshop would be helpful.",
  "sentiment": "positive"
}
```

**Business rules**

- One feedback submission per student per event.
- Ratings must satisfy the configured schema limits.
- Only eligible attendees may submit feedback.
- The database collection name depends on the Mongoose model configuration.

### Additional Collections

| Collection | Purpose |
|---|---|
| `notifications` | User notifications and read status |
| `organizerverifications` | Organizer verification workflow |
| `platformfeedbacks` | Platform bug reports and feature requests |
| `eventsuggestions` | Student event suggestions and upvotes |

*MongoDB collection names are determined by the Mongoose model name and collection configuration. Verify the exact names in your database.*

---

## 📊 MongoDB Aggregation Pipelines

The project uses aggregation pipelines to transform and analyze MongoDB documents without requiring a separate relational database.

### 1. Attendance Analytics by Category

This pipeline calculates registration totals, check-ins, cancellations, no-shows, and attendance rates by event category.

```javascript
db.registrations.aggregate([
  {
    $lookup: {
      from: "events",
      localField: "eventId",
      foreignField: "_id",
      as: "event"
    }
  },
  { $unwind: "$event" },
  {
    $group: {
      _id: "$event.category",
      totalRegistrations: {
        $sum: {
          $cond: [
            { $in: ["$status", ["registered", "checked-in", "no-show"]] },
            1,
            0
          ]
        }
      },
      checkedIn: {
        $sum: {
          $cond: [{ $eq: ["$status", "checked-in"] }, 1, 0]
        }
      },
      noShows: {
        $sum: {
          $cond: [{ $eq: ["$status", "no-show"] }, 1, 0]
        }
      },
      cancelled: {
        $sum: {
          $cond: [{ $eq: ["$status", "cancelled"] }, 1, 0]
        }
      }
    }
  },
  {
    $project: {
      _id: 0,
      category: "$_id",
      totalRegistrations: 1,
      checkedIn: 1,
      noShows: 1,
      cancelled: 1,
      attendanceRate: {
        $cond: [
          { $gt: ["$totalRegistrations", 0] },
          {
            $round: [
              {
                $multiply: [
                  { $divide: ["$checkedIn", "$totalRegistrations"] },
                  100
                ]
              },
              2
            ]
          },
          0
        ]
      }
    }
  },
  { $sort: { attendanceRate: -1 } }
]);
```

*Adjust the status values and attendance denominator if your application uses a different registration lifecycle.*

### 2. Personalized Event Recommendations

The following example ranks upcoming events based on category interests, registration popularity, and views. It assumes `registeredEventIds` and `userInterests` have already been populated for the authenticated student.

```javascript
db.events.aggregate([
  {
    $match: {
      _id: { $nin: registeredEventIds },
      status: "published",
      startDate: { $gte: new Date() }
    }
  },
  {
    $lookup: {
      from: "venues",
      localField: "venueId",
      foreignField: "_id",
      as: "venue"
    }
  },
  {
    $unwind: {
      path: "$venue",
      preserveNullAndEmptyArrays: true
    }
  },
  {
    $addFields: {
      relevanceScore: {
        $add: [
          {
            $cond: [
              { $in: ["$category", userInterests] },
              10,
              0
            ]
          },
          {
            $multiply: [
              { $ifNull: ["$statistics.registrations", 0] },
              0.1
            ]
          },
          {
            $multiply: [
              { $ifNull: ["$statistics.views", 0] },
              0.01
            ]
          }
        ]
      }
    }
  },
  { $sort: { relevanceScore: -1, startDate: 1 } },
  { $limit: 6 }
]);
```

### 3. Venue Utilization Analysis

This pipeline calculates the number of events assigned to each venue and compares average event capacity with venue capacity.

```javascript
db.venues.aggregate([
  {
    $lookup: {
      from: "events",
      localField: "_id",
      foreignField: "venueId",
      as: "events"
    }
  },
  {
    $project: {
      name: 1,
      capacity: 1,
      totalEventsHosted: { $size: "$events" },
      avgEventCapacity: {
        $cond: [
          { $gt: [{ $size: "$events" }, 0] },
          { $avg: "$events.capacity" },
          0
        ]
      }
    }
  },
  {
    $addFields: {
      avgEventCapacity: { $round: ["$avgEventCapacity", 1] },
      capacityUtilizationRate: {
        $cond: [
          { $gt: ["$capacity", 0] },
          {
            $round: [
              {
                $multiply: [
                  { $divide: ["$avgEventCapacity", "$capacity"] },
                  100
                ]
              },
              2
            ]
          },
          0
        ]
      }
    }
  },
  { $sort: { totalEventsHosted: -1 } }
]);
```

*This is an average planned-capacity metric, not actual attendance utilization. Actual utilization requires joining registrations or attendance records.*

---

## ⚡ REST API Reference

All endpoints use the `/api` prefix unless stated otherwise. Protected endpoints require the authentication and authorization configured by the backend.

### Authentication

| Method | Endpoint | Purpose |
|---|---|---|
| POST | `/api/auth/register` | Register an account |
| POST | `/api/auth/login` | Authenticate and obtain a JWT |
| GET | `/api/auth/me` | Get the current user's profile |

### Users

| Method | Endpoint | Purpose |
|---|---|---|
| GET | `/api/users` | List users; admin access |
| GET | `/api/users/:id` | Get a user profile |
| PUT | `/api/users/:id` | Update profile or preferences |
| DELETE | `/api/users/:id` | Deactivate an account |
| PUT | `/api/users/:id/reactivate` | Reactivate an account |
| GET | `/api/users/:id/history` | View registration and event history |

### Events

| Method | Endpoint | Purpose |
|---|---|---|
| GET | `/api/events` | Search and filter events |
| POST | `/api/events` | Create an event |
| GET | `/api/events/:id` | Get event details and ratings |
| PUT | `/api/events/:id` | Update event information |
| PUT | `/api/events/:id/status` | Approve, reject, or cancel an event |
| DELETE | `/api/events/:id` | Archive an event |

### Registrations and Attendance

| Method | Endpoint | Purpose |
|---|---|---|
| POST | `/api/events/:eventId/register` | Register or join a waitlist |
| GET | `/api/events/:eventId/registrations` | Get the attendee roster |
| GET | `/api/registrations/:id` | View a registration and QR pass |
| PUT | `/api/registrations/:id/check-in` | Record attendance |
| DELETE | `/api/registrations/:id` | Cancel a registration |

Roster access must follow the authorization and privacy rules enforced by the backend. Organizers should only access the operational rosters for events they manage, while students should receive only the permitted attendee information.

### Feedback and Reviews

| Method | Endpoint | Purpose |
|---|---|---|
| POST | `/api/events/:eventId/feedback` | Submit feedback |
| GET | `/api/events/:eventId/feedback` | Get event reviews and rating summaries |
| PUT | `/api/feedback/:id` | Update feedback |
| DELETE | `/api/feedback/:id` | Delete feedback |

### Reports and Analytics

| Method | Endpoint | Purpose |
|---|---|---|
| GET | `/api/reports/admin` | Administrative KPI summary |
| GET | `/api/reports/attendance` | Attendance analytics |
| GET | `/api/reports/student` | Student engagement metrics |
| GET | `/api/reports/organizer` | Organizer performance metrics |
| GET | `/api/reports/venues` | Venue utilization metrics |
| GET | `/api/reports/recommendations` | Personalized event recommendations |

*This API reference describes the intended interface. Confirm each route exists in your current source code before treating it as implemented.*

---

## 🚀 Installation and Setup

### Prerequisites

Install the following before running the project:

- Node.js and npm.
- MongoDB Community Server for local database use.
- MongoDB Compass (optional, for visual database inspection).

### 1. Install Dependencies

Open a terminal in the project root:

```bash
npm install
```

### 2. Configure Environment Variables

Create a `.env` file in the project root. For local MongoDB, use:

```env
PORT=5000
MONGODB_URI=mongodb://127.0.0.1:27017/college_events
USE_IN_MEMORY_DB_IF_LOCAL_FAILS=true
JWT_SECRET=replace_with_a_long_random_secret
JWT_EXPIRES_IN=7d
```

**Important:** Generate your own strong JWT secret. Never commit `.env` or production credentials to Git. Keep `.env.example` populated with placeholders only.

The project is configured to use local MongoDB. Keep this URI if you want to continue using your existing local database.

### 3. Start MongoDB

Make sure the local MongoDB service is running on port `27017`.

You can verify the connection through MongoDB Compass using:

```text
mongodb://127.0.0.1:27017
```

### 4. Seed the Database

Run the seed command from the project root:

```bash
npm run seed
```

The documented sample dataset contains:

- 1 administrator
- 3 organizers
- 10 students
- 5 venues
- 8 events
- 28 registrations
- Feedback and notification records

**Warning:** Check `server/utils/seedRunner.js` before running the seed command. Depending on its implementation, it may delete or recreate existing sample data. Back up any data you need to preserve.

### 5. Start the Application

```bash
npm start
```

Open the application at:

**http://localhost:5000**

This address assumes that the Express application serves the frontend and that the server starts successfully. If the frontend is hosted separately, follow the configuration in your project.

### 6. Optional Development and Test Commands

```bash
npm run validate
npm run smoke
npm test
```

Run only scripts that exist in your `package.json`. These commands are intended for validation, smoke testing, and comprehensive tests, respectively.

---

## 🔑 Test Accounts

The following are the sample credentials documented for the seeded dataset.

| Role | Email | Password |
|---|---|---|
| Admin | `admin@example.com` | `Admin@123` |
| Organizer | `cs_dept@example.com` | `Organizer@123` |
| Organizer | `ieee_chapter@example.com` | `Organizer@123` |
| Organizer | `sports_council@example.com` | `Organizer@123` |
| Student | `student@example.com` | `Student@123` |
| Student | `pooja.patel@example.com` | `Student@123` |

These accounts work only if they exist in your seed data. Use sample credentials only in local development and never deploy publicly accessible accounts with default passwords.

---

## 🔍 Inspecting the Database with MongoDB Compass

1. Start MongoDB and open MongoDB Compass.
2. Connect using `mongodb://127.0.0.1:27017`.
3. Select the `college_events` database.
4. Inspect the users, events, registrations, venues, feedback, notifications, and other collections.
5. Open the **Indexes** tab to review unique, compound, and text indexes.
6. Open the **Aggregations** tab to execute and inspect the sample pipelines.

Check the collection names and index definitions against the actual Mongoose models and database.

---

## 🔐 Validation and Security

The intended architecture uses multiple layers of validation and security.

| Control | Purpose |
|---|---|
| Password hashing | Stores password hashes using bcrypt |
| JWT authentication | Protects authenticated API requests |
| Role-based access control | Restricts actions to permitted roles |
| Input validation | Rejects malformed or invalid request data |
| Mongoose validation | Enforces schema rules |
| Unique indexes | Prevents duplicate records where configured |
| Rate limiting | Limits abusive or repeated requests |
| Output escaping | Helps prevent cross-site scripting |
| Security headers | Adds browser-level protections |
| Centralized error handling | Returns consistent API errors without exposing internals |
| File upload restrictions | Enforces configured size and file-type limits |

Additional controls, such as CSRF protection, account lockout, content security policy, and HSTS, must be verified in the actual middleware and deployment configuration before being claimed as operational.

### Department and Course Validation

Where department and course values are managed through database collections, validate incoming identifiers against the corresponding records. Do not trust arbitrary client-provided department or course names.

### Event Capacity

The registration endpoint should check available seats before confirming a registration. A full event can return HTTP `409 Conflict` when that behavior is implemented.

To prevent race conditions when multiple students register simultaneously, seat allocation and registration updates should use an appropriate atomic operation or MongoDB transaction where supported.

### Venue Scheduling

Before creating or updating an event, validate:

- The venue exists and is active.
- The event capacity does not exceed venue capacity.
- The start date precedes the end date.
- No other event overlaps the requested venue time slot.

Perform these checks on the server, not only in the frontend.

---

## 🧪 Testing Checklist

Use this checklist to verify the project end to end.

- [ ] Connect to the local MongoDB database.
- [ ] Register a new student and validate required fields.
- [ ] Log in and verify JWT authentication.
- [ ] Confirm unauthorized requests are rejected.
- [ ] Verify role-based access for students, organizers, and administrators.
- [ ] Create, update, publish, and archive an event.
- [ ] Reject events with invalid dates or excessive venue capacity.
- [ ] Register a student and verify the database record.
- [ ] Reject duplicate registration for the same event.
- [ ] Verify capacity limits and waitlist behavior.
- [ ] Generate and validate a registration QR pass.
- [ ] Check in a student and verify attendance persistence.
- [ ] Submit feedback only when the eligibility rules are satisfied.
- [ ] Run each aggregation pipeline and validate the output.
- [ ] Verify unique and text indexes.
- [ ] Confirm API errors do not expose passwords, tokens, or stack traces.

---

## 🧰 Troubleshooting

### MongoDB connection failed

1. Confirm MongoDB Community Server is installed and running.
2. Verify `MONGODB_URI` in `.env`.
3. Confirm port `27017` is available.
4. Check the server terminal for the underlying connection error.

### In-memory MongoDB download failure

The optional `mongodb-memory-server` package may download a MongoDB binary when it starts. A DNS error or unavailable network can prevent that download.

For normal local development, use the installed MongoDB Community Server and the local URI. Do not assume the in-memory fallback works unless its dependency, binary availability, and fallback logic have been tested.

### Seed command fails

- Check that MongoDB is running.
- Review the seed script and its database connection.
- Check whether the script clears existing records.
- Inspect the error output before rerunning the seed.

### Index creation fails

Unique index creation can fail if existing records contain duplicate values. Identify and resolve duplicates before creating the index.

---

## 📚 Learning Outcomes

This project demonstrates:

- MongoDB NoSQL document modeling.
- Mongoose schemas, validation, references, and embedded documents.
- Unique, compound, and text indexing.
- Aggregation pipelines and collection joins with `$lookup`.
- Authentication and role-based authorization.
- REST API development with Express.js.
- Event registration, waitlist, and attendance workflows.
- Database-backed analytics and recommendations.
- Validation, error handling, and application testing.

---

## 📌 Project Information

**Project:** College Event Management System  
**Backend:** Node.js and Express.js  
**Database:** MongoDB with Mongoose  
**Frontend:** HTML, CSS, and JavaScript  
**Architecture:** REST API with a web dashboard

The documentation describes the intended project design. Confirm implementation details, endpoint behavior, test counts, security controls, and database contents against the current source code before using them as verified project claims., Organizers, and Students.

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
#   C o l l a g e - E v e n t 
 
 
