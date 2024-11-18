# **Chapter 3: Manually Testing APIs**

#### **3.1. Introduction to API Testing**

API testing involves verifying that an API behaves as expected under different conditions. Testing an API manually allows you to ensure that it responds correctly to various requests, handles errors appropriately, and returns the expected data.

In this chapter, we’ll walk through the fundamentals of manually testing APIs. You’ll learn about **GET**, **POST**, **PUT**, and **DELETE** requests and how to test them using common tools like **Postman** and **cURL**.

#### **3.2. What Is Manual API Testing?**

Manual API testing is the process of interacting with an API directly (without automation scripts) to verify its functionality. Manual testing is usually the first step in verifying that an API works before automating the tests.

Key goals of manual API testing include:
- **Verify the API behaves as expected:** Does the API return the correct data?
- **Validate response codes:** Are the correct HTTP status codes returned?
- **Check data integrity:** Does the API return the correct data in the correct format?
- **Test error handling:** Does the API handle invalid requests or unexpected conditions properly?

#### **3.3. Types of HTTP Methods to Test**

When manually testing APIs, you will interact with different HTTP methods. The most common methods are:

- **GET:** Retrieve data from the API. Typically used to fetch data.
- **POST:** Send data to the server, often used for creating resources (e.g., creating a user).
- **PUT:** Update an existing resource with new data.
- **DELETE:** Remove a resource.

Each of these methods performs a specific function, and testing them ensures that your API can handle all CRUD operations (Create, Read, Update, Delete).

Let’s explore how to test each method.

#### **3.4. Testing a GET Request**

A **GET** request is used to retrieve information from the server. When testing a **GET** request, you want to ensure that the API returns the correct data.

**Steps to manually test a GET request:**
1. Identify the endpoint for the **GET** request.
2. Send a **GET** request to the endpoint.
3. Verify the response:
   - Ensure the correct HTTP status code is returned (e.g., `200 OK`).
   - Check the response body to ensure it contains the expected data (usually in JSON format).

For example, testing a **GET /users** request might look like this:

**Request:**
```bash
GET http://localhost:8080/users
```

**Response:**
```json
[
  {
    "id": 1,
    "name": "John Doe",
    "email": "john.doe@example.com",
    "age": 30
  }
]
```

You should verify that the data returned in the response is correct, such as checking the values of `"id"`, `"name"`, `"email"`, and `"age"`.

#### **3.5. Testing a POST Request**

A **POST** request is used to create a new resource on the server. You send data in the request body, and the API should create the resource based on that data.

**Steps to manually test a POST request:**
1. Identify the endpoint where a new resource can be created.
2. Provide the necessary data in the request body (usually in JSON format).
3. Send a **POST** request to create the resource.
4. Verify the response:
   - Ensure that the response status code is `201 Created` or another appropriate status code.
   - Check the response body to confirm that the newly created resource is returned (e.g., the `id` of the new user).

**Example:**

**Request:**
```bash
POST http://localhost:8080/users
Content-Type: application/json

{
  "name": "Jane Smith",
  "email": "jane.smith@example.com",
  "age": 25
}
```

**Response:**
```json
{
  "id": 2,
  "name": "Jane Smith",
  "email": "jane.smith@example.com",
  "age": 25
}
```

Here, you should verify that the newly created user object is returned, and the `id` field is assigned a value automatically by the server.

#### **3.6. Testing a PUT Request**

A **PUT** request is used to update an existing resource on the server. When testing a **PUT** request, you will send the updated data in the request body.

**Steps to manually test a PUT request:**
1. Identify the endpoint for the resource you wish to update (e.g., `/users/{id}`).
2. Send a **PUT** request to update the resource.
3. Verify the response:
   - Ensure the correct HTTP status code (`200 OK` or `204 No Content`) is returned.
   - Confirm that the resource is updated correctly.

**Example:**

