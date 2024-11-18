# **Chapter 5: Writing Automated API Tests**

#### **5.1. Introduction**

Automated API testing plays a crucial role in ensuring that your API behaves as expected and maintains its reliability as it evolves. Writing automated tests for your API can help detect regressions, validate new features, and ensure that edge cases are handled properly.

In this chapter, we will explore how to write automated API tests using popular frameworks in different programming languages:

- **Jest (TypeScript)**
- **Testing (Go)**
- **NUnit (C#)**

We will cover how to structure these tests, how to handle both happy and sad paths, and how to make assertions for various response attributes like status codes and payloads. By the end of this chapter, you’ll be able to write basic automated tests for your API endpoints, such as `GET /users` and `POST /users`.

---

#### **5.2. Introduction to API Testing Frameworks**

Testing frameworks allow us to automate and structure our tests for APIs. These frameworks typically provide utilities for making HTTP requests, checking the responses, and asserting various conditions.

Let's look at a few popular API testing frameworks in different languages:

---

##### **5.2.1. Jest (TypeScript)**

Jest is a widely used testing framework for JavaScript and TypeScript. It has built-in utilities for mocking functions, handling asynchronous code, and generating reports. To test APIs with Jest, we usually use the `supertest` library, which provides an easy way to make HTTP requests and assert conditions on the responses.

**Installation for Jest + Supertest:**

```bash
npm install --save-dev jest supertest
```

Once installed, you can write tests for your API endpoints by using `supertest` to send requests to the application.

---

##### **5.2.2. Testing (Go)**

The Go `testing` package is the built-in testing framework in Go. It allows you to create unit and integration tests for your Go-based API endpoints. For API testing, we typically use the `net/http/httptest` package, which provides utilities for testing HTTP requests and responses in memory.

**Example:**

```go
import (
	"testing"
	"net/http"
	"net/http/httptest"
)

func TestGetUsers(t *testing.T) {
	req, err := http.NewRequest("GET", "/users", nil)
	if err != nil {
		t.Fatal(err)
	}

	// Create a response recorder
	rr := httptest.NewRecorder()
	handler := http.HandlerFunc(GetUsers)

	// Call the handler with the request and recorder
	handler.ServeHTTP(rr, req)

	// Assert status code
	if status := rr.Code; status != http.StatusOK {
		t.Errorf("Expected status code %d, got %d", http.StatusOK, status)
	}
}
```

---

##### **5.2.3. NUnit (C#)**

NUnit is a popular testing framework for .NET languages like C#. To automate API testing with NUnit, you can use the `HttpClient` class to send requests and verify the responses. You can then use NUnit's assertion methods to check the status code, headers, and response body.

**Installation:**

You can install the NUnit NuGet package:

```bash
Install-Package NUnit
```

Then, you can write your API tests using NUnit's attributes and assertions.

---

#### **5.3. Writing Tests for Happy Path, Sad Path, and Edge Cases**

Now, let's break down how to write different types of tests:

---

##### **5.3.1. Happy Path Tests (Valid Inputs)**

Happy path tests ensure that the API works as expected when given valid input. These are the tests you would run when you know everything is correct, such as sending valid data to a `POST` endpoint and expecting a successful response.

For example, testing the `GET /users` endpoint might look like this:

**Jest Example:**

```typescript
import request from 'supertest';
import app from './app'; // your Express app

describe('GET /users', () => {
  it('should return a list of users with status 200', async () => {
    const response = await request(app).get('/users');
    expect(response.status).toBe(200);
    expect(Array.isArray(response.body)).toBe(true);
  });
});
```

In the above example, we're asserting that the response has a `200 OK` status code and that the body is an array (since we're expecting a list of users).

---

##### **5.3.2. Sad Path Tests (Invalid Inputs)**

Sad path tests verify how the API behaves when given invalid input or in error situations. These tests help ensure that the API handles failure scenarios gracefully.

