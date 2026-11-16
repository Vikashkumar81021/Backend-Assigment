# Teacher Session Booking API

A RESTful backend application built using Node.js, TypeScript, Express.js, MongoDB, and Mongoose. This application allows teachers to create available sessions, users to book sessions, and users to view their upcoming and completed session history.

## Tech Stack

- Node.js
- TypeScript
- Express.js
- MongoDB
- Mongoose
- dotenv

## Project Structure

```text
src/
├── config/
├── controllers/
├── middlewares/
├── models/
├── routes/
├── services/
├── types/
├── utils/
├── validations/
├── app.ts
└── server.ts

postman/
README.md
.env
package.json
tsconfig.json
```

## Installation

Clone the repository:

```bash
git clone <repository-url>
```

Navigate to the project directory:

```bash
cd Assigment
```

Install dependencies:

```bash
npm install
```

## Environment Variables

Create a `.env` file in the root directory.

```env
PORT=3000
MONGO_URI=mongodb://127.0.0.1:27017/teacher_booking
```

Update the MongoDB connection string according to your database configuration.

## Run the Application

Start the development server:

```bash
npm run dev
```

Build the TypeScript application:

```bash
npm run build
```

Start the production server:

```bash
npm start
```

## Database Collections

### 1. User

- fullName
- email (unique)
- phone
- timestamps

### 2. Teacher

- fullName
- email (unique)
- specialization
- experience
- timestamps

### 3. Session

- teacherId (ObjectId reference)
- userId (ObjectId reference or null)
- startTime
- endTime
- status (AVAILABLE / BOOKED / COMPLETED)
- completedAt
- timestamps

## API Endpoints

Base URL:

```text
http://localhost:3000
```

### 1. Create User

**POST** `/users`

Creates a new user.

Request Body:

```json
{
  "fullName": "Vikash Kumar",
  "email": "vikash@gmail.com",
  "phone": "9876543210"
}
```

Email must be unique.

### 2. Create Session

**POST** `/sessions`

Creates a new session for an existing teacher.

Request Body:

```json
{
  "teacherId": "TEACHER_OBJECT_ID",
  "startTime": "2026-11-20T10:00:00.000Z",
  "endTime": "2026-11-20T11:00:00.000Z"
}
```

Default session status is `AVAILABLE`.

### 3. Get Available Sessions

**GET** `/sessions/available?dateTimestamp=TIMESTAMP`

Returns all available sessions for the requested date.

This API uses the MongoDB Aggregation Pipeline.

### 4. Book Session

**POST** `/sessions/:id/book`

Books an available session for an existing user.

Request Body:

```json
{
  "userId": "USER_OBJECT_ID"
}
```

Booking rules:

- User must exist.
- Session must exist.
- Only AVAILABLE sessions can be booked.
- A session cannot be booked more than once.
- Successful booking changes status to `BOOKED`.

### 5. Complete Session

**PATCH** `/sessions/:id/complete`

Marks a booked session as completed.

Rules:

- Session must exist.
- Only BOOKED sessions can be completed.
- Status changes to `COMPLETED`.
- Completion timestamp is saved in `completedAt`.

### 6. Get User Sessions

**GET** `/users/:id/sessions`

Returns the user's sessions in two separate categories:

- `upcomingSessions`
- `completedSessions`

This API uses the MongoDB Aggregation Pipeline.

## Session Status Flow

```text
AVAILABLE
    |
    | User books session
    v
  BOOKED
    |
    | Session completed
    v
COMPLETED
```

## Error Handling

The application uses centralized error handling and appropriate HTTP status codes for successful responses and errors.

Common HTTP status codes:

| Status Code | Description           |
| ----------- | --------------------- |
| 200         | OK                    |
| 201         | Created               |
| 400         | Bad Request           |
| 404         | Not Found             |
| 409         | Conflict              |
| 500         | Internal Server Error |

## Postman Collection

The Postman collection is available in the `postman/` directory.

Import the collection into Postman to test all API endpoints.

## Author

Vikash Kumar Singh
