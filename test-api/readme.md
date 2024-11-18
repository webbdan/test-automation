# User Management API

This is a simple Go application that provides a REST API for managing users. It uses Go 1.22's native multiplexer for routing and maintains data in memory. The application can be run locally or inside a Docker container for portability.

---

## **Features**

- **Endpoints** for managing users (`/users`, `/users/{id}`)
- Supports **GET**, **POST**, **PUT**, and **DELETE** operations.
- Built-in **in-memory database**.

---

## **Prerequisites**

- Go 1.22+ installed (if running locally).
- Docker installed (if using the container).

---

## **Run Locally**

1. **Clone the repository:**

   ```bash
   git clone <repository-url>
   cd user-management-api
   ```

2. **Run the application:**

   ```bash
   go run main.go
   ```

3. **Access the API:**

   The application runs on [http://localhost:8080](http://localhost:8080).

---

## **Run with Docker**

1. **Build the Docker image:**

   ```bash
   docker build -t user-management-api .
   ```

2. **Run the container:**

   ```bash
   docker run -p 8080:8080 user-management-api
   ```

3. **Access the API:**

   The application runs on [http://localhost:8080](http://localhost:8080).

---

## **API Documentation**

### **Base URL**

```plaintext
http://localhost:8080
```

---

### **Endpoints**

#### 1. **List All Users**

- **Method:** `GET`
- **Endpoint:** `/users/`
- **Description:** Retrieves a list of all users.
- **Request Body:** None.
- **Response:**
  ```json
  [
    {
      "id": 1,
      "name": "John Doe",
      "email": "john@example.com"
    }
  ]
  ```

---

#### 2. **Create a New User**

- **Method:** `POST`
- **Endpoint:** `/users/`
- **Description:** Creates a new user.
- **Request Body:**
  ```json
  {
    "name": "John Doe",
    "email": "john@example.com"
  }
  ```
- **Response:**
  ```json
  {
    "id": 1,
    "name": "John Doe",
    "email": "john@example.com"
  }
  ```

---

#### 3. **Get User by ID**

- **Method:** `GET`
- **Endpoint:** `/users/{id}/`
- **Description:** Retrieves a specific user by their ID.
- **Request Body:** None.
- **Response:**
  ```json
  {
    "id": 1,
    "name": "John Doe",
    "email": "john@example.com"
  }
  ```

---

#### 4. **Update User by ID**

- **Method:** `PUT`
- **Endpoint:** `/users/{id}/`
- **Description:** Updates a specific user by their ID.
- **Request Body:**
  ```json
  {
    "name": "Jane Doe",
    "email": "jane@example.com"
  }
  ```
- **Response:**
  ```json
  {
    "id": 1,
    "name": "Jane Doe",
    "email": "jane@example.com"
  }
  ```

---

#### 5. **Delete User by ID**

- **Method:** `DELETE`
- **Endpoint:** `/users/{id}/`
- **Description:** Deletes a specific user by their ID.
- **Request Body:** None.
- **Response:**
  - **Status Code:** `204 No Content`

---

## **Example Requests**

### Using `curl`

1. **List Users:**
   ```bash
   curl -X GET http://localhost:8080/users/
   ```

2. **Create a User:**
   ```bash
   curl -X POST -H "Content-Type: application/json" \
   -d '{"name":"John Doe","email":"john@example.com"}' \
   http://localhost:8080/users/
   ```

3. **Get User by ID:**
   ```bash
   curl -X GET http://localhost:8080/users/1/
   ```

4. **Update a User:**
   ```bash
   curl -X PUT -H "Content-Type: application/json" \
   -d '{"name":"Jane Doe","email":"jane@example.com"}' \
   http://localhost:8080/users/1/
   ```

5. **Delete a User:**
   ```bash
   curl -X DELETE http://localhost:8080/users/1/
   ```

---

## **Docker Cleanup**

After testing, clean up the Docker container:

```bash
docker stop $(docker ps -q --filter ancestor=user-management-api)
docker rm $(docker ps -aq --filter ancestor=user-management-api)
```
