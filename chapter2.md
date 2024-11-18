# **Chapter 2: Understanding JSON**

#### **2.1. What is JSON?**

JSON (JavaScript Object Notation) is a lightweight data-interchange format that is easy for humans to read and write and easy for machines to parse and generate. JSON is language-independent but uses conventions that are familiar to most modern programming languages, including **JavaScript**, **Python**, **Go**, and **C#**.

JSON is commonly used for transmitting data in web APIs because it is easy to work with and efficiently encodes data structures.

#### **2.2. Structure of JSON**

A JSON object consists of key-value pairs. The key is always a **string**, and the value can be any of the following:

- **String:** A sequence of characters enclosed in double quotes (`" "`).
- **Number:** A numerical value, either an integer or floating point.
- **Boolean:** `true` or `false`.
- **Array:** An ordered list of values enclosed in square brackets (`[ ]`).
- **Object:** A collection of key-value pairs enclosed in curly braces (`{ }`).
- **Null:** Represents an empty value or no value (`null`).

Here’s an example of a simple JSON object:

```json
{
  "name": "John Doe",
  "age": 30,
  "isActive": true,
  "emails": ["john.doe@example.com", "john.doe123@example.com"],
  "address": {
    "street": "123 Main St",
    "city": "New York",
    "zip": "10001"
  },
  "children": null
}
```

### **2.3. JSON Data Types**

Let’s break down the types of data you can encounter in a JSON object:

#### **2.3.1. Strings**

A string is a sequence of characters, and it must be enclosed in double quotes.

Example:
```json
{
  "name": "John"
}
```

#### **2.3.2. Numbers**

Numbers in JSON can be integers or floating-point numbers. JSON does not differentiate between integer and floating-point types — all numbers are treated as **floating point**.

Example:
```json
{
  "age": 30,
  "height": 5.9
}
```

#### **2.3.3. Booleans**

Booleans are simple values that can either be `true` or `false`.

Example:
```json
{
  "isActive": true,
  "isVerified": false
}
```

#### **2.3.4. Arrays**

An array is a list of values enclosed in square brackets (`[]`). The values can be of any type, including other arrays or objects.

Example:
```json
{
  "emails": ["john@example.com", "john.doe@example.com"]
}
```

#### **2.3.5. Objects**

An object is a collection of key-value pairs, where the key is a string, and the value can be any valid JSON type, including other objects and arrays. Objects are enclosed in curly braces (`{}`).

Example:
```json
{
  "address": {
    "street": "123 Main St",
    "city": "New York",
    "zip": "10001"
  }
}
```

#### **2.3.6. Null**

`null` is a special value that represents an absence of data or an empty field.

Example:
```json
{
  "children": null
}
```

### **2.4. JSON Syntax Rules**

- **Key-Value Pairs:** Each key-value pair in an object is separated by a colon (`:`).
- **Comma Separation:** Multiple key-value pairs or array elements are separated by commas.
- **No Trailing Commas:** JSON does not allow trailing commas. For example, this is **incorrect**:
  ```json
  {
    "name": "John",
    "age": 30,   <-- this comma is not allowed here
  }
  ```
- **Nested Structures:** JSON supports nested objects and arrays. You can have an array of objects or an object containing arrays, enabling complex data structures.

Example:
```json
{
  "users": [
    {
      "name": "Alice",
      "age": 25
    },
    {
      "name": "Bob",
      "age": 30
    }
  ]
}
```

### **2.5. JSON in APIs**

Most modern web APIs use JSON to send and receive data. In APIs, JSON is typically used in **request bodies** (especially for POST and PUT methods) and in **response bodies** (to send data back to the client).

Let’s look at an example of how you might interact with a JSON-based API:

#### **2.5.1. Sending JSON in a POST Request**

Suppose you're creating a new user in a system. You might send a `POST` request with the following JSON body:

