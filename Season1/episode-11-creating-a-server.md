## Episode-11 | Creating a Server

### **Lecture Title:**
Creating a Server - Building Your First HTTP Server from Scratch

---

### **🔥 Core Concept:**
Ab asli game shuru hoti hai! 🎮 This is where Node.js truly shines - **creating servers**! Akshay bhaiya shows you how to build an HTTP server from absolute scratch using Node.js's built-in `http` module. No Express.js, no frameworks - pure Node.js!

You'll understand what a **server** actually is, how it listens for requests, how to handle different **routes**, send **responses**, work with **HTTP methods**, set **headers** and **status codes**. This is the foundation upon which all web frameworks (Express, Koa, NestJS) are built. Once you master this, frameworks will make complete sense!

**Why it's important:**
- **Core skill for backend development** - every backend developer must know this
- Understanding raw HTTP servers helps you master Express.js and other frameworks
- Common interview question: "Create a simple HTTP server in Node.js"
- Explains how APIs actually work at the fundamental level
- Helps you build custom servers when frameworks are too heavy
- Foundation for understanding REST APIs, microservices, and web architecture

---

### **💡 Key Definitions:**

- **Server**: A program that listens for incoming network requests and sends back responses.

- **HTTP (HyperText Transfer Protocol)**: The protocol browsers and servers use to communicate on the web.

- **HTTP Server**: A server that handles HTTP requests (GET, POST, PUT, DELETE, etc.) and sends HTTP responses.

- **Port**: A virtual endpoint for network communications - like an apartment number for network traffic (e.g., port 3000, 8080).

- **Request (req)**: Incoming data from client containing URL, method, headers, body, etc.

- **Response (res)**: Outgoing data from server containing status code, headers, body/content.

- **Route/Endpoint**: A specific URL path that the server handles (e.g., `/home`, `/api/users`).

- **HTTP Methods**: Verbs that indicate the type of operation (GET, POST, PUT, DELETE, PATCH, etc.).

- **Status Code**: Numeric code indicating the result of the request (200 OK, 404 Not Found, 500 Error, etc.).

- **Headers**: Metadata sent with request/response (Content-Type, Authorization, etc.).

- **Request Body**: Data sent by client (in POST/PUT requests) - usually JSON, form data, or files.

- **Response Body**: Data sent by server - HTML, JSON, files, etc.

- **Localhost**: Your own computer (IP: 127.0.0.1) - used for local development.

---

### **⚙️ Step-by-Step Explanation:**

**Understanding HTTP Server Fundamentals:**

1. **What is a Server?**
   ```
   Server = Program that:
   1. Listens on a specific port (e.g., 3000)
   2. Waits for incoming requests
   3. Processes the request
   4. Sends back a response
   5. Repeats forever (until stopped)

   Like a restaurant:
   - Server (waiter) waits for orders (requests)
   - Kitchen processes the order
   - Waiter brings food (response)
   - Ready for next customer
   ```

2. **Client-Server Model**
   ```
   Client (Browser/App)          Server (Node.js)
   ─────────────────────────────────────────────

   1. Send Request →            Listen on Port 3000
      GET /api/users

                                2. Receive Request
                                3. Process (check route)
                                4. Prepare Response

   5. Receive Response ←        Send Response
      200 OK                     { users: [...] }
      [User Data]
   ```

3. **HTTP Request Structure**
   ```
   GET /api/users HTTP/1.1
   Host: localhost:3000
   User-Agent: Mozilla/5.0
   Accept: application/json
   Authorization: Bearer token123

   [Body - empty for GET requests]

   Components:
   - Method: GET
   - Path: /api/users
   - HTTP Version: 1.1
   - Headers: Host, User-Agent, Accept, etc.
   - Body: Data (for POST/PUT)
   ```

4. **HTTP Response Structure**
   ```
   HTTP/1.1 200 OK
   Content-Type: application/json
   Content-Length: 45

   {"users": [{"id": 1, "name": "Akshay"}]}

   Components:
   - Status Code: 200 (OK)
   - Headers: Content-Type, Content-Length
   - Body: JSON data
   ```

5. **Common HTTP Status Codes**
   ```
   2xx - Success
   ├─ 200 OK - Request succeeded
   ├─ 201 Created - Resource created
   └─ 204 No Content - Success, no response body

   3xx - Redirection
   ├─ 301 Moved Permanently
   └─ 302 Found (Temporary redirect)

   4xx - Client Errors
   ├─ 400 Bad Request - Invalid request
   ├─ 401 Unauthorized - Authentication required
   ├─ 403 Forbidden - No permission
   ├─ 404 Not Found - Resource doesn't exist
   └─ 429 Too Many Requests - Rate limit exceeded

   5xx - Server Errors
   ├─ 500 Internal Server Error - Server crashed
   ├─ 502 Bad Gateway - Upstream server error
   └─ 503 Service Unavailable - Server overloaded
   ```

