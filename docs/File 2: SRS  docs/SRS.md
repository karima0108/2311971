# Software Requirements Specification (SRS)

## 1. Introduction

### 1.1 Purpose

This document defines the functional and non-functional requirements of the Quiz Game application.

### 1.2 Scope

The application supports authentication, quiz management, quiz attempts, score calculation, results, and user administration.

### 1.3 Intended Users

Students, teachers, administrators, developers, and project evaluators.

## 2. System Overview

The application consists of a React.js frontend, a FastAPI backend, and an SQLite database. The frontend communicates with the backend through REST APIs.

## 3. User Roles

* Student: attempts quizzes and views personal results.
* Teacher: creates and manages owned quizzes and views their quiz results.
* Admin: manages user accounts and assigns roles.

## 4. Functional Requirements

### FR-01: Registration

The system shall allow users to register using required account information.

### FR-02: Login

The system shall verify credentials and issue an authentication token when login succeeds.

### FR-03: Authentication

Protected endpoints shall reject requests with missing, invalid, or expired tokens.

### FR-04: Role-Based Access Control

The system shall restrict operations according to the authenticated user's role and permissions.

### FR-05: Quiz Listing

The system shall display published quizzes to authenticated users.

### FR-06: Quiz Creation

Authorized teachers shall be able to create quizzes with titles and descriptions.

### FR-07: Question Management

Teachers shall be able to add, update, and delete questions and answer options in their own quizzes.

### FR-08: Quiz Publication

Teachers shall be able to publish or unpublish their own quizzes.

### FR-09: Quiz Submission

Students shall be able to submit answers to published quizzes.

### FR-10: Score Calculation

The backend shall calculate scores using the correct answers stored in the database.

### FR-11: Results

Students shall be able to view their own results. Teachers shall be able to view results for quizzes they own.

### FR-12: User Management

Administrators shall be able to view user accounts and activate or deactivate accounts.

### FR-13: Role Assignment

Only authorized administrators shall be able to assign user roles.

### FR-14: Authorization Validation

The backend shall check the user's permissions and resource ownership before protected operations.

## 5. Non-Functional Requirements

* Security: Passwords must be hashed, and protected endpoints must enforce authorization.
* Usability: The application must provide clear navigation and useful error messages.
* Reliability: Invalid requests must be handled without crashing the application.
* Privacy: Students must not access other students' private results.
* Maintainability: Frontend, API routes, business logic, and database code should be separated.
* Data validation: Invalid or incomplete requests must be rejected.

## 6. Use Cases

### UC-01: Student Attempts a Quiz

1. Student logs in.
2. Student views published quizzes.
3. Student selects a quiz.
4. Student submits answers.
5. Backend validates answers and calculates the score.
6. Student views the result.

### UC-02: Teacher Creates a Quiz

1. Teacher logs in.
2. Teacher creates a quiz.
3. Teacher adds questions and answer options.
4. Teacher publishes the quiz.
5. Students can access the published quiz.

### UC-03: Admin Manages Users

1. Admin logs in.
2. Admin opens user management.
3. Admin selects an account.
4. Admin updates account status or role.
5. Backend verifies permissions before saving the change.

## 7. Data Requirements

### User

* id
* username
* email
* password_hash
* role
* is_active

### Quiz

* id
* title
* description
* owner_id
* is_published

### Question

* id
* quiz_id
* question_text

### Option

* id
* question_id
* option_text
* is_correct

### Attempt

* id
* quiz_id
* student_id
* score
* submitted_at

### SubmittedAnswer

* id
* attempt_id
* question_id
* selected_option_id

## 8. Business Rules

* Students may attempt only published quizzes.
* The backend determines the final score.
* Correct answers must not be exposed through student-facing quiz APIs before submission.
* Teachers may modify only quizzes they own.
* Students may view only their own private results.
* Users cannot assign themselves additional privileges.

## 9. Acceptance Criteria

* Registration and login work correctly.
* Invalid credentials are rejected.
* Student, Teacher, and Admin permissions are enforced.
* Teachers can create and publish quizzes.
* Students can submit answers and receive correct scores.
* Students cannot access other students' results.
* Unauthorized API requests are rejected.
