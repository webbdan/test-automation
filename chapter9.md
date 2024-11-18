# **Chapter 9: Capstone Project**

#### **9.1. Introduction**

In this chapter, you will apply everything you have learned in API testing to create a robust, fully automated testing framework for a Go application. This hands-on project will guide you through setting up a test suite that covers all of the key concepts and techniques discussed throughout the course: comprehensive test scripts, reporting, debugging, parameterized test cases for edge scenarios, mocking, and performance testing.

By the end of this chapter, you will have a complete API test suite that can be used to ensure the robustness and reliability of your Go application’s API endpoints. This project will also help you understand how to structure and deliver a test suite that can be shared with others, whether it's through GitHub or another version control platform.

---

#### **9.2. Objective: Automate Comprehensive Tests for the Go Application**

Your task is to automate the testing of all API endpoints within your Go application. These tests should cover a variety of scenarios, from basic functionality to edge cases and performance under load. 

The test suite will include:
- **Test scripts for all API endpoints**: Ensure that every endpoint is thoroughly tested.
- **Reporting and debugging setup**: Set up reporting mechanisms for easy tracking of test results and debugging.
- **Parameterized test cases**: Use data-driven testing for different input scenarios.
- **Mocking and performance tests**: Mock external dependencies and test the API under stress.

---

#### **9.3. Setting Up the Test Suite for the Go Application**

##### **9.3.1. Tools and Libraries**

To create a robust API test suite for a Go application, we will be using the following tools:
- **Testing framework**: Go’s built-in testing package (`testing`).
- **HTTP mocking**: Use the `httpmock` package to mock external API calls during testing.
- **HTTP client**: Use `net/http` for making requests in tests.
- **Assertion library**: Use the built-in `assert` package or third-party libraries like `stretchr/testify` for more readable assertions.
- **Performance testing tools**: We will use `k6` or `JMeter` for load and performance testing.
- **Continuous Integration (CI)**: GitHub Actions, Jenkins, or similar tools to run tests automatically.

---

##### **9.3.2. Test Script Structure**

The test suite will be organized in a clear and scalable way. Here’s a general structure for organizing the test scripts:

```
/tests
    /api
        /users
            user_test.go
        /auth
            auth_test.go
        /products
            product_test.go
    /mock
        mock_external_api.go
    /performance
        performance_test.js
    /utils
        utils_test.go
    /report
        test_report.go
```

- **`/api`**: Contains tests for all the core API endpoints (`users`, `auth`, `products`, etc.).
- **`/mock`**: Contains mock configurations for external APIs and services.
- **`/performance`**: Holds scripts for performance testing (e.g., `k6` or `JMeter` scripts).
- **`/utils`**: Contains utility functions and test helpers.
- **`/report`**: Handles the reporting logic for test results.

---

#### **9.4. Writing Test Scripts for API Endpoints**

##### **9.4.1. Basic Test Case for an Endpoint**

Let's start with a basic test case for the `/users/{id}` endpoint. This will be a simple GET request that checks if a user exists and returns the correct status and data.

```go
package api

import (
	"testing"
	"net/http"
	"github.com/stretchr/testify/assert"
)

func TestGetUser(t *testing.T) {
	// Make the GET request to the /users/{id} endpoint
	resp, err := http.Get("https://api.example.com/users/1")
	if err != nil {
		t.Fatalf("Error while making GET request: %v", err)
	}
	defer resp.Body.Close()

	// Assert the status code
	assert.Equal(t, 200, resp.StatusCode)

	// Assert the response body (using a basic struct check here, but could be expanded)
	var user struct {
		ID   int    `json:"id"`
		Name string `json:"name"`
	}
	// Parse the response body into the user struct
	// Use the JSON package or any other preferred parsing method
	assert.NoError(t, json.NewDecoder(resp.Body).Decode(&user))
	assert.Equal(t, "Alice", user.Name)
}
```

This basic test ensures that the `/users/{id}` endpoint works as expected. The `assert` statements are used to check the status code and the response body.

---

##### **9.4.2. Parameterized Test Cases for Edge Scenarios**

You can extend this test to cover different edge cases using parameterized tests. For instance, testing how the system behaves when different IDs are passed.