6. **HTTP Methods (CRUD Operations)**
   ```
   GET     - Read/Retrieve data (don't modify)
   POST    - Create new resource
   PUT     - Update entire resource
   PATCH   - Update partial resource
   DELETE  - Remove resource

   Example API:
   GET    /users      - Get all users
   GET    /users/1    - Get user with ID 1
   POST   /users      - Create new user
   PUT    /users/1    - Update user 1 (full)
   PATCH  /users/1    - Update user 1 (partial)
   DELETE /users/1    - Delete user 1
   ```

---

### **💻 Code Example & Detailed Explanation:**

**Example 1: Most Basic HTTP Server**

```javascript
// basic-server.js

// Import built-in http module
const http = require('http');

// Create server
const server = http.createServer((req, res) => {
    // This callback runs for EVERY request
    console.log("Request received!");

    // Send response
    res.end("Hello from Node.js Server! 🚀");
});

// Start listening on port 3000
const PORT = 3000;
server.listen(PORT, () => {
    console.log(`Server is running on http://localhost:${PORT}`);
});
```

**Run the server:**
```bash
$ node basic-server.js
Server is running on http://localhost:3000
```

**Test in browser:**
- Open: `http://localhost:3000`
- You'll see: "Hello from Node.js Server! 🚀"

**📝 Detailed Explanation:**

1. **`const http = require('http')`**
   - Import Node.js built-in HTTP module
   - Provides functionality to create HTTP servers
   - No installation needed (comes with Node.js)

2. **`http.createServer(callback)`**
   - Creates an HTTP server instance
   - Callback function executes for **every incoming request**
   - Callback receives two arguments:
     - `req` (request) - incoming data from client
     - `res` (response) - object to send data back

3. **`res.end("Hello...")`**
   - Sends response to client
   - Ends the response (must call this!)
   - Parameter is the response body (text/HTML/JSON)

4. **`server.listen(PORT, callback)`**
   - Starts the server
   - Listens on specified port (3000)
   - Callback runs once when server starts
   - Server runs forever (until you stop it with Ctrl+C)

5. **Port 3000**
   - Common port for development
   - Production: Usually 80 (HTTP) or 443 (HTTPS)
   - Ports < 1024 require admin/root privileges

**What happens when you visit `localhost:3000`:**
```
1. Browser sends HTTP GET request to localhost:3000
2. Node.js server receives the request
3. Callback executes: (req, res) => {...}
4. Server logs "Request received!"
5. Server sends response: "Hello from Node.js Server! 🚀"
6. Browser displays the text
7. Server ready for next request
```

---

**Example 2: Inspecting Request Object**

```javascript
// inspect-request.js

const http = require('http');

const server = http.createServer((req, res) => {
    console.log("=== New Request ===");
    console.log("Method:", req.method);           // GET, POST, etc.
    console.log("URL:", req.url);                 // /about, /api/users
    console.log("Headers:", req.headers);         // Object with headers
    console.log("HTTP Version:", req.httpVersion); // 1.1

    // Send response
    res.end("Request details logged in terminal!");
});

server.listen(3000, () => {
    console.log("Server running on http://localhost:3000");
    console.log("Try visiting different URLs:");
    console.log("  - http://localhost:3000");
    console.log("  - http://localhost:3000/about");
    console.log("  - http://localhost:3000/api/users");
});
```

**Output in terminal when visiting `http://localhost:3000/about`:**
```
Server running on http://localhost:3000
Try visiting different URLs:
  - http://localhost:3000
  - http://localhost:3000/about
  - http://localhost:3000/api/users

=== New Request ===
Method: GET
URL: /about
Headers: {
  host: 'localhost:3000',
  connection: 'keep-alive',
  'user-agent': 'Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7)...',
  accept: 'text/html,application/xhtml+xml...',
  'accept-encoding': 'gzip, deflate, br',
  'accept-language': 'en-US,en;q=0.9'
}
HTTP Version: 1.1
```

**📝 Key Request Properties:**

- **`req.method`**: HTTP method (GET, POST, PUT, DELETE, etc.)
- **`req.url`**: The path requested (/, /about, /api/users, etc.)
- **`req.headers`**: Object containing all headers
  - `req.headers.host` - "localhost:3000"
  - `req.headers['user-agent']` - Browser info
  - `req.headers['content-type']` - Data format
- **`req.httpVersion`**: HTTP protocol version ("1.1" or "2.0")

---

**Example 3: Basic Routing (Handling Different URLs)**

