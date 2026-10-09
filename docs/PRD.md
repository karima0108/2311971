# Product Requirements Document (PRD)

## 1. Project Information

* **Project Name:** Quiz Game
* **Document:** Product Requirements Document
* **Version:** 1.0
* **Status:** Draft

## 2. Product Overview

Quiz Game is a secure web-based quiz platform where students can attempt quizzes and view their scores. Teachers can create and manage quizzes, while administrators can manage users and assign roles.

The application will be developed using React.js with TypeScript for the frontend and FastAPI with Python for the backend. REST APIs will connect the frontend and backend.

## 3. Problem Statement

Students need a simple platform to take online quizzes and check their results. Teachers need a convenient way to create quizzes and manage questions. Administrators need control over user accounts and access permissions.

Quiz Game aims to provide these features in one secure and easy-to-use application.

## 4. Project Objectives

* Provide secure user registration and login.
* Allow students to attempt available quizzes.
* Automatically calculate quiz scores.
* Allow teachers to create, edit, publish, and manage their own quizzes.
* Allow administrators to manage users and assign roles.
* Demonstrate REST API integration, authentication, authorization, RBAC, and security scopes.

## 5. Target Users

### Student

A student can view published quizzes, submit answers, and view personal results.

### Teacher

A teacher can create quizzes, manage questions, publish quizzes, and view results for their own quizzes.

### Administrator

An administrator can view and manage user accounts, assign roles, and oversee the platform.

## 6. Functional Scope

### 6.1 Authentication

* User registration and login.
* Secure password storage.
* Token-based authentication.
* Logout and appropriate handling of expired tokens.

### 6.2 Quiz Management

* Teachers can create quizzes with titles and descriptions.
* Teachers can add multiple-choice questions and answer options.
* Teachers can identify the correct answer.
* Teachers can edit, publish, and manage their own quizzes.

### 6.3 Quiz Participation

* Students can view published quizzes.
* Students can submit answers to quiz questions.
* The backend calculates the score.
* Students can view their own submitted results.

### 6.4 Administration

* Administrators can view user accounts.
* Administrators can activate or deactivate accounts.
* Administrators can assign permitted user roles.

### 6.5 Access Control

* Students cannot create or modify quizzes.
* Teachers cannot manage administrator accounts or assign roles.
* Teachers can manage only their own quizzes.
* Students can access only their own results.
* Administrative actions require appropriate backend permissions.

## 7. Non-Functional Requirements

* The interface should be responsive and easy to use.
* Passwords must not be stored in plain text.
* Protected APIs must verify authentication and authorization.
* Input data must be validated by the backend.
* Error messages should be clear without exposing sensitive information.
* The codebase should be modular and maintainable.

## 8. Technology Stack

* Frontend: React.js and TypeScript
* Backend: FastAPI and Python
* API style: REST
* Authentication: OAuth2-compatible bearer token flow using JWT
* Database: SQLite for the initial course-project implementation
* Password hashing: A suitable password-hashing library
* Version control: Git and GitHub

## 9. Out of Scope

The initial version will not include:

* Artificial intelligence or machine learning.
* Live multiplayer quizzes.
* Payment systems.
* Video calls or chat.
* Complex analytics or recommendation systems.

## 10. Success Criteria

The project will be considered successful when:

* Users can log in and receive valid authentication tokens.
* Each role can access only its permitted features.
* Teachers can create and publish quizzes.
* Students can attempt published quizzes and view their own scores.
* The backend calculates scores correctly.
* Unauthorized requests are rejected.
* The frontend communicates with the backend through REST APIs.

## 11. Assumptions and Constraints

* The application is developed as a course project.
* The team has limited development time.
* The first version uses multiple-choice questions.
* Features will be kept small enough to implement, test, and document within the course timeline.

