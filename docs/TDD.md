# Technical Design Document (TDD)

## 1. Introduction

### 1.1 Purpose

This document describes the technical architecture and implementation plan for Quiz Game.

### 1.2 Technology Stack

* Frontend: React.js with TypeScript
* Backend: FastAPI with Python
* Database: SQLite
* API communication: REST over HTTP
* Authentication: JWT bearer tokens
* Password security: Password hashing using a suitable password-hashing library
* Version control: Git and GitHub

## 2. System Architecture

The application follows a client-server architecture.

### Frontend

The React application provides registration, login, quiz listing, quiz management, quiz attempts, results, and administration pages.

### Backend

FastAPI exposes REST endpoints, validates requests, authenticates users, checks permissions, performs business logic, and communicates with the database.

### Database

SQLite stores user accounts, quizzes, questions, answer options, attempts, and submitted answers.

### Request Flow

1. The user interacts with the React frontend.
2. The frontend sends an HTTP request to a FastAPI endpoint.
3. FastAPI validates the request and authenticates the user when required.
4. Authorization dependencies check roles and security scopes.
5. Business logic validates resource ownership and performs the operation.
6. The backend reads or updates the database.
7. FastAPI returns a JSON response and an appropriate HTTP status code.
8. React displays the result.

## 3. Proposed Project Structure

```text
quiz-game/
├── docs/
│   ├── PRD.md
│   ├── SRS.md
│   └── TDD.md
├── frontend/
│   └── src/
│       ├── components/
│       ├── pages/
│       ├── services/
│       ├── types/
│       ├── App.tsx
│       └── main.tsx
├── backend/
│   └── app/
│       ├── routers/
│       ├── models/
│       ├── schemas/
│       ├── services/
│       ├── dependencies/
│       ├── database.py
│       ├── security.py
│       └── main.py
└── README.md
```

## 4. Authentication Design

1. A user submits credentials to the login endpoint.
2. The backend verifies the credentials against the stored password hash.
3. If valid, the backend issues a signed JWT containing the user identity and appropriate authorization claims.
4. The frontend uses the access token when calling protected APIs.
5. FastAPI validates the token and identifies the current user.
6. Authorization dependencies verify the required scopes and role.
7. Resource ownership checks are performed before accessing or modifying user-owned resources.

Passwords must never be stored as plain text. Tokens must have an appropriate expiration time. JWT signing secrets must be kept outside source control.

## 5. Role-Based Access Control (RBAC)

### Student

* View published quizzes.
* Submit quiz attempts.
* View personal results.

### Teacher

* View published quizzes.
* Create quizzes.
* Edit, publish, and delete owned quizzes.
* Manage questions in owned quizzes.
* View results for owned quizzes.

### Admin

* View and manage user accounts.
* Activate or deactivate accounts.
* Assign roles.
* Perform authorized administrative operations.

Role checks alone are not enough for all operations. The backend must also check quiz ownership and result ownership.

## 6. Security Scopes and Permissions

Proposed OAuth2-compatible scopes:

* `quiz:read` — Read published quizzes.
* `quiz:create` — Create quizzes.
* `quiz:update_own` — Modify quizzes owned by the current teacher.
* `quiz:delete_own` — Delete quizzes owned by the current teacher.
* `attempt:submit` — Submit a quiz attempt.
* `result:read_own` — Read the current user's results.
* `result:read_quiz` — Read results for quizzes owned by the current teacher.
* `user:manage` — Manage user accounts.
* `role:assign` — Assign user roles.

### Scope Assignment

* Student: `quiz:read`, `attempt:submit`, `result:read_own`
* Teacher: `quiz:read`, `quiz:create`, `quiz:update_own`, `quiz:delete_own`, `result:read_quiz`
* Admin: `user:manage`, `role:assign`

Additional scopes can be granted when needed. For example, the application may allow administrators to inspect published quizzes through `quiz:read`.

In FastAPI, scopes can be declared using OAuth2 security utilities and enforced with dependencies. Scope checks must be combined with resource ownership checks. The `update_own` and `delete_own` scopes do not independently prove ownership.

## 7. REST API Design

### Authentication

| Method | Endpoint             | Purpose                      | Access        |
| ------ | -------------------- | ---------------------------- | ------------- |
| POST   | `/api/auth/register` | Register a user              | Public        |
| POST   | `/api/auth/token`    | Log in and receive a token   | Public        |
| GET    | `/api/auth/me`       | Get current user information | Authenticated |

### Quizzes

| Method | Endpoint                         | Purpose                | Access        |
| ------ | -------------------------------- | ---------------------- | ------------- |
| GET    | `/api/quizzes`                   | List published quizzes | Authenticated |
| GET    | `/api/quizzes/{quiz_id}`         | Get quiz details       | Authenticated |
| POST   | `/api/quizzes`                   | Create a quiz          | Teacher       |
| PUT    | `/api/quizzes/{quiz_id}`         | Update an owned quiz   | Teacher/owner |
| DELETE | `/api/quizzes/{quiz_id}`         | Delete an owned quiz   | Teacher/owner |
| POST   | `/api/quizzes/{quiz_id}/publish` | Publish an owned quiz  | Teacher/owner |

