# **Chapter 7: Debugging API Issues**

#### **7.1. Introduction**

Debugging is an essential skill for any API developer or tester. When things go wrong with your API, whether it's an unexpected error or a failure in your test, having the right strategies and tools in place can save you a lot of time and frustration.

In this chapter, we’ll go over common API issues, effective debugging techniques, and tools that can help you pinpoint the root cause of failures. By the end of this chapter, you'll be equipped with strategies to handle debugging in various scenarios, including handling incorrect payloads, missing headers, and backend bugs.

We will also work through debugging a failing `PUT /users/{id}` request as a hands-on exercise.

---

#### **7.2. Common API Issues**

Understanding the typical issues that can arise when working with APIs is the first step toward effective debugging. Here are some of the most common API issues:

---

##### **7.2.1. Incorrect Payloads**

Incorrect payloads are one of the most common reasons for API failures. The payload could be malformed, missing required fields, or using the wrong data types. For example, a `POST /users` request might fail if the `name` field is missing or if the age is provided as a string instead of a number.

**Example of an Incorrect Payload:**

```json
{
  "name": "John",
  "age": "twenty-five"  // Incorrect data type, should be a number
}
```

**How to debug:**
- Check if the payload structure matches the expected format.
- Ensure all required fields are present.
- Validate data types and field constraints.

---

##### **7.2.2. Missing Headers**

HTTP headers are often crucial for API requests, and missing headers can cause requests to fail. For example, an API might require an `Authorization` header for authentication or a `Content-Type` header to specify the request format (e.g., `application/json`).

**Example of Missing Headers:**

If an API expects an `Authorization` header but it’s not provided, the request will likely return a `401 Unauthorized` response.

**How to debug:**
- Check the API documentation to ensure the required headers are included.
- Make sure headers like `Authorization`, `Content-Type`, and `Accept` are set correctly.
- Use tools like Postman or browser developer tools to inspect request headers.

---

##### **7.2.3. Backend Bugs**

Sometimes, the issue lies within the backend code itself. This can happen due to incorrect logic, missing database records, or unexpected failures in the backend service.

**How to debug:**
- Check the server logs for error messages and stack traces.
- Review the backend code to ensure the correct logic is being executed.
- Verify that all dependent services (e.g., databases, third-party APIs) are functioning properly.

---

#### **7.3. Debugging Techniques**

Now that we've covered common issues, let’s look at debugging techniques that can help identify and fix problems in your API.

---

##### **7.3.1. Logging Requests and Responses**

Logging is a fundamental debugging technique that allows you to see the data being sent and received. By logging the incoming requests and outgoing responses, you can verify if the API is receiving the correct data and sending the expected results.

**How to implement:**
- In your Go application, you can use the built-in `log` package to log requests and responses.

Example in Go:

```go
import (
	"log"
	"net/http"
)

func logRequest(r *http.Request) {
	log.Printf("Received request: %s %s", r.Method, r.URL)
}

func logResponse(w http.ResponseWriter, statusCode int, responseBody string) {
	log.Printf("Response status: %d, body: %s", statusCode, responseBody)
}

func handler(w http.ResponseWriter, r *http.Request) {
	logRequest(r)

	// Simulate some backend logic
	statusCode := http.StatusOK
	responseBody := "{\"message\": \"Success\"}"

	// Log the response
	logResponse(w, statusCode, responseBody)

	w.WriteHeader(statusCode)
	w.Write([]byte(responseBody))
}
```

In the above example, we log the request method and URL, and also log the status code and response body before sending the response.

---

##### **7.3.2. Capturing Traffic with Tools like Postman Proxy or Fiddler**

When debugging HTTP requests and responses, capturing and inspecting the traffic between your client and the server can provide valuable insights.

Tools like **Postman Proxy** and **Fiddler** allow you to capture HTTP traffic and view the details of each request and response, including headers, payloads, and any errors.

- **Postman Proxy**: Postman provides an option to act as a proxy, allowing you to capture and inspect all HTTP requests between your client and the server. This can help debug issues with headers, request payloads, and responses.
- **Fiddler**: Fiddler is a web debugging proxy that captures HTTP(S) traffic between your computer and the internet. It provides a user-friendly interface to inspect, modify, and debug requests and responses.

Both tools allow you to:
- View request and response headers.
- Inspect the body of requests and responses.
- Modify requests on the fly and resend them to test different inputs.

---

##### **7.3.3. Inspecting Server Logs (Go Application)**

Server logs can provide valuable information when debugging backend issues. In Go, you can use the `log` package to output logs, but for more complex scenarios, you might want to use a logging framework like **logrus** or **zap** that provides structured logging and better log management.

**Go Example:**

```go
import (
	"log"
	"os"
)

func main() {
	// Create a log file
	file, err := os.OpenFile("app.log", os.O_CREATE|os.O_APPEND|os.O_RDWR, 0666)
	if err != nil {
		log.Fatal(err)
	}
	defer file.Close()

	// Set log output to file
	log.SetOutput(file)

	// Log an info message
	log.Println("Starting the application...")

	// Simulate an error
	err = someFunction()
	if err != nil {
		log.Printf("Error occurred: %v", err)
	}
}
```

Here, we create a log file and log both general information and error messages. Server logs can help you trace through the execution flow and identify where things went wrong.

---

#### **7.4. Tools for Advanced Debugging**

In addition to logging and traffic capture, there are other tools you can use to debug API issues:

---

##### **7.4.1. Postman Console**

The Postman Console is a powerful tool that helps you see detailed information about requests, including headers, body, and status codes. You can view the console logs in the Postman application to troubleshoot issues such as failed requests or incorrect parameters.

To access the Postman Console:
1. Open Postman.
2. Click on the "View" menu.
3. Select "Show Postman Console."

You can then make requests in Postman and inspect the console output for detailed information on each request and response.

---

##### **7.4.2. Browser Developer Tools (Network Tab)**

Modern browsers have built-in developer tools that include a **Network** tab for inspecting HTTP traffic. This is especially useful for debugging client-side issues or verifying how your frontend interacts with the backend API.

To use the Network tab:
1. Open the developer tools in your browser (press `F12` or right-click and select "Inspect").
2. Go to the **Network** tab.
3. Make the request from your application.
4. Inspect the details of the HTTP request and response.

The **Network** tab allows you to:
- View headers, request parameters, and responses.
- Inspect status codes, response times, and payloads.
- Identify failed requests and review error messages.

---

#### **7.5. Hands-On: Debug a Failing PUT /users/{id} Request**

Let’s put everything into practice by debugging a failing `PUT /users/{id}` request. Suppose the request is failing, and we need to find the root cause.

**Steps:**
1. **Check the Logs:** Look at the server logs to see if there are any error messages or stack traces that explain why the request failed.
2. **Inspect the Payload:** Use Postman or your browser’s developer tools to inspect the request payload and verify that all required fields are included and formatted correctly.
3. **Check the Headers:** Ensure the request includes all necessary headers, such as `Authorization` or `Content-Type`.
4. **Reproduce the Issue:** Try reproducing the issue by sending different types of data in the `PUT /users/{id}` request (e.g., valid, invalid, or missing fields) to see how the server responds.

---

#### **7.6. Conclusion**

Debugging is an essential part of API testing and development. By using the techniques and tools outlined in this chapter, you can quickly identify and resolve common issues like incorrect payloads, missing headers, and backend bugs. Logging, traffic capture, and inspecting server logs are your primary methods for understanding and fixing problems.

In the next chapter, we will dive deeper into advanced API testing strategies and best practices for building reliable APIs.

----

[>> Next Chapter](chapter8.md)