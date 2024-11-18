# **Chapter 1: Introduction to APIs and HTTP Basics**

#### **1.1. What is an API?**

API stands for **Application Programming Interface**. It is a set of rules and protocols that allows different software applications to communicate with each other. Think of an API as a **messenger** that takes requests from one application, sends them to another, and then returns the response.

In the context of web applications, **Web APIs** are typically used to allow different systems (like a client and a server) to communicate over the internet. Web APIs often follow the **RESTful** design principles, which we'll explore in more detail later.

#### **1.2. What is REST?**

REST (Representational State Transfer) is a set of architectural principles for designing networked applications. RESTful APIs use **HTTP** as the protocol for communication and are stateless, meaning each request from a client contains all the information necessary to complete the request.

In RESTful API design:
- Resources are represented as URLs (Uniform Resource Locators).
- Each URL typically corresponds to an entity or collection of entities.
- The HTTP methods **GET**, **POST**, **PUT**, **DELETE**, etc., are used to perform operations on these resources.

For example:
- **GET /users** could retrieve a list of users.
- **POST /users** could create a new user.
- **PUT /users/{id}** could update an existing user.
- **DELETE /users/{id}** could delete a user.

#### **1.3. The Basics of HTTP**

HTTP (Hypertext Transfer Protocol) is the foundational protocol used for transferring data across the web. When you make a request to an API, the request and response follow the HTTP protocol. Let’s break down some key HTTP concepts.

##### **1.3.1. HTTP Methods**

When you interact with an API, you'll use one of the following HTTP methods to request or manipulate data:

- **GET:** Retrieve data from the server. It’s used when you want to fetch resources.
  - Example: `GET /users` retrieves a list of users.
  
- **POST:** Send data to the server to create a new resource. It’s often used when submitting a form or uploading data.
  - Example: `POST /users` sends data to create a new user.
  
- **PUT:** Update an existing resource on the server. This method replaces the resource entirely.
  - Example: `PUT /users/{id}` updates the user with the specified ID.
  
- **DELETE:** Remove a resource from the server.
  - Example: `DELETE /users/{id}` deletes the user with the specified ID.

##### **1.3.2. HTTP Status Codes**

When you make a request to an API, the server responds with an HTTP status code that indicates the outcome of the request. Some of the most common status codes are:

- **200 OK:** The request was successful, and the server returned the requested data.
  
- **201 Created:** The resource was successfully created (often used with `POST`).
  
- **400 Bad Request:** The server could not process the request due to invalid input or syntax errors.
  
- **401 Unauthorized:** The request lacks valid authentication credentials.
  
- **404 Not Found:** The requested resource could not be found on the server.
  
- **500 Internal Server Error:** The server encountered an error that prevented it from fulfilling the request.

Each of these codes helps you understand the result of the API call and take appropriate action.

#### **1.4. Request and Response Structure**

HTTP requests and responses follow a specific structure:

##### **1.4.1. HTTP Request Structure**

When you make a request to an API, it consists of the following parts:

1. **Request Line:**
   - The method (e.g., `GET`, `POST`), the URL (e.g., `/users`), and the HTTP version.
   - Example: `GET /users HTTP/1.1`

2. **Headers:**
   - Provide additional information about the request. For example, you might send authentication tokens or specify the data type you're sending.
   - Example: `Content-Type: application/json`, `Authorization: Bearer {token}`

3. **Body (optional):**
   - For methods like `POST` or `PUT`, the body contains the data you want to send to the server.
   - Example: A `POST` request might contain a JSON payload like `{"name": "John", "email": "john@example.com"}`.

##### **1.4.2. HTTP Response Structure**

The server responds with the following:

1. **Status Line:**
   - Contains the HTTP version, status code, and a brief status message.
   - Example: `HTTP/1.1 200 OK`

2. **Headers:**
   - Provide additional metadata about the response.
   - Example: `Content-Type: application/json`

3. **Body (optional):**
   - The actual data returned by the server, often in JSON format.
   - Example: `{"name": "John", "email": "john@example.com"}`

#### **1.5. Tools for Exploring APIs**

To interact with APIs, there are several tools that make testing and debugging easier:

- **Postman:**
  - A popular tool for making API requests, sending data, and analyzing responses. It allows you to set headers, query parameters, and body content. It’s useful for manually testing APIs and inspecting responses.
  
- **cURL:**
  - A command-line tool that allows you to send HTTP requests directly from the terminal.
  - Example: `curl -X GET http://localhost:8080/users`
  
- **Browser Developer Tools:**
  - Most modern browsers come with built-in tools (e.g., Chrome DevTools) that allow you to inspect network requests and responses. You can see the headers, status codes, and bodies of API responses.

#### **1.6. Hands-On Exercise: Exploring the Go API**

Let’s now test the `GET` and `POST` methods of our Go API.

1. **GET /users**
   - Use **Postman** or **cURL** to send a `GET` request to the endpoint:
     ```bash
     GET http://localhost:8080/users
     ```
   - You should receive a list of users (or an empty array if none are created yet).

2. **POST /users**
   - Use **Postman** to send a `POST` request with a JSON body:
     ```json
     {
       "name": "John",
       "email": "john@example.com"
     }
     ```
   - The API should respond with a `201 Created` status and the new user’s data.

---

### **Summary of Chapter 1**

In this chapter, you learned the fundamental concepts of APIs and HTTP. Here are the key takeaways:

- **APIs** allow different applications to communicate over the internet using HTTP as the protocol.
- **RESTful APIs** use HTTP methods (GET, POST, PUT, DELETE) to interact with resources identified by URLs.
- **HTTP Status Codes** provide feedback on the result of an API request, such as `200 OK` or `404 Not Found`.
- **Requests and Responses** contain headers, methods, bodies, and status codes that define how data is exchanged.
- **Tools** like Postman and cURL are essential for exploring, testing, and debugging APIs.

In the next chapter, we’ll dive deeper into **JSON**, the most common format for API data.

----

[>> Next Chapter](chapter2.md)