For instance, testing a `POST /users` endpoint with missing required fields might look like this:

**Jest Example:**

```typescript
describe('POST /users', () => {
  it('should return 400 when required fields are missing', async () => {
    const response = await request(app).post('/users').send({ name: '' });
    expect(response.status).toBe(400);
    expect(response.body.error).toBe('Name is required');
  });
});
```

In this case, we assert that when a user sends a `POST` request with an empty name, the API responds with a `400 Bad Request` and a helpful error message.

---

##### **5.3.3. Edge Case Tests**

Edge cases test the boundaries of your API. These might include:

- Empty arrays or lists.
- Extremely large or small numbers.
- Unusual string formats.

For example, a test that checks whether the `GET /users` endpoint handles an empty database properly:

**Jest Example:**

```typescript
describe('GET /users', () => {
  it('should return an empty array if no users exist', async () => {
    const response = await request(app).get('/users');
    expect(response.status).toBe(200);
    expect(response.body).toEqual([]);
  });
});
```

---

#### **5.4. Assertions for Status Codes and Response Payloads**

One of the main things you will be asserting in API tests is the **status code**. Each HTTP response includes a status code that indicates the result of the request.

Common status codes:
- `200 OK`: The request was successful.
- `201 Created`: A resource was successfully created.
- `400 Bad Request`: The request was invalid or incomplete.
- `404 Not Found`: The requested resource could not be found.
- `500 Internal Server Error`: The server encountered an error.

In addition to status codes, you will often need to assert on the **response payload**. This includes checking the body of the response to ensure it matches the expected format and contains the right data.

For example, a test for `GET /users` might involve checking that the response body is an array and that each item has a `name` field.

**Jest Example:**

```typescript
describe('GET /users', () => {
  it('should return a list of users with a name field', async () => {
    const response = await request(app).get('/users');
    expect(response.status).toBe(200);
    expect(Array.isArray(response.body)).toBe(true);
    response.body.forEach(user => {
      expect(user).toHaveProperty('name');
    });
  });
});
```

---

#### **5.5. Hands-On: Automating Tests for `GET /users` and `POST /users`**

Now, let's automate some tests for two common API endpoints: `GET /users` and `POST /users`.

---

##### **5.5.1. Automating a Test for `GET /users`**

Let's write an automated test to verify that the `GET /users` endpoint returns the expected list of users.

**Jest Example:**

```typescript
describe('GET /users', () => {
  it('should return a list of users', async () => {
    const response = await request(app).get('/users');
    expect(response.status).toBe(200);
    expect(response.body).toEqual(expect.arrayContaining([{ name: expect.any(String) }]));
  });
});
```

This test will ensure that `GET /users` returns a list of users, each having a `name` property.

---

##### **5.5.2. Automating a Test for `POST /users`**

Next, let's automate a test to ensure that a `POST /users` request works as expected when sending valid data.

**Jest Example:**

```typescript
describe('POST /users', () => {
  it('should create a new user and return the user with status 201', async () => {
    const newUser = { name: 'John Doe' };
    const response = await request(app).post('/users').send(newUser);
    expect(response.status).toBe(201);
    expect(response.body).toHaveProperty('id');
    expect(response.body.name).toBe(newUser.name);
  });
});
```

This test sends a valid `POST` request with a user name and asserts that the response status is `201 Created`, the user is returned with an `id` field, and the name matches the input.

---

#### **5.6. Conclusion**

In this chapter, we explored how to write automated API tests using frameworks like Jest (TypeScript), Go's testing package, and NUnit (C#). We covered how to write tests for the happy path, sad path, and edge cases, and we learned how to make assertions on status codes and response payloads.

By automating your API tests, you can ensure that your API remains reliable as it evolves, and you can catch potential issues early in the development process.

In the next chapter, we’ll dive deeper into more complex scenarios and test strategies for APIs.

----

[>> Next Chapter](chapter6.md)