### Questions

| Method | Endpoint                           | Purpose           | Access        |
| ------ | ---------------------------------- | ----------------- | ------------- |
| POST   | `/api/quizzes/{quiz_id}/questions` | Add a question    | Teacher/owner |
| PUT    | `/api/questions/{question_id}`     | Update a question | Teacher/owner |
| DELETE | `/api/questions/{question_id}`     | Delete a question | Teacher/owner |

### Attempts and Results

| Method | Endpoint                          | Purpose           | Access        |
| ------ | --------------------------------- | ----------------- | ------------- |
| POST   | `/api/quizzes/{quiz_id}/attempts` | Submit answers    | Student       |
| GET    | `/api/results/me`                 | View own results  | Student       |
| GET    | `/api/quizzes/{quiz_id}/results`  | View quiz results | Teacher/owner |

### User Administration

| Method | Endpoint                            | Purpose               | Access |
| ------ | ----------------------------------- | --------------------- | ------ |
| GET    | `/api/admin/users`                  | List user accounts    | Admin  |
| PATCH  | `/api/admin/users/{user_id}/status` | Change account status | Admin  |
| PATCH  | `/api/admin/users/{user_id}/role`   | Assign a role         | Admin  |

These endpoints describe the planned API. Request schemas, response schemas, and exact permission dependencies will be defined during implementation.

## 8. Database Design

### User

Stores account details, password hashes, role, and account status.

### Quiz

Stores quiz information, publication status, and the ID of the teacher who owns it.

### Question

Stores question text and its parent quiz ID.

### Option

Stores answer choices and the correct-answer indicator.

### Attempt

Stores the student, quiz, score, and submission timestamp.

### SubmittedAnswer

Stores the selected option for each question in an attempt.

Foreign keys will maintain relationships between related records. Database constraints and backend validation will be used where appropriate.

## 9. Request and Response Handling

The API will use JSON for request and response bodies, except for standard form-encoded login requests when required by the OAuth2 password-bearer flow.

Common HTTP status codes:

* `200 OK` — Successful read or update.
* `201 Created` — Resource created.
* `204 No Content` — Successful deletion.
* `400 Bad Request` — Invalid operation.
* `401 Unauthorized` — Missing or invalid authentication.
* `403 Forbidden` — Insufficient permissions.
* `404 Not Found` — Resource not found.
* `422 Unprocessable Entity` — Request validation failure.

## 10. Security Considerations

* Hash passwords using an appropriate password-hashing algorithm.
* Use signed, expiring authentication tokens.
* Keep signing secrets and configuration outside version control.
* Validate and authorize every protected request on the backend.
* Do not trust roles or scores sent by the frontend.
* Never return correct answers in the student-facing quiz response before submission.
* Verify resource ownership before quiz edits, deletions, and result access.
* Prevent unauthorized role escalation.
* Return only necessary user information in API responses.
* Use HTTPS in production.
* Test invalid tokens, insufficient scopes, inactive accounts, and unauthorized resource access.

## 11. Frontend-Backend Integration

The frontend will use a centralized API service to call FastAPI endpoints.

TypeScript interfaces will represent users, quizzes, questions, attempts, and results. The frontend will handle loading states, form validation, API errors, and navigation.

Role-specific pages and buttons will improve usability, but the backend will remain responsible for enforcing all security rules.

## 12. Testing Strategy

### Authentication Tests

* Successful registration and login.
* Invalid credentials.
* Missing, invalid, and expired tokens.

### Authorization Tests

* Student attempting to create a quiz.
* Teacher editing another teacher's quiz.
* Student accessing another student's results.
* Non-admin attempting to assign roles.
* Inactive user attempting to access protected endpoints.

### Functional Tests

* Creating and publishing a quiz.
* Adding questions and options.
* Submitting answers.
* Calculating scores correctly.
* Displaying personal results.

### Integration Tests

Verify that the React frontend can communicate with FastAPI and correctly handle successful responses and API errors.

## 13. Implementation Plan

1. Set up the repository and frontend/backend environments.
2. Implement database models and authentication.
3. Implement role checks, scopes, and ownership validation.
4. Implement quiz and question management.
5. Implement quiz attempts and score calculation.
6. Build the React pages and connect them to REST APIs.
7. Test functionality, permissions, and error handling.
8. Update the documentation to reflect the final implementation.

## 14. Design Constraints

The project will prioritize a complete and secure core application over optional features. AI, machine learning, live multiplayer, and complex analytics are excluded from the initial implementation.