```javascript
// basic-routing.js

const http = require('http');

const server = http.createServer((req, res) => {
    const url = req.url;
    const method = req.method;

    console.log(`${method} ${url}`);

    // Route: Homepage
    if (url === '/') {
        res.end("🏠 Welcome to Homepage!");
    }
    // Route: About page
    else if (url === '/about') {
        res.end("ℹ️ About Us - We build awesome Node.js apps!");
    }
    // Route: Contact page
    else if (url === '/contact') {
        res.end("📧 Contact: akshay@namastenode.com");
    }
    // Route: API endpoint (JSON response)
    else if (url === '/api/users') {
        const users = [
            { id: 1, name: "Akshay" },
            { id: 2, name: "Saini" }
        ];

        // Set header for JSON
        res.setHeader('Content-Type', 'application/json');
        res.end(JSON.stringify(users));
    }
    // 404: Not Found
    else {
        res.statusCode = 404;
        res.end("❌ 404 - Page Not Found!");
    }
});

server.listen(3000, () => {
    console.log("Server running on http://localhost:3000");
    console.log("\nAvailable routes:");
    console.log("  GET  /           - Homepage");
    console.log("  GET  /about      - About page");
    console.log("  GET  /contact    - Contact info");
    console.log("  GET  /api/users  - Users API (JSON)");
});
```

**Testing routes:**
```bash
# In browser or using curl:
curl http://localhost:3000/
# Output: 🏠 Welcome to Homepage!

curl http://localhost:3000/about
# Output: ℹ️ About Us - We build awesome Node.js apps!

curl http://localhost:3000/api/users
# Output: [{"id":1,"name":"Akshay"},{"id":2,"name":"Saini"}]

curl http://localhost:3000/random
# Output: ❌ 404 - Page Not Found!
```

**📝 Explanation:**

1. **Routing Logic**: Simple if-else chain checking `req.url`
2. **Different responses** for different routes
3. **JSON API**:
   - Set `Content-Type` header to `application/json`
   - Convert JavaScript object to JSON string with `JSON.stringify()`
4. **404 Handling**: Default case for unknown routes
   - Set `res.statusCode = 404`
   - Send error message

---

**Example 4: Handling Different HTTP Methods**

```javascript
// http-methods.js

const http = require('http');

// In-memory data store (will be lost when server restarts)
let users = [
    { id: 1, name: "Akshay", email: "akshay@test.com" },
    { id: 2, name: "Saini", email: "saini@test.com" }
];

let nextId = 3;

const server = http.createServer((req, res) => {
    const { method, url } = req;

    console.log(`${method} ${url}`);

    // Set CORS headers (allow requests from browser)
    res.setHeader('Access-Control-Allow-Origin', '*');
    res.setHeader('Access-Control-Allow-Methods', 'GET, POST, PUT, DELETE');
    res.setHeader('Access-Control-Allow-Headers', 'Content-Type');

    // Handle preflight request
    if (method === 'OPTIONS') {
        res.statusCode = 204;
        res.end();
        return;
    }

    // GET /api/users - Get all users
    if (method === 'GET' && url === '/api/users') {
        res.setHeader('Content-Type', 'application/json');
        res.statusCode = 200;
        res.end(JSON.stringify(users));
    }

    // GET /api/users/:id - Get single user
    else if (method === 'GET' && url.startsWith('/api/users/')) {
        const id = parseInt(url.split('/')[3]);
        const user = users.find(u => u.id === id);

        if (user) {
            res.setHeader('Content-Type', 'application/json');
            res.statusCode = 200;
            res.end(JSON.stringify(user));
        } else {
            res.statusCode = 404;
            res.end(JSON.stringify({ error: "User not found" }));
        }
    }

    // POST /api/users - Create new user
    else if (method === 'POST' && url === '/api/users') {
        let body = '';

        // Collect data chunks
        req.on('data', chunk => {
            body += chunk.toString();
        });

        // When all data received
        req.on('end', () => {
            try {
                const newUser = JSON.parse(body);
                newUser.id = nextId++;
                users.push(newUser);

                res.setHeader('Content-Type', 'application/json');
                res.statusCode = 201; // Created
                res.end(JSON.stringify(newUser));
            } catch (err) {
                res.statusCode = 400; // Bad Request
                res.end(JSON.stringify({ error: "Invalid JSON" }));
            }
        });
    }

    // PUT /api/users/:id - Update user
    else if (method === 'PUT' && url.startsWith('/api/users/')) {
        const id = parseInt(url.split('/')[3]);
        let body = '';

        req.on('data', chunk => {
            body += chunk.toString();
        });

        req.on('end', () => {
            const updatedData = JSON.parse(body);
            const userIndex = users.findIndex(u => u.id === id);

            if (userIndex !== -1) {
                users[userIndex] = { ...users[userIndex], ...updatedData };
                res.setHeader('Content-Type', 'application/json');
                res.statusCode = 200;
                res.end(JSON.stringify(users[userIndex]));
            } else {
                res.statusCode = 404;
                res.end(JSON.stringify({ error: "User not found" }));
            }
        });
    }

    // DELETE /api/users/:id - Delete user
    else if (method === 'DELETE' && url.startsWith('/api/users/')) {
        const id = parseInt(url.split('/')[3]);
        const userIndex = users.findIndex(u => u.id === id);

        if (userIndex !== -1) {
            const deletedUser = users.splice(userIndex, 1)[0];
            res.setHeader('Content-Type', 'application/json');
            res.statusCode = 200;
            res.end(JSON.stringify({ message: "User deleted", user: deletedUser }));
        } else {
            res.statusCode = 404;
            res.end(JSON.stringify({ error: "User not found" }));
        }
    }

    // 404: Route not found
    else {
        res.statusCode = 404;
        res.end(JSON.stringify({ error: "Endpoint not found" }));
    }
});

server.listen(3000, () => {
    console.log("Server running on http://localhost:3000");
    console.log("\nAPI Endpoints:");
    console.log("  GET    /api/users      - Get all users");
    console.log("  GET    /api/users/:id  - Get single user");
    console.log("  POST   /api/users      - Create user");
    console.log("  PUT    /api/users/:id  - Update user");
    console.log("  DELETE /api/users/:id  - Delete user");
});
```

