# **Chapter 8: Advanced Topics in API Testing**

#### **8.1. Introduction**

As you gain more experience in API testing, you will encounter more complex scenarios that require advanced techniques and tools. These scenarios may involve APIs with authentication, data-driven testing with parameterized tests, mocking services for isolated tests, performance testing, and integrating automated tests into CI/CD pipelines. In this chapter, we’ll explore these advanced topics, providing both theoretical background and hands-on examples to expand your knowledge and skills.

By the end of this chapter, you will be well-equipped to handle more intricate testing challenges, ensuring your APIs are reliable, performant, and secure.

---

#### **8.2. Testing APIs with Authentication**

Many modern APIs require authentication to ensure that only authorized users can access the endpoints. Common authentication methods include **token-based authentication** (e.g., JWT), **OAuth 2.0**, and **API keys**. Testing APIs that require authentication adds an extra layer of complexity, but understanding how to handle these scenarios is crucial.

---

##### **8.2.1. Token-Based Authentication**

Token-based authentication is often used in modern web APIs. The client authenticates with the server and receives a token (e.g., JWT) that is then sent with each subsequent request to prove the client’s identity.

**Steps for testing an authenticated API using tokens:**
1. **Obtain a Token:** First, you need to authenticate with the API using a username and password, which should return a token.
2. **Send the Token:** For subsequent requests, include the token in the `Authorization` header.
3. **Verify Token Expiry and Permissions:** Make sure the token is valid and hasn’t expired, and verify the required permissions.

**Example of Authorization Header:**

```http
Authorization: Bearer <token>
```

---

##### **8.2.2. OAuth 2.0 Authentication**

OAuth 2.0 is a more complex, token-based authorization framework commonly used by APIs, especially when they need to interact with third-party services. OAuth allows users to grant access to their data without sharing their credentials.

**Steps for testing OAuth 2.0:**
1. **Authenticate Using OAuth:** Obtain the access token by sending a request with the correct client credentials and authorization code.
2. **Use the Token:** Use the access token in the `Authorization` header to authenticate subsequent requests.
3. **Handle Refresh Tokens:** OAuth typically provides a refresh token that can be used to obtain a new access token when the current one expires.

---

#### **8.3. Parameterized Tests for Data-Driven Testing**

Parameterized testing is an approach where the same test case is executed with multiple sets of data. This is useful when you want to validate an API’s behavior under different conditions, such as testing multiple users or different payloads.

---

##### **8.3.1. Writing Parameterized Tests**

In many testing frameworks, you can define parameterized tests to execute a test with a set of inputs. This allows you to avoid duplicating similar tests and increases coverage.

**Example in TypeScript (using Jest):**

```typescript
describe('GET /users', () => {
  const testCases = [
    { id: 1, expectedName: 'Alice' },
    { id: 2, expectedName: 'Bob' },
    { id: 3, expectedName: 'Charlie' }
  ];

  testCases.forEach(({ id, expectedName }) => {
    it(`should return user ${id} with name ${expectedName}`, async () => {
      const response = await request(app).get(`/users/${id}`);
      expect(response.status).toBe(200);
      expect(response.body.name).toBe(expectedName);
    });
  });
});
```

In this example, we test the `/users/{id}` endpoint with different user IDs and verify that the name matches the expected value.

---

##### **8.3.2. Benefits of Parameterized Tests**

- **Increased Test Coverage:** You can test multiple cases without writing redundant test functions.
- **Efficiency:** Reduces boilerplate code, making your tests cleaner and more maintainable.
- **Scalability:** Easily extend tests by adding more data to the parameterized set.

---

#### **8.4. Mocking APIs for Isolated Tests**

When testing APIs, it’s often necessary to mock external services to isolate the behavior of your API and test it without relying on other systems. Mocking allows you to simulate various scenarios, such as failures or slow responses, without actually calling the external service.

---

##### **8.4.1. Why Mock APIs?**

- **Isolate Tests:** Mocking ensures that your tests only focus on the API you are testing, rather than on third-party systems.
- **Simulate Edge Cases:** You can mock specific responses to simulate edge cases or failure conditions (e.g., timeouts, server errors).
- **Improve Test Stability:** If an external service is unavailable or flaky, mocking prevents your tests from failing because of that.

---

##### **8.4.2. Mocking with Libraries**

Many testing frameworks have libraries or plugins for mocking HTTP requests. For example:
- **Go:** You can use the `httpmock` package to mock HTTP requests in Go.
- **Jest (TypeScript):** The `jest-fetch-mock` or `nock` libraries allow you to mock network requests in your tests.
- **C#:** The `Moq` library is often used to mock dependencies in C#.

---

##### **Example of Mocking the `/users/{id}` Endpoint:**

In a Go test, we can use the `httpmock` package to mock the `/users/{id}` endpoint:

