# Software Requirements Specification (SRS)

## 1. Introduction

### 1.1 Purpose

This document specifies the functional and non-functional requirements of Quiz Game, a secure online quiz application.

### 1.2 Scope

The system allows users to authenticate, access role-specific features, manage quizzes, submit answers, and view results.

### 1.3 Intended Users

Students, teachers, administrators, developers, and project evaluators.

## 2. Overall Description

### 2.1 Product Perspective

Quiz Game is a web application with a React.js and TypeScript frontend, a FastAPI backend, and a relational database.

### 2.2 User Classes

* Student
* Teacher
* Administrator

### 2.3 Operating Environment

The application will run in a modern web browser. The frontend and backend will communicate over HTTP using REST APIs.

## 3. Functional Requirements

### FR-01: User Registration

The system shall allow a user to register with the required account information. Passwords shall be hashed before storage.

### FR-02: User Login

The system shall validate user credentials and issue an authentication token when login succeeds.

### FR-03: Authentication

The backend shall validate the token before allowing access to protected endpoints.

### FR-04: Role-Based Authorization

The system shall distinguish Student, Teacher, and Admin roles and enforce the permissions assigned to each role.

### FR-05: Quiz Listing

The system shall allow authenticated users to view published quizzes.

### FR-06: Quiz Creation

The system shall allow authorized teachers to create quizzes with titles and descriptions.

### FR-07: Question Management

The system shall allow quiz owners to add, edit, and remove multiple-choice questions and answer options.

### FR-08: Quiz Publication

The system shall allow authorized teachers to publish or unpublish their own quizzes.

### FR-09: Quiz Submission

The system shall allow students to submit answers to published quizzes.

### FR-10: Score Calculation

The backend shall calculate the score by comparing submitted answers with the correct answers stored by the system.

### FR-11: Result Access

The system shall allow students to view their own results. Teachers shall be able to view results for their own quizzes.

### FR-12: User Administration

The system shall allow administrators to view user accounts and activate or deactivate accounts.

### FR-13: Role Assignment

The system shall allow only authorized administrators to assign user roles.

### FR-14: Permission Enforcement

The system shall reject requests when the authenticated user lacks the required role, scope, ownership, or permission.

## 4. Non-Functional Requirements

### NFR-01: Security

Passwords must be hashed. Protected endpoints must enforce authentication and authorization.

### NFR-02: Data Validation

The backend must validate request fields, question options, quiz identifiers, and submitted answers.

### NFR-03: Privacy

Students must not be able to view other students' private results.

### NFR-04: Reliability

The system must handle invalid requests and expected errors without crashing.

### NFR-05: Usability

The user interface must provide clear navigation, form validation, and feedback.

### NFR-06: Maintainability

Frontend components, API routes, business logic, and database models should be organized into separate modules.

## 5. Use Cases

### UC-01: Student Attempts a Quiz

1. The student logs in.
2. The student views published quizzes.
3. The student selects a quiz.
4. The student submits answers.
5. The backend validates the submission and calculates the score.
6. The student views the result.

### UC-02: Teacher Creates a Quiz

1. The teacher logs in.
2. The teacher opens the quiz management page.
3. The teacher creates a quiz.
4. The teacher adds questions and options.
5. The teacher publishes the quiz.
6. The quiz becomes available to students.

### UC-03: Administrator Manages Users

1. The administrator logs in.
2. The administrator opens user management.
3. The administrator views user accounts.
4. The administrator changes an account status or assigns a role.
5. The backend verifies administrative permissions before saving the change.

## 6. Authorization Requirements

* Students may read published quizzes, submit attempts, and read their own results.
* Teachers may create and manage quizzes they own and view their quiz results.
* Administrators may manage user accounts and assign roles.
* All users must authenticate before accessing protected operations.
* The backend must check resource ownership where applicable.
* A user must not gain additional privileges by modifying frontend state or request data.

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

### Submitted Answer

* id
* attempt_id
* question_id
* selected_option_id

Relationships:

* One teacher can own many quizzes.
* One quiz can contain many questions.
* One question can have many options.
* One student can have many attempts.
* One attempt can contain many submitted answers.

## 8. Constraints

* The first version supports multiple-choice quizzes.
* A student can submit an attempt only when the quiz is published.
* The backend determines the final score.
* The correct answers must not be exposed through the student-facing quiz API before submission.
* The system must enforce appropriate permissions on every protected endpoint.

## 9. Acceptance Criteria

The system meets its requirements when:

* Registration and login work correctly.
* Invalid credentials are rejected.
* Student, Teacher, and Admin permissions are enforced.
* Teachers can manage their own quizzes but not another teacher's quizzes.
* Students can submit quizzes and receive correctly calculated scores.
* Students cannot access other students' results.
* Unauthorized and insufficiently privileged API requests are rejected.