**Testing with curl:**

```bash
# GET all users
curl http://localhost:3000/api/users
# Output: [{"id":1,"name":"Akshay","email":"akshay@test.com"},{"id":2,"name":"Saini","email":"saini@test.com"}]

# GET single user
curl http://localhost:3000/api/users/1
# Output: {"id":1,"name":"Akshay","email":"akshay@test.com"}

# POST - Create new user
curl -X POST http://localhost:3000/api/users \
  -H "Content-Type: application/json" \
  -d '{"name":"Tejas","email":"tejas@test.com"}'
# Output: {"name":"Tejas","email":"tejas@test.com","id":3}

# PUT - Update user
curl -X PUT http://localhost:3000/api/users/1 \
  -H "Content-Type: application/json" \
  -d '{"email":"akshay.new@test.com"}'
# Output: {"id":1,"name":"Akshay","email":"akshay.new@test.com"}

# DELETE user
curl -X DELETE http://localhost:3000/api/users/2
# Output: {"message":"User deleted","user":{"id":2,"name":"Saini","email":"saini@test.com"}}
```

**📝 Detailed Explanation:**

**1. Reading Request Body (POST/PUT):**
```javascript
let body = '';

// Data comes in chunks (streams)
req.on('data', chunk => {
    body += chunk.toString();
});

// When all chunks received
req.on('end', () => {
    const data = JSON.parse(body);
    // Now use data
});
```
- Request body is a **stream** (not available immediately)
- Use `'data'` event to collect chunks
- Use `'end'` event when complete
- Parse JSON string to JavaScript object

**2. Setting Status Codes:**
```javascript
res.statusCode = 200;  // OK
res.statusCode = 201;  // Created
res.statusCode = 400;  // Bad Request
res.statusCode = 404;  // Not Found
res.statusCode = 500;  // Server Error
```

**3. Setting Headers:**
```javascript
res.setHeader('Content-Type', 'application/json');
res.setHeader('Access-Control-Allow-Origin', '*');
```

**4. CRUD Operations:**
- **C**reate - POST `/api/users`
- **R**ead - GET `/api/users` or `/api/users/:id`
- **U**pdate - PUT `/api/users/:id`
- **D**elete - DELETE `/api/users/:id`

---

**Example 5: Serving HTML Pages**

```javascript
// html-server.js

const http = require('http');
const fs = require('fs');
const path = require('path');

const server = http.createServer((req, res) => {
    let filePath;

    // Determine which file to serve
    if (req.url === '/' || req.url === '/index.html') {
        filePath = path.join(__dirname, 'public', 'index.html');
    } else if (req.url === '/about.html') {
        filePath = path.join(__dirname, 'public', 'about.html');
    } else if (req.url === '/style.css') {
        filePath = path.join(__dirname, 'public', 'style.css');
    } else {
        // 404
        res.statusCode = 404;
        res.setHeader('Content-Type', 'text/html');
        res.end('<h1>404 - Page Not Found</h1>');
        return;
    }

    // Read and serve file
    fs.readFile(filePath, (err, content) => {
        if (err) {
            res.statusCode = 500;
            res.end('Server Error');
            return;
        }

        // Set content type based on file extension
        const ext = path.extname(filePath);
        let contentType = 'text/html';

        if (ext === '.css') {
            contentType = 'text/css';
        } else if (ext === '.js') {
            contentType = 'text/javascript';
        } else if (ext === '.json') {
            contentType = 'application/json';
        }

        res.setHeader('Content-Type', contentType);
        res.statusCode = 200;
        res.end(content);
    });
});

server.listen(3000, () => {
    console.log("Server running on http://localhost:3000");
});
```

**Create HTML files:**

```html
<!-- public/index.html -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Node.js Server</title>
    <link rel="stylesheet" href="/style.css">
</head>
<body>
    <h1>🚀 Welcome to Node.js Server</h1>
    <p>This page is served by a Node.js HTTP server!</p>
    <nav>
        <a href="/">Home</a>
        <a href="/about.html">About</a>
    </nav>
</body>
</html>
```