```go
package main

import (
	"fmt"
	"testing"
	"github.com/jarcoal/httpmock"
	"net/http"
	"net/http/httptest"
)

func TestGetUser(t *testing.T) {
	// Mock the HTTP request
	httpmock.Activate()
	defer httpmock.DeactivateAndReset()

	// Define the mock response
	httpmock.RegisterResponder("GET", "https://api.example.com/users/1", httpmock.NewStringResponder(200, `{"id": 1, "name": "Alice"}`))

	// Send a request to the mock server
	resp, err := http.Get("https://api.example.com/users/1")
	if err != nil {
		t.Fatalf("expected no error, got %v", err)
	}

	// Check the response status
	if resp.StatusCode != http.StatusOK {
		t.Fatalf("expected status 200, got %d", resp.StatusCode)
	}

	// Read the response body
	var responseBody map[string]interface{}
	if err := json.NewDecoder(resp.Body).Decode(&responseBody); err != nil {
		t.Fatalf("expected no error decoding response body, got %v", err)
	}

	// Verify the mocked response data
	if responseBody["name"] != "Alice" {
		t.Fatalf("expected user name to be 'Alice', got %v", responseBody["name"])
	}
}
```

This example demonstrates how to mock an HTTP GET request to the `/users/{id}` endpoint and check that the response is as expected.

---

#### **8.5. Testing Performance with Tools like JMeter or k6**

Performance testing ensures that your API can handle the expected load and perform well under stress. Tools like **JMeter** and **k6** are commonly used for load testing and performance benchmarking.

---

##### **8.5.1. JMeter for Performance Testing**

Apache JMeter is a popular open-source tool for performance testing. It allows you to simulate multiple users and measure how your API performs under load. JMeter can also be used for functional testing, but it shines when it comes to load testing APIs.

**How to use JMeter:**
1. Define a test plan with requests to your API.
2. Configure the number of users (threads) and the duration of the test.
3. Analyze the results in real-time using graphs and reports.

---

##### **8.5.2. k6 for Performance Testing**

[k6](https://k6.io/) is a modern open-source tool for load testing APIs. It allows you to write tests in JavaScript, making it easy to integrate with other testing frameworks.

**Basic Example of k6 Script:**

```javascript
import http from 'k6/http';
import { check } from 'k6';

export default function () {
  const res = http.get('https://api.example.com/users');
  
  check(res, {
    'status is 200': (r) => r.status === 200,
    'response time is below 200ms': (r) => r.timings.duration < 200,
  });
}
```

---

#### **8.6. CI/CD Pipeline Integration for Automated Tests**

Integrating API tests into a **CI/CD pipeline** ensures that your tests run automatically whenever changes are made to your codebase, providing early feedback about potential issues.

---

##### **8.6.1. Setting Up a CI/CD Pipeline for API Testing**

Most CI/CD platforms like **Jenkins**, **GitLab CI**, **CircleCI**, and **GitHub Actions** support automated testing. You can configure your pipeline to run your API tests whenever code is pushed to a repository.

**Example of a GitHub Actions Workflow for API Testing:**

```yaml
name: API Testing

on: [push]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout Code
      uses: actions/checkout@v2

    - name: Set up Go
      uses: actions/setup-go@v2
      with:
        go-version: '1.18'

    - name: Run Tests
      run:

 |
        go test -v ./...
```

This configuration runs the tests whenever there is a push to the repository.

---

#### **8.7. Hands-On: Write a Test Suite for `/users` Endpoints**

Let’s put our knowledge into practice by creating a test suite for a `/users` API endpoint with parameterized test cases.

---

##### **8.7.1. Test Suite for `/users` Endpoint**

Here, we'll write a test suite in TypeScript with Jest to test the `/users` API.

```typescript
describe('GET /users', () => {
  const testCases = [
    { userId: 1, name: 'Alice', status: 200 },
    { userId: 2, name: 'Bob', status: 200 },
    { userId: 3, name: 'Charlie', status: 200 }
  ];

  testCases.forEach(({ userId, name, status }) => {
    it(`should return user ${userId} with name ${name}`, async () => {
      const response = await request(app).get(`/users/${userId}`);
      expect(response.status).toBe(status);
      expect(response.body.name).toBe(name);
    });
  });
});
```

---

##### **8.7.2. Mock the `/users/{id}` Endpoint**

To simulate failures, we’ll mock the `/users/{id}` endpoint to return an error. This simulates a failure scenario such as a 404 error when the user is not found.

```typescript
import nock from 'nock';

describe('GET /users/{id} with failure', () => {
  it('should return a 404 when user not found', async () => {
    nock('https://api.example.com')
      .get('/users/999')
      .reply(404, { message: 'User not found' });

    const response = await request(app).get('/users/999');
    expect(response.status).toBe(404);
    expect(response.body.message).toBe('User not found');
  });
});
```

---

#### **8.8. Conclusion**

In this chapter, we’ve covered several advanced topics in API testing:
- Testing APIs with various authentication methods (e.g., token-based, OAuth).
- Writing parameterized tests for data-driven scenarios.
- Mocking APIs for isolated testing.
- Using performance testing tools like JMeter and k6.
- Integrating automated API tests into CI/CD pipelines.

With these advanced techniques, you’re now better equipped to tackle complex testing challenges, ensuring that your APIs are reliable, secure, and performant under different conditions.

----

[>> Next Chapter](chapter9.md)