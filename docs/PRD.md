Product Requirements Document (PRD)

1. Project Information
   
 Project Name: 2311971-Quiz-Game
 Document: Product Requirements Document
 Version: 1.0
 Status: Draft
 Repository: https://github.com/karima0108/2311971-Quiz-Game

3. Product Overview
   
Quiz Game is a web application that allows students to attempt online multiple-choice quizzes and view their scores. Teachers can create and manage quizzes, while administrators can manage user accounts and assign roles.
The frontend will use React.js with TypeScript, and the backend will use FastAPI with Python. REST APIs will connect the frontend and backend.

3. Problem Statement

Students need a simple way to attempt quizzes and review their results. Teachers need a platform to create questions and publish quizzes. Administrators need to manage accounts and control access.
Quiz Game will provide these functions through one secure web application.

4. Objectives
   
  * Implement secure registration and login.
  * Allow students to attempt published quizzes.
  * Calculate quiz scores automatically.
  * Allow teachers to create and manage their own quizzes.
  * Allow administrators to manage accounts and assign roles.
  * Demonstrate REST APIs, authentication, authorization, RBAC, and security scopes.

6. Target Users

Student: Can view published quizzes, submit answers, and view personal results.

Teacher: Can create quizzes, manage questions, publish quizzes, and view results for their own quizzes.

Admin: Can manage user accounts, activate or deactivate accounts, and assign roles.

6. Functional Scope
   
Authentication:
  * Register and log in.
  * Store passwords as secure hashes.
  * Issue and validate authentication tokens.
  * Reject invalid or expired tokens.
  
Quiz Management:
  * Create quizzes with a title and description.
  * Add multiple-choice questions and options.
  * Edit and delete owned quizzes.
  * Publish or unpublish owned quizzes.
  
Quiz Participation:
  * View published quizzes.
  * Submit answers.
  * Calculate scores on the backend.
  * View personal results.
  
Administration:
  * View user accounts.
  * Change account status.
  * Assign user roles.
  * Access Control
  * Students cannot create quizzes.
  * Teachers can manage only their own quizzes.
  * Students cannot access other students' private results.
  * Only authorized administrators can assign roles.
  
8. Non-Functional Requirements
   * The interface should be simple and responsive.
   * The backend should validate all requests.
   * Protected APIs must enforce authentication and authorization.
   * Sensitive information must not be exposed in responses.
   * The application should be modular and maintainable.
     
9. Technology Stack
  Frontend: React.js and TypeScript
  Backend: FastAPI and Python
  Database: SQLite
  API: REST
  Authentication: JWT bearer tokens
  Version control: Git and GitHub

11. Out of Scope

The first version will not include AI, machine learning, live multiplayer, payments, chat, or complex analytics.

10. Success Criteria

The project is successful when users can log in, roles are enforced, teachers can publish quizzes, students can submit answers and view scores, and unauthorized API requests are rejected.

11. Constraints

The application must be completed within the course timeline. The initial implementation will focus on multiple-choice quizzes and essential security features.
