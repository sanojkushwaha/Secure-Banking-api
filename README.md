# 🏦 Banking & Finance REST API

<p align="center">
  A secure, RESTful backend for a modern Banking & Finance Rest Apis.
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Java-25-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white" alt="Java 25">
  <img src="https://img.shields.io/badge/Spring%20Boot-4-6DB33F?style=for-the-badge&logo=springboot&logoColor=white" alt="Spring Boot 4">
  <img src="https://img.shields.io/badge/MySQL-8+-4479A1?style=for-the-badge&logo=mysql&logoColor=white" alt="MySQL">
  <img src="https://img.shields.io/badge/API-OpenAPI%203.1-6BA539?style=for-the-badge&logo=swagger&logoColor=white" alt="OpenAPI 3.1">
</p>

<p align="center">
  <a href="#quick-start">Quick start</a> ·
  <a href="#api-reference">API reference</a> ·
  <a href="#documentation">Swagger UI</a> ·
<a href="#security">Security</a>
</p>

<p align="center">
  <a href="http://localhost:9090/api/swagger-ui/index.html">
    <img src="https://img.shields.io/badge/Open%20Swagger%20UI-85EA2D?style=for-the-badge&logo=swagger&logoColor=black" alt="Open Swagger UI">
  </a>
</p>

## Overview

A secure and feature-rich banking REST API built with Spring Boot 3 and JWT authentication. The project supports user registration, login, and a full set of banking operations including account management, deposits, withdrawals, and transfers. All financial transactions are tracked and securely protected with stateless JWT-based authorization.


## INTRODUCTION

Secure-Banking-api is a backend banking system that manages users, accounts, and transactions. It delivers a robust authentication mechanism, RESTful endpoints for all core banking actions, and comprehensive documentation through Swagger/OpenAPI. The codebase is organized for clarity and maintainability with clear separation of concerns between controllers, services, DTOs, and entities.

> [!NOTE]
> Swagger UI and interactive API docs are available at `http://localhost:8080/swagger-ui.html` after running the application.

---

## Features

- **User Registration & Authentication**
  Register new users and authenticate existing ones. JWT tokens are issued on successful login for stateless security.

- **Account Management**
  Each user owns a unique account; admins can view all accounts.

- **Transactions**
  Supports deposits, withdrawals, and transfers between accounts, with a complete transaction log.

- **Role-Based Access Control**
  Users see only their own data; admins have system-wide visibility.

- **Exception Handling**
  All endpoints return consistent, structured responses. Errors are handled globally.

- **Secure by Default**
  All non-auth routes require a valid JWT token; passwords use BCrypt hashing.

- **Interactive API Docs**
  OpenAPI (Swagger) integration for testing and exploring endpoints.

---

## Requirements

- Java 17 or higher
- Maven (for building the project)
- No external database required (H2 in-memory DB is used for development and testing)

---

## Installation

Follow the steps below to get the API running locally:

```steps
1. Clone the repository | git clone https://github.com/sanojkushwaha/Secure-Banking-api.git
2. Navigate to the project folder | cd Secure-Banking-api
3. Build the project using Maven | mvn clean install
4. Run the application | mvn spring-boot:run
```

> [!TIP]
> After startup, visit `http://localhost:8080/swagger-ui.html` to test the API interactively.

---

## Usage

The API exposes endpoints for registration, login, accounts, and transactions. Protected routes require a valid JWT in the `Authorization` header.

### Authentication Endpoints

#### Register a New User (POST /api/auth/register)

Registers a new user and creates a bank account automatically.

```api
{
    "title": "Register User",
    "description": "Creates a new user and associated bank account. Returns a JWT token.",
    "method": "POST",
    "baseUrl": "http://localhost:8080",
    "endpoint": "/api/auth/register",
    "headers": [],
    "bodyType": "json",
    "requestBody": "{\n  \"fullName\": \"Jane Doe\",\n  \"email\": \"jane@example.com\",\n  \"password\": \"securePass123\",\n  \"phoneNumber\": \"1234567890\"\n}",
    "responses": {
        "201": {
            "description": "Registration successful",
            "body": "{\n  \"success\": true,\n  \"message\": \"Registration successful! Your bank account has been created.\",\n  \"data\": {\n    \"token\": \"<JWT>\",\n    \"tokenType\": \"Bearer\",\n    \"email\": \"jane@example.com\",\n    \"fullName\": \"Jane Doe\",\n    \"role\": \"ROLE_USER\"\n  },\n  \"timestamp\": \"2024-01-01T10:00:00\"\n}"
        },
        "400": {
            "description": "Validation error or duplicate resource",
            "body": "{\n  \"success\": false,\n  \"message\": \"Email is already registered\",\n  \"timestamp\": \"2024-01-01T10:00:00\"\n}"
        }
    }
}
```