```html
<!-- public/about.html -->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>About</title>
    <link rel="stylesheet" href="/style.css">
</head>
<body>
    <h1>ℹ️ About Us</h1>
    <p>We build servers with Node.js!</p>
    <a href="/">Back to Home</a>
</body>
</html>
```

```css
/* public/style.css */
body {
    font-family: Arial, sans-serif;
    max-width: 800px;
    margin: 50px auto;
    padding: 20px;
    background: #f0f0f0;
}

h1 {
    color: #333;
}

a {
    margin-right: 15px;
    color: #0066cc;
    text-decoration: none;
}
```

**📝 Explanation:**

- Server reads HTML files from disk using `fs.readFile()`
- Sets appropriate `Content-Type` header based on file extension
- Serves CSS, JS, images, etc. the same way
- This is how **static file serving** works!
- Frameworks like Express have this built-in with `express.static()`

---

**Example 6: Complete REST API with Error Handling**

```javascript
// rest-api-complete.js

const http = require('http');

// Database simulation
let products = [
    { id: 1, name: "Laptop", price: 50000, stock: 10 },
    { id: 2, name: "Phone", price: 20000, stock: 25 },
    { id: 3, name: "Tablet", price: 15000, stock: 15 }
];

let nextId = 4;

// Helper: Send JSON response
function sendJSON(res, statusCode, data) {
    res.setHeader('Content-Type', 'application/json');
    res.statusCode = statusCode;
    res.end(JSON.stringify(data));
}

// Helper: Parse request body
function getRequestBody(req) {
    return new Promise((resolve, reject) => {
        let body = '';
        req.on('data', chunk => body += chunk);
        req.on('end', () => {
            try {
                resolve(JSON.parse(body));
            } catch (err) {
                reject(err);
            }
        });
    });
}

const server = http.createServer(async (req, res) => {
    const { method, url } = req;

    console.log(`[${new Date().toISOString()}] ${method} ${url}`);

    try {
        // CORS headers
        res.setHeader('Access-Control-Allow-Origin', '*');
        res.setHeader('Access-Control-Allow-Methods', 'GET, POST, PUT, DELETE');
        res.setHeader('Access-Control-Allow-Headers', 'Content-Type');

        if (method === 'OPTIONS') {
            res.statusCode = 204;
            res.end();
            return;
        }

        // GET /api/products - Get all products
        if (method === 'GET' && url === '/api/products') {
            sendJSON(res, 200, {
                success: true,
                count: products.length,
                data: products
            });
        }

        // GET /api/products/:id - Get single product
        else if (method === 'GET' && url.match(/^\/api\/products\/\d+$/)) {
            const id = parseInt(url.split('/')[3]);
            const product = products.find(p => p.id === id);

            if (product) {
                sendJSON(res, 200, {
                    success: true,
                    data: product
                });
            } else {
                sendJSON(res, 404, {
                    success: false,
                    error: "Product not found"
                });
            }
        }

        // POST /api/products - Create product
        else if (method === 'POST' && url === '/api/products') {
            const body = await getRequestBody(req);

            // Validation
            if (!body.name || !body.price || !body.stock) {
                sendJSON(res, 400, {
                    success: false,
                    error: "Name, price, and stock are required"
                });
                return;
            }

            const newProduct = {
                id: nextId++,
                name: body.name,
                price: parseFloat(body.price),
                stock: parseInt(body.stock)
            };

            products.push(newProduct);

            sendJSON(res, 201, {
                success: true,
                message: "Product created",
                data: newProduct
            });
        }

        // PUT /api/products/:id - Update product
        else if (method === 'PUT' && url.match(/^\/api\/products\/\d+$/)) {
            const id = parseInt(url.split('/')[3]);
            const body = await getRequestBody(req);
            const productIndex = products.findIndex(p => p.id === id);

            if (productIndex !== -1) {
                products[productIndex] = {
                    ...products[productIndex],
                    ...body,
                    id: id  // Ensure ID doesn't change
                };

                sendJSON(res, 200, {
                    success: true,
                    message: "Product updated",
                    data: products[productIndex]
                });
            } else {
                sendJSON(res, 404, {
                    success: false,
                    error: "Product not found"
                });
            }
        }

        // DELETE /api/products/:id - Delete product
        else if (method === 'DELETE' && url.match(/^\/api\/products\/\d+$/)) {
            const id = parseInt(url.split('/')[3]);
            const productIndex = products.findIndex(p => p.id === id);

            if (productIndex !== -1) {
                const deletedProduct = products.splice(productIndex, 1)[0];
                sendJSON(res, 200, {
                    success: true,
                    message: "Product deleted",
                    data: deletedProduct
                });
            } else {
                sendJSON(res, 404, {
                    success: false,
                    error: "Product not found"
                });
            }
        }

        // 404 - Route not found
        else {
            sendJSON(res, 404, {
                success: false,
                error: "Endpoint not found"
            });
        }

    } catch (err) {
        console.error("Server error:", err);
        sendJSON(res, 500, {
            success: false,
            error: "Internal server error"
        });
    }
});

const PORT = 3000;
server.listen(PORT, () => {
    console.log(`🚀 Server running on http://localhost:${PORT}`);
    console.log("\n📋 API Endpoints:");
    console.log("  GET    /api/products      - Get all products");
    console.log("  GET    /api/products/:id  - Get single product");
    console.log("  POST   /api/products      - Create product");
    console.log("  PUT    /api/products/:id  - Update product");
    console.log("  DELETE /api/products/:id  - Delete product");
});
```

**Test the API:**

```bash
# Get all products
curl http://localhost:3000/api/products