**Request:**
```bash
PUT http://localhost:8080/users/2
Content-Type: application/json

{
  "name": "Jane Smith",
  "email": "jane.smith_updated@example.com",
  "age": 26
}
```

**Response:**
```json
{
  "id": 2,
  "name": "Jane Smith",
  "email": "jane.smith_updated@example.com",
  "age": 26
}
```

Here, you would verify that the updated user has the new `email` and `age` values.

#### **3.7. Testing a DELETE Request**

A **DELETE** request is used to remove a resource from the server. When testing a **DELETE** request, you need to confirm that the resource is removed and that the correct status code is returned.

**Steps to manually test a DELETE request:**
1. Identify the endpoint for the resource to be deleted (e.g., `/users/{id}`).
2. Send a **DELETE** request to delete the resource.
3. Verify the response:
   - Ensure the correct HTTP status code (`204 No Content`) is returned.
   - Send a **GET** request afterward to ensure that the resource no longer exists.

**Example:**

**Request:**
```bash
DELETE http://localhost:8080/users/2
```

**Response:**
```text
204 No Content
```

Then, send a **GET** request to confirm the resource is deleted:

```bash
GET http://localhost:8080/users/2
```

**Response:**
```json
{
  "error": "User not found"
}
```

This confirms that the user with `id = 2` has been deleted.

#### **3.8. Tools for Manual API Testing**

While you can test APIs using **cURL** directly in the terminal, tools like **Postman** make it much easier to interact with and visualize the data returned from APIs. Here’s how to use each of them:

##### **3.8.1. Postman**
Postman is a popular API testing tool that provides a user-friendly interface for making requests, viewing responses, and testing APIs interactively.

- **Create a new request:** Click the **New Request** button and choose the method (GET, POST, PUT, DELETE).
- **Enter the URL:** Specify the API endpoint.
- **Set headers (optional):** Specify `Content-Type` or other necessary headers.
- **Provide request body (for POST/PUT):** In the body section, input the request data in **JSON** format.
- **View the response:** Once you send the request, Postman will show the status code, response body, headers, and other details.

##### **3.8.2. cURL**
cURL is a command-line tool for transferring data with URLs. It is very useful for quickly testing APIs directly in the terminal.

**Example of using cURL for a GET request:**
```bash
curl http://localhost:8080/users
```

**Example of using cURL for a POST request:**
```bash
curl -X POST http://localhost:8080/users \
     -H "Content-Type: application/json" \
     -d '{"name": "Jane Smith", "email": "jane.smith@example.com", "age": 25}'
```

### **3.9. Common Issues When Manually Testing APIs**

When manually testing APIs, you may encounter a few common issues:

- **Incorrect endpoints:** Always check the endpoint path carefully.
- **Authentication errors:** Ensure you’re passing the correct authentication tokens or credentials (if required).
- **Invalid request data:** Ensure that the data you send in the request matches the API’s expected format.
- **Unexpected response codes:** Always check the HTTP status codes to verify the result of your request (e.g., `200 OK`, `400 Bad Request`, `404 Not Found`, `500 Internal Server Error`).

### **3.10. Hands-On Exercise: Manual API Testing**

Now that you have an understanding of the basic steps involved in manual API testing, it’s time to practice:

1. Use **Postman** or **cURL** to test the **GET**, **POST**, **PUT**, and **DELETE** requests on the `/users` endpoint of your Go application.
2. Verify the correctness of responses for each HTTP method.
3. Test edge cases, such as invalid user IDs, missing required fields, or sending incorrect data formats.

---

### **Summary of Chapter 3**

In this chapter, you learned the following:

- **Manual API Testing** involves sending HTTP requests directly to an API and verifying the responses.
- **HTTP Methods:** You learned how to test the most common HTTP methods: GET, POST, PUT, and DELETE.
- **Tools:** You explored how to use tools like **Postman** and **cURL** for testing APIs
----

[>> Next Chapter](chapter4.md)