# **Go Inventory Management API**

This project is a complete, production-grade backend server for an Inventory Management System, built entirely in Go. It features a secure, high-performance RESTful API for all standard CRUD operations, backed by a PostgreSQL database.

The API incorporates advanced features including pagination, sorting, filtering, rate-limiting, in-memory caching for performance, and JWT-based authentication for security. The project is fully documented with Swagger UI.

## **Features**

- **Secure RESTful API:** Full CRUD functionality for inventory items, with data modification endpoints protected by JWT authentication.
- **PostgreSQL Database:** Utilizes a robust, persistent SQL database for data storage, managed with the GORM ORM.
- **Advanced Querying:** Supports pagination (`?page=`, `?pageSize=`), sorting (`?sort_by=`, `?sort_order=`), and filtering (`?name=`, `?min_stock=`) on the main inventory endpoint.
- **High Performance:** Implements in-memory caching to reduce database load and provide near-instant responses for frequently accessed data. Cache is automatically invalidated upon data modification.
- **Security:**
    - **Authentication:** A full user registration and login system using secure password hashing (`bcrypt`) and JSON Web Tokens (JWT).
    - **Rate Limiting:** Protects the API from abuse with a token-bucket rate limiter (1 request/second with a burst of 5).
- **Interactive API Documentation:** Automatically generated, interactive API documentation is available via Swagger UI.

## **Technologies Used**

- **Language:** Go
- **API Framework:** Gin
- **Database:** PostgreSQL
- **ORM:** GORM
- **Authentication:** `jwt-go` for JWTs, `bcrypt` for password hashing
- **Caching:** `go-cache`
- **Documentation:** `gin-swagger`, `swaggo`

## **API Endpoints**

### **Authentication Routes**

| **Method** | **Path** | **Description** |
| --- | --- | --- |
| `POST` | `/auth/register` | Creates a new user account. |
| `POST` | `/auth/login` | Logs in a user and returns a JWT. |

### **Inventory Routes**

| **Method** | **Path** | **Description** | **Authentication** |
| --- | --- | --- | --- |
| `GET` | `/inventory` | Retrieves a list of all inventory items. | **Public** |
| `GET` | `/inventory/{id}` | Retrieves a single inventory item by ID. | **Public** |
| `POST` | `/inventory` | Adds a new item to the inventory. | **Required** |
| `PUT` | `/inventory/{id}` | Updates an existing item's data. | **Required** |
| `DELETE` | `/inventory/{id}` | Deletes an item by its ID. | **Required** |

### **Documentation Route**

| **Method** | **Path** | **Description** |
| --- | --- | --- |
| `GET` | `/swagger/index.html` | Renders the interactive Swagger UI. |

## **Local Development Setup**

### **Prerequisites**

- [Go](https://golang.org/doc/install) (version 1.24 or higher)
- [PostgreSQL](https://www.postgresql.org/)
- The `swag` CLI tool (`go install github.com/swaggo/swag/cmd/swag@latest`)
- A tool like [Postman](https://www.postman.com/downloads/) to test the API.

### **Instructions**

1. **Clone the Repository:**
    
    ```
    git clone <your-github-repo-url>
    cd <your-repo-name>
    
    ```
    
2. **Install Dependencies:**
    
    ```
    go mod tidy
    
    ```
    
3. **Set Up Local Database:**
    - Start your local PostgreSQL service (`brew services start postgresql`).
    - Use `psql` to create a new database named `postgres` if it doesn't exist.
4. **Generate Swagger Docs:**
    
    ```
    swag init
    
    ```
    
5. **Run the Server:**
    
    ```
    go run main.go
    
    ```
    
    The server will start on `http://localhost:8080`.
    

## **How to Use the API**

1. **Register a User:**
    - `POST /auth/register` with `{"username": "testuser", "password": "password123"}`
2. **Log In to Get a Token:**
    - `POST /auth/login` with `{"username": "testuser", "password": "password123"}`
    - Copy the `token` from the response body.
3. **Make a Protected Request:**
    - To `POST`, `PUT`, or `DELETE` an item, go to the **Authorization** tab in Postman.
    - Select **Type: Bearer Token**.
    - Paste the copied token into the "Token" field.
    - Send your request.