# Get single product
curl http://localhost:3000/api/products/1

# Create product
curl -X POST http://localhost:3000/api/products \
  -H "Content-Type: application/json" \
  -d '{"name":"Headphones","price":2000,"stock":50}'

# Update product
curl -X PUT http://localhost:3000/api/products/1 \
  -H "Content-Type: application/json" \
  -d '{"price":45000}'

# Delete product
curl -X DELETE http://localhost:3000/api/products/2
```

**📝 Best Practices Demonstrated:**

1. **Consistent Response Format:**
   ```json
   {
     "success": true/false,
     "message": "...",
     "data": {...},
     "error": "..."
   }
   ```

2. **Proper Status Codes:**
   - 200 - Success
   - 201 - Created
   - 400 - Bad Request
   - 404 - Not Found
   - 500 - Server Error

3. **Error Handling:**
   - Try-catch for all async operations
   - Validation before processing
   - Graceful error responses

4. **Helper Functions:**
   - `sendJSON()` - DRY principle
   - `getRequestBody()` - Promise-based body parsing

5. **Logging:**
   - Log all requests with timestamp
   - Log errors for debugging

---

### **❌ Common Mistakes & Fixes:**

1. **Mistake: Forgetting to Call `res.end()`**
   - ❌ Problem: Response never sent, browser keeps loading
   ```javascript
   // ❌ WRONG - Response never sent!
   const server = http.createServer((req, res) => {
       res.statusCode = 200;
       res.setHeader('Content-Type', 'text/plain');
       // Forgot res.end()!
   });
   // Browser will timeout waiting for response
   ```
   - ✅ Fix: Always call `res.end()`
   ```javascript
   // ✅ CORRECT
   const server = http.createServer((req, res) => {
       res.statusCode = 200;
       res.setHeader('Content-Type', 'text/plain');
       res.end("Hello World");  // ✅ Response sent!
   });
   ```
   - 💡 **Remember**: `res.end()` is mandatory. Without it, connection hangs!

2. **Mistake: Not Handling Request Body Correctly**
   - ❌ Problem: Trying to access body synchronously
   ```javascript
   // ❌ WRONG - Body is not available yet!
   const server = http.createServer((req, res) => {
       console.log(req.body);  // undefined! No such property!

       const data = JSON.parse(req.body);  // Crashes!
       res.end("Done");
   });
   ```
   - ✅ Fix: Use streams to collect body
   ```javascript
   // ✅ CORRECT - Collect body chunks
   const server = http.createServer((req, res) => {
       let body = '';

       req.on('data', chunk => {
           body += chunk.toString();
       });

       req.on('end', () => {
           try {
               const data = JSON.parse(body);
               console.log("Received:", data);
               res.end("Done");
           } catch (err) {
               res.statusCode = 400;
               res.end("Invalid JSON");
           }
       });
   });
   ```

3. **Mistake: Port Already in Use Error**
   - ❌ Problem: Trying to start server on occupied port
   ```bash
   Error: listen EADDRINUSE: address already in use :::3000
   ```
   - ✅ Fix: Use different port or kill existing process
   ```javascript
   // Option 1: Use different port
   const PORT = process.env.PORT || 3000;
   server.listen(PORT);

   // Option 2: Handle error
   server.listen(3000).on('error', (err) => {
       if (err.code === 'EADDRINUSE') {
           console.log('Port 3000 is busy, trying 3001...');
           server.listen(3001);
       }
   });

   // Option 3: Kill process using port (terminal)
   // macOS/Linux:
   lsof -ti:3000 | xargs kill

   // Windows:
   netstat -ano | findstr :3000
   taskkill /PID <PID> /F
   ```

4. **Mistake: Not Setting Correct Content-Type**
   - ❌ Problem: Sending JSON without proper header
   ```javascript
   // ❌ WRONG - Browser may not parse as JSON
   const server = http.createServer((req, res) => {
       const data = { message: "Hello" };
       res.end(JSON.stringify(data));
       // Missing Content-Type header!
   });
   ```
   - ✅ Fix: Always set Content-Type for JSON
   ```javascript
   // ✅ CORRECT
   const server = http.createServer((req, res) => {
       const data = { message: "Hello" };
       res.setHeader('Content-Type', 'application/json');
       res.end(JSON.stringify(data));
   });
   ```

5. **Mistake: Blocking Server with Synchronous Operations**
   - ❌ Problem: Using sync file operations
   ```javascript
   // ❌ BAD - Blocks entire server!
   const server = http.createServer((req, res) => {
       const data = fs.readFileSync('large-file.txt');  // BLOCKS!
       res.end(data);
   });
   // While reading file, server can't handle other requests!
   ```
   - ✅ Fix: Use async operations
   ```javascript
   // ✅ GOOD - Non-blocking
   const server = http.createServer((req, res) => {
       fs.readFile('large-file.txt', (err, data) => {
           if (err) {
               res.statusCode = 500;
               res.end('Error');
               return;
           }
           res.end(data);
       });
   });
   // Server remains responsive while reading file
   ```

---

### **❓ Interview Questions:**

1. **Q: Create a simple HTTP server in Node.js that responds with "Hello World".**
   - **A:**

   ```javascript
   const http = require('http');

   const server = http.createServer((req, res) => {
       res.statusCode = 200;
       res.setHeader('Content-Type', 'text/plain');
       res.end('Hello World');
   });

   const PORT = 3000;
   server.listen(PORT, () => {
       console.log(`Server running at http://localhost:${PORT}/`);
   });
   ```

   **Explanation:**
   - `http.createServer()` creates server instance
   - Callback `(req, res) => {}` handles each request
   - `res.statusCode = 200` sets HTTP status
   - `res.setHeader()` sets response headers
   - `res.end()` sends response and closes connection
   - `server.listen()` starts server on port 3000

2. **Q: How do you handle different routes in a Node.js HTTP server?**
   - **A:**

   ```javascript
   const http = require('http');

   const server = http.createServer((req, res) => {
       const { url, method } = req;

       // Set common headers
       res.setHeader('Content-Type', 'text/html');

       // Route handling
       if (url === '/' && method === 'GET') {
           res.statusCode = 200;
           res.end('<h1>Home Page</h1>');
       }
       else if (url === '/about' && method === 'GET') {
           res.statusCode = 200;
           res.end('<h1>About Page</h1>');
       }
       else if (url === '/api/data' && method === 'GET') {
           res.setHeader('Content-Type', 'application/json');
           res.statusCode = 200;
           res.end(JSON.stringify({ message: 'API data' }));
       }
       else {
           // 404 - Not Found
           res.statusCode = 404;
           res.end('<h1>404 - Page Not Found</h1>');
       }
   });

   server.listen(3000);
   ```

   **Key Points:**
   - Check both `req.url` and `req.method`
   - Use if-else or switch-case for routing
   - Set appropriate status codes
   - Handle 404 for unknown routes
   - **Note**: In production, use routers (Express.js) instead of manual if-else

3. **Q: How do you read POST request body in Node.js HTTP server?**
   - **A:**

   ```javascript
   const http = require('http');

   const server = http.createServer((req, res) => {
       if (req.method === 'POST' && req.url === '/api/data') {
           let body = '';

           // Listen for data chunks
           req.on('data', chunk => {
               body += chunk.toString();
           });

           // When all data received
           req.on('end', () => {
               try {
                   const data = JSON.parse(body);
                   console.log('Received:', data);

                   res.setHeader('Content-Type', 'application/json');
                   res.statusCode = 200;
                   res.end(JSON.stringify({
                       message: 'Data received',
                       received: data
                   }));
               } catch (err) {
                   res.statusCode = 400;
                   res.end('Invalid JSON');
               }
           });
       } else {
           res.statusCode = 404;
           res.end('Not Found');
       }
   });

   server.listen(3000);
   ```

   **Why streams?**
   - Request body arrives in **chunks** (streaming)
   - Could be large (MB/GB of data)
   - `data` event fires for each chunk
   - `end` event fires when complete
   - Must concatenate chunks to get full body

   **Test with curl:**
   ```bash
   curl -X POST http://localhost:3000/api/data \
     -H "Content-Type: application/json" \
     -d '{"name":"Akshay","age":30}'
   ```

4. **Q: What's the difference between `res.write()` and `res.end()`?**
   - **A:**

   **`res.write(data)`:**
   - Sends a chunk of response body
   - Can be called multiple times
   - **Does NOT end the response**
   - Used for streaming large responses

   **`res.end([data])`:**
   - Signals that response is complete
   - Optionally sends final data
   - **Must be called** to close connection
   - Called only once per response

   **Example:**
   ```javascript
   const server = http.createServer((req, res) => {
       res.setHeader('Content-Type', 'text/html');

       // Send response in chunks
       res.write('<html><body>');
       res.write('<h1>Hello</h1>');
       res.write('<p>This is sent in chunks</p>');
       res.write('</body></html>');

       // Close connection
       res.end();
   });
   ```

   **Or combined:**
   ```javascript
   const server = http.createServer((req, res) => {
       res.write('<h1>Hello</h1>');
       res.end('<p>Final chunk and close</p>');
   });
   ```

   **Streaming large file:**
   ```javascript
   const server = http.createServer((req, res) => {
       const stream = fs.createReadStream('large-video.mp4');

       stream.on('data', chunk => {
           res.write(chunk);  // Send chunk by chunk
       });

       stream.on('end', () => {
           res.end();  // Close when done
       });
   });

   // Or use pipe (simpler):
   const server = http.createServer((req, res) => {
       const stream = fs.createReadStream('large-video.mp4');
       stream.pipe(res);  // Automatically writes and ends
   });
   ```

   **Key Rule:**
   - `res.write()` = Add more data
   - `res.end()` = Finish and close (mandatory!)

---

### **🌐 Real-World Usage:**

**How HTTP Servers Are Used in Production:**

1. **REST API for Mobile App**
   ```javascript
   // API backend for mobile app
   const server = http.createServer(async (req, res) => {
       const { method, url } = req;

       // Authenticate user
       const token = req.headers.authorization;
       if (!isValidToken(token)) {
           res.statusCode = 401;
           res.end(JSON.stringify({ error: 'Unauthorized' }));
           return;
       }

       // Route to controllers
       if (url.startsWith('/api/posts')) {
           await handlePosts(req, res);
       } else if (url.startsWith('/api/users')) {
           await handleUsers(req, res);
       }
   });
   ```

2. **Webhook Receiver**
   ```javascript
   // Receive webhooks from payment gateway
   const server = http.createServer(async (req, res) => {
       if (req.method === 'POST' && req.url === '/webhooks/payment') {
           const body = await getBody(req);
           const event = JSON.parse(body);

           // Process payment event
           if (event.type === 'payment.success') {
               await updateOrderStatus(event.orderId, 'paid');
               await sendConfirmationEmail(event.userId);
           }

           res.statusCode = 200;
           res.end('OK');
       }
   });
   ```

3. **Health Check Endpoint**
   ```javascript
   // For load balancers, monitoring tools
   const server = http.createServer((req, res) => {
       if (req.url === '/health') {
           const health = {
               status: 'OK',
               timestamp: Date.now(),
               uptime: process.uptime(),
               memory: process.memoryUsage()
           };

           res.setHeader('Content-Type', 'application/json');
           res.end(JSON.stringify(health));
       }
   });
   ```

4. **Microservice**
   ```javascript
   // Small service in microservice architecture
   const server = http.createServer(async (req, res) => {
       // User service - handles only user-related operations
       if (url === '/api/users/validate') {
           const isValid = await validateUser(userId);
           res.end(JSON.stringify({ valid: isValid }));
       }
   });

   // Other microservices call this endpoint
   ```

5. **Static File Server (CDN)**
   ```javascript
   // Serve static assets (images, CSS, JS)
   const server = http.createServer((req, res) => {
       const filePath = path.join(__dirname, 'public', req.url);

       fs.stat(filePath, (err, stats) => {
           if (err || !stats.isFile()) {
               res.statusCode = 404;
               res.end('Not Found');
               return;
           }

           const stream = fs.createReadStream(filePath);
           stream.pipe(res);
       });
   });
   ```

**Real Production Setup:**
```
nginx (Reverse Proxy) :80/:443
        ↓
   Load Balancer
        ↓
    ┌───────┬───────┬───────┐
    │       │       │       │
 Node.js Node.js Node.js Node.js (Multiple instances)
 Server1 Server2 Server3 Server4
    │       │       │       │
    └───────┴───────┴───────┘
            ↓
        Database
```

---

### **⚡ Summary & Quick Revision:**

✅ **HTTP server created with `http.createServer(callback)`** - callback receives `req` (request) and `res` (response) for every incoming request!

✅ **Always call `res.end()`** to complete response - without it, browser hangs waiting forever!

✅ **Routing** done by checking `req.url` and `req.method` - use if-else or switch, but frameworks (Express) are better for complex routing!

✅ **Request body is a stream** - use `req.on('data')` and `req.on('end')` to collect chunks, then parse JSON - this is foundational for POST/PUT requests! 🚀

---

**🎯 Key Takeaway:**
Creating servers with raw `http` module teaches you fundamentals. In production, you'll use **Express.js** which abstracts all this, but knowing what happens underneath makes you a better developer!

**Simple Server Summary:**
```javascript
const http = require('http');

http.createServer((req, res) => {
    res.end('Hello!');
}).listen(3000);
```
That's it! Node.js server in 3 lines! 💪

---

**Next Episode Preview:** Episode-12 will explore **Databases - SQL & NoSQL** - understanding different database types, when to use which, and how they differ! 🔥
