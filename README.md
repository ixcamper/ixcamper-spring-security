# Spring Boot JWT Security Application

A secure Spring Boot REST API demonstrating JWT (JSON Web Token) authentication and role-based authorization using Spring Security.

## Features
* **Stateless Authentication**: Uses JWT tokens instead of server-side sessions.
* **Role-Based Access Control**: Restricts endpoints based on user roles (`USER`, `ADMIN`).
* **Secure Password Hashing**: Utilizes `BCryptPasswordEncoder`.

---

## API Endpoints


| Endpoint | HTTP Method | Access Level | Description |
| :--- | :--- | :--- | :--- |
| `/login` | `POST` | Public | Authenticates credentials and returns a JWT token. |
| `/hello` | `GET` | Public | Open endpoint accessible without authentication. |
| `/user` | `GET` | Authenticated | Accessible by users with `ROLE_USER` or `ROLE_ADMIN`. |
| `/admin` | `GET` | Authenticated | Strictly accessible only by users with `ROLE_ADMIN`. |

---

## Getting Started

### Prerequisites
* Java 17 or higher
* Maven 3.6+

### Installation & Running
1. Clone the repository:
   ```bash
   git clone https://github.com
   cd your-repo-name
   ```
2. Build the application:
   ```bash
   mvn clean install
   ```
3. Run the application:
   ```bash
   mvn spring-boot:run
   ```
The server will start on `http://localhost:8080`.

---

## How to Use

### 1. Authenticate (Login)
Send a `POST` request to `/login` with user credentials in the request body.

**Request:**
* URL: `http://localhost:8080/login`
* Body (JSON):
```json
{
  "username": "admin",
  "password": "password123"
}
```

**Response:**
```json
{
  "token": "eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJhZG1pbiIsImV4cCI6MTc4Mzk..."
}
```

### 2. Access Secure Endpoints
Copy the `token` string from the login response. Include it in the **Authorization** header of your subsequent requests as a Bearer token.

* **Header Key:** `Authorization`
* **Header Value:** `Bearer <your_jwt_token>`

#### Example: Accessing the Admin Endpoint via cURL
```bash
curl -X GET http://localhost:8080/admin \
  -H "Authorization: Bearer eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJhZG1pbiIsImV4cCI6MTc4Mzk..."
```

---

## Architecture Flow
1. **Client** sends credentials to `/login`.
2. **Authentication Filter** validates credentials and generates a JWT.
3. **Client** stores the JWT and sends it in the `Authorization` header for secure requests.
4. **JWT Authorization Filter** intercepts requests, validates the token, extracts roles, and populates the `SecurityContext`.
5. **Controller** processes the request if the roles match the required permissions.