**Request:**
```bash
POST /users HTTP/1.1
Content-Type: application/json

{
  "name": "John Doe",
  "email": "john.doe@example.com",
  "age": 28
}
```

#### **2.5.2. Receiving JSON in a Response**

The server might respond with a JSON object containing the newly created user’s data:

**Response:**
```json
{
  "id": 1,
  "name": "John Doe",
  "email": "john.doe@example.com",
  "age": 28
}
```

The client (such as a browser or an API testing tool) can then process this JSON response to display the data to the user or take further action.

### **2.6. Handling JSON in Go**

In Go, the standard library provides a package called `encoding/json` that makes it easy to work with JSON data. You can encode (marshal) Go objects into JSON and decode (unmarshal) JSON data into Go objects.

#### **2.6.1. Example: Encoding and Decoding JSON in Go**

Here’s a simple Go example to demonstrate encoding and decoding JSON:

```go
package main

import (
	"encoding/json"
	"fmt"
	"log"
)

// Define a struct that matches the JSON structure
type User struct {
	ID    int    `json:"id"`
	Name  string `json:"name"`
	Email string `json:"email"`
	Age   int    `json:"age"`
}

func main() {
	// Creating a new user object
	user := User{
		ID:    1,
		Name:  "John Doe",
		Email: "john.doe@example.com",
		Age:   28,
	}

	// Encoding the Go object to JSON
	userJSON, err := json.Marshal(user)
	if err != nil {
		log.Fatal(err)
	}
	fmt.Println("Encoded JSON:", string(userJSON))

	// Decoding the JSON back into a Go object
	var decodedUser User
	err = json.Unmarshal(userJSON, &decodedUser)
	if err != nil {
		log.Fatal(err)
	}
	fmt.Printf("Decoded Go Object: %+v\n", decodedUser)
}
```

Output:
```text
Encoded JSON: {"id":1,"name":"John Doe","email":"john.doe@example.com","age":28}
Decoded Go Object: {ID:1 Name:John Doe Email:john.doe@example.com Age:28}
```

### **2.7. Working with JSON in APIs**

When you interact with APIs, the data you send (usually in `POST` or `PUT` requests) and receive (from `GET` or `POST` responses) is usually in JSON format. You'll need to use JSON parsing and serialization tools in your chosen programming language.

#### **2.7.1. Sending JSON in a Request**

For example, when sending data to a Go API, you would typically send a `POST` request with a JSON body. Here’s an example using **Postman** to send JSON to an endpoint:

1. Set the request type to **POST**.
2. Enter the URL of the endpoint (e.g., `http://localhost:8080/users`).
3. In the body tab, choose **raw** and set the format to **JSON**.
4. Provide the JSON data, such as:
   ```json
   {
     "name": "John Doe",
     "email": "john.doe@example.com",
     "age": 28
   }
   ```

#### **2.7.2. Parsing JSON Response**

When the server responds, you’ll receive JSON, which you can then parse and handle appropriately. In **Go**, this can be done using the `json.Unmarshal()` method, as shown earlier.

### **2.8. Hands-On Exercise: Working with JSON in APIs**

Now, let’s practice:

1. **POST** a JSON request to create a new user.
2. **GET** the list of users and see how the response is structured as JSON.
3. Parse the JSON response and display specific fields like `name`, `email`, and `age` in your code.

---

### **Summary of Chapter 2**

In this chapter, you learned the basics of **JSON** (JavaScript Object Notation), a lightweight and widely used format for data exchange in APIs. The key takeaways include:

- JSON consists of key-value pairs, arrays, and nested objects.
- JSON can represent different data types such as strings, numbers, booleans, null, and arrays.
- JSON is commonly used to send and receive data in web APIs.
- You learned how to encode and decode JSON data in Go.

In the next chapter, we will dive into the practical aspects of **manually testing APIs** using tools like Postman and **cURL**.
----

[>> Next Chapter](chapter3.md)