#### Login (POST /api/auth/login)

Authenticates a user and returns a JWT token.

```api
{
    "title": "Login",
    "description": "Authenticates with email and password. Returns a JWT token valid for 24 hours.",
    "method": "POST",
    "baseUrl": "http://localhost:8080",
    "endpoint": "/api/auth/login",
    "headers": [],
    "bodyType": "json",
    "requestBody": "{\n  \"email\": \"jane@example.com\",\n  \"password\": \"securePass123\"\n}",
    "responses": {
        "200": {
            "description": "Login successful",
            "body": "{\n  \"success\": true,\n  \"message\": \"Login successful!\",\n  \"data\": {\n    \"token\": \"<JWT>\",\n    \"tokenType\": \"Bearer\",\n    \"email\": \"jane@example.com\",\n    \"fullName\": \"Jane Doe\",\n    \"role\": \"ROLE_USER\"\n  },\n  \"timestamp\": \"2024-01-01T10:00:00\"\n}"
        },
        "401": {
            "description": "Invalid credentials",
            "body": "{\n  \"success\": false,\n  \"message\": \"Invalid email or password\",\n  \"timestamp\": \"2024-01-01T10:00:00\"\n}"
        }
    }
}
```

### Account Endpoints

> [!IMPORTANT]
> All account and transaction endpoints require an `Authorization: Bearer <token>` header.

#### Get My Account (GET /api/accounts/me)

Returns account details for the logged-in user.

```api
{
    "title": "Get My Account",
    "description": "Returns the bank account details of the logged-in user.",
    "method": "GET",
    "baseUrl": "http://localhost:8080",
    "endpoint": "/api/accounts/me",
    "headers": [
        {
            "key": "Authorization",
            "value": "Bearer <token>",
            "required": true
        }
    ],
    "bodyType": "none",
    "responses": {
        "200": {
            "description": "Account details",
            "body": "{\n  \"success\": true,\n  \"message\": \"Account details retrieved successfully.\",\n  \"data\": {\n    \"id\": 1,\n    \"accountNumber\": \"1234567890\",\n    \"balance\": 1000.00,\n    \"ownerName\": \"Jane Doe\",\n    \"ownerEmail\": \"jane@example.com\",\n    \"createdAt\": \"2024-01-01T10:00:00\"\n  },\n  \"timestamp\": \"2024-01-01T10:00:00\"\n}"
        },
        "401": {
            "description": "Unauthorized",
            "body": "{\n  \"success\": false,\n  \"message\": \"JWT token missing or invalid\",\n  \"timestamp\": \"2024-01-01T10:00:00\"\n}"
        }
    }
}
```

### Transaction Endpoints

#### Deposit, Withdraw, and Transfer

Initiate deposits, withdrawals, or transfers by POSTing to `/api/accounts/deposit`, `/api/accounts/withdraw`, or `/api/accounts/transfer` respectively. Provide transaction details in the request body.

> [!TIP]
> See Swagger UI for detailed request/response samples and try out endpoints directly.

---

## Configuration

- **Application Properties**
  The main application configuration is found in `src/main/resources/application.properties`.
  For test-specific configuration, see `src/test/resources/application-test.properties`.

- **Security**
  Public API routes: `/api/auth/**`, `/swagger-ui/**`, `/h2-console/**`
  All other endpoints are protected and require a JWT.

- **Swagger/OpenAPI**
  Documentation is enabled and configured via `src/main/java/com/banking/config/SwaggerConfig.java`.

---

## Contributing

Contributions are welcome! To propose enhancements or fixes:

- Fork the repository.
- Create a feature branch.
- Commit your changes.
- Open a pull request describing your changes.

> [!CAUTION]
> Ensure that all new features include relevant tests and documentation updates.

---

## 👤 Author and Developed by
**Sanoj Kumar Kushwaha**  
4th Year Computer Science Student  
GitHub: https://github.com/sanojkushwaha

---

## License

This repository is licensed under the [Apache-2.0](https://www.apache.org/licenses/LICENSE-2.0) license.