```go
func TestGetUserWithEdgeCases(t *testing.T) {
	testCases := []struct {
		userId       int
		expectedName string
		expectedStatus int
	}{
		{1, "Alice", 200},
		{999, "", 404}, // Simulating user not found
		{-1, "", 400},  // Simulating invalid input
	}

	for _, tc := range testCases {
		t.Run(fmt.Sprintf("User %d", tc.userId), func(t *testing.T) {
			resp, err := http.Get(fmt.Sprintf("https://api.example.com/users/%d", tc.userId))
			if err != nil {
				t.Fatalf("Error while making GET request: %v", err)
			}
			defer resp.Body.Close()

			assert.Equal(t, tc.expectedStatus, resp.StatusCode)
			if resp.StatusCode == 200 {
				var user struct {
					ID   int    `json:"id"`
					Name string `json:"name"`
				}
				assert.NoError(t, json.NewDecoder(resp.Body).Decode(&user))
				assert.Equal(t, tc.expectedName, user.Name)
			}
		})
	}
}
```

This example tests the `/users/{id}` endpoint with valid and invalid user IDs, ensuring that the system responds correctly to both valid and error conditions.

---

#### **9.5. Mocking External APIs**

In real-world scenarios, your API might rely on third-party services, and it’s important to mock these services to isolate your tests.

Here’s an example of mocking an external API request using `httpmock`:

```go
package mock

import (
	"testing"
	"github.com/jarcoal/httpmock"
	"net/http"
	"github.com/stretchr/testify/assert"
)

func TestMockExternalAPI(t *testing.T) {
	// Activate HTTP mock
	httpmock.Activate()
	defer httpmock.DeactivateAndReset()

	// Define the mock response
	httpmock.RegisterResponder("GET", "https://externalapi.com/data",
		httpmock.NewStringResponder(200, `{"status": "success"}`))

	// Send a request to the mocked endpoint
	resp, err := http.Get("https://externalapi.com/data")
	if err != nil {
		t.Fatalf("Expected no error, got %v", err)
	}
	defer resp.Body.Close()

	// Assert the mock response
	assert.Equal(t, 200, resp.StatusCode)
}
```

This test simulates an external API call to `https://externalapi.com/data` and verifies that the correct mocked response is returned.

---

#### **9.6. Performance Testing with k6**

Performance testing ensures that your API can handle the expected load. We'll use `k6` for this.

**Basic k6 Script:**

```javascript
import http from 'k6/http';
import { check } from 'k6';

export default function () {
  const res = http.get('https://api.example.com/users');

  check(res, {
    'status is 200': (r) => r.status === 200,
    'response time is below 500ms': (r) => r.timings.duration < 500,
  });
}
```

This script sends a GET request to `/users` and checks whether the status is `200` and the response time is below 500ms.

**Running the k6 Test:**

```bash
k6 run script.js
```

You can further enhance this script by simulating more traffic and different kinds of loads (e.g., ramping up users or stress testing).

---

#### **9.7. Reporting and Debugging**

To ensure that test results are easily understandable, we need to configure reporting. Here's an example of how you can generate a simple test report:

```go
package report

import (
	"fmt"
	"testing"
	"github.com/stretchr/testify/assert"
)

func TestGenerateReport(t *testing.T) {
	result := "Test passed"
	report := fmt.Sprintf("Test Report: %s", result)
	t.Log(report) // Log the report
	assert.Equal(t, "Test passed", result)
}
```

You can integrate this into your CI pipeline (e.g., using GitHub Actions or Jenkins) to collect and display test results. Additionally, tools like `Allure` or `TestRail` can be integrated for richer reporting and test case management.

---

#### **9.8. Deliverable: A Fully Functional API Test Suite**

Once you have written your test cases, mocked the necessary external services, and implemented performance testing, you should push your work to a GitHub repository (or any other version control platform). This repository will contain:

- The complete test suite for all API endpoints.
- Mock configurations for external dependencies.
- Performance testing scripts.
- Reporting and debugging configurations.

---

#### **9.9. Conclusion**

In this chapter

, you've built a comprehensive API test suite for a Go application. You've applied a variety of techniques, including:

- Writing test scripts for all endpoints.
- Using parameterized test cases to cover edge scenarios.
- Mocking external services to isolate tests.
- Implementing performance tests to evaluate your API’s robustness.
- Setting up reporting and debugging for test results.

This capstone project has provided you with the hands-on experience necessary to create a fully functional API testing framework. You’re now ready to tackle any real-world testing challenges in your API development process.