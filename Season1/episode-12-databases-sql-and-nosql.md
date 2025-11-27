## Episode-12 | Databases - SQL & NoSQL

### **Lecture Title:**
Databases - SQL & NoSQL - Understanding Data Storage & Choosing the Right Database

---

### **🔥 Core Concept:**
Yeh episode backend development ki **foundation** hai! 💾 Akshay bhaiya explains the two major categories of databases - **SQL (Relational)** and **NoSQL (Non-Relational)** - their differences, strengths, weaknesses, and **when to use which**. You'll understand database fundamentals that every backend developer MUST know.

This isn't just theory - you'll learn **real-world decision-making**: Should you use PostgreSQL or MongoDB for your project? Why does Instagram use PostgreSQL? Why does Netflix use Cassandra? Understanding databases helps you build **scalable, efficient applications** and make **architectural decisions** that can make or break your project.

**Why it's important:**
- **Every backend application needs a database** - it's not optional!
- Choosing the wrong database can cause major problems later (performance, scalability)
- Common interview topic: "SQL vs NoSQL - explain differences and use cases"
- Understanding databases is crucial for system design interviews
- Helps you optimize queries, prevent bottlenecks, and scale applications
- Foundation for learning specific databases (MongoDB, PostgreSQL, MySQL, Redis, etc.)

---

### **💡 Key Definitions:**

- **Database**: An organized collection of structured data stored electronically, designed for efficient retrieval, management, and updates.

- **SQL (Structured Query Language)**: A standardized language for managing and querying relational databases.

- **Relational Database (RDBMS)**: Database that stores data in tables (rows and columns) with predefined relationships between them.

- **NoSQL (Not Only SQL)**: Broad category of databases that don't use traditional relational table structures - designed for flexible schemas and horizontal scaling.

- **Schema**: The structure/blueprint that defines how data is organized in a database (tables, columns, data types, relationships).

- **Table**: In SQL databases, a collection of related data organized in rows and columns (like a spreadsheet).

- **Row/Record**: A single entry in a table containing related data (e.g., one user's information).

- **Column/Field**: A specific attribute/property in a table (e.g., "name", "email", "age").

- **Primary Key**: A unique identifier for each record in a table (e.g., user ID).

- **Foreign Key**: A field in one table that links to the primary key of another table (establishes relationships).

- **ACID Properties**: Atomicity, Consistency, Isolation, Durability - guarantees for reliable database transactions.

- **BASE Properties**: Basically Available, Soft state, Eventual consistency - characteristics of many NoSQL databases.

- **Normalization**: Process of organizing data to reduce redundancy and improve data integrity in SQL databases.

- **Horizontal Scaling (Scale Out)**: Adding more servers/nodes to distribute the load.

- **Vertical Scaling (Scale Up)**: Adding more resources (CPU, RAM) to existing server.

- **Sharding**: Distributing data across multiple database instances for horizontal scaling.

- **Replication**: Copying data across multiple servers for redundancy and availability.

- **Collection**: In NoSQL (especially MongoDB), equivalent to a table in SQL - a group of documents.

- **Document**: In document-based NoSQL (MongoDB), equivalent to a row/record - a JSON-like object.

---

### **⚙️ Step-by-Step Explanation:**

**Understanding Database Types:**

1. **What is a Database?**
   ```
   Database = Organized storage for application data

   Without Database:
   ❌ Data stored in files (unorganized)
   ❌ Hard to search, update, manage
   ❌ No concurrent access control
   ❌ No data integrity guarantees

   With Database:
   ✅ Structured storage
   ✅ Fast queries and searches
   ✅ Concurrent access management
   ✅ Data integrity and consistency
   ✅ Security and access control
   ```

2. **SQL (Relational) Databases**

   **Structure: Tables with Rows and Columns**
   ```
   Users Table:
   ┌────────┬──────────┬──────────────────────┬─────┐
   │ UserID │   Name   │        Email         │ Age │
   ├────────┼──────────┼──────────────────────┼─────┤
   │   1    │ Akshay   │ akshay@example.com   │ 30  │
   │   2    │ Tejas    │ tejas@example.com    │ 25  │
   │   3    │ Priya    │ priya@example.com    │ 28  │
   └────────┴──────────┴──────────────────────┴─────┘

   Orders Table:
   ┌─────────┬────────┬──────────┬────────┐
   │ OrderID │ UserID │ Product  │ Amount │
   ├─────────┼────────┼──────────┼────────┤
   │  101    │   1    │ Laptop   │ 50000  │
   │  102    │   2    │ Phone    │ 20000  │
   │  103    │   1    │ Mouse    │  500   │
   └─────────┴────────┴──────────┴────────┘

   Relationship: Orders.UserID → Users.UserID (Foreign Key)
   ```

   **Key Characteristics:**
   - **Fixed Schema**: Table structure defined beforehand
   - **ACID Compliance**: Strong consistency guarantees
   - **Relationships**: Tables linked via foreign keys
   - **SQL Language**: Standard query language
   - **Normalization**: Data organized to reduce redundancy

   **Popular SQL Databases:**
   - PostgreSQL (most feature-rich, open source)
   - MySQL (widely used, web applications)
   - Oracle Database (enterprise, expensive)
   - Microsoft SQL Server (enterprise, Windows)
   - SQLite (lightweight, embedded)

3. **NoSQL Databases**

   **Types of NoSQL Databases:**

   **A. Document-Based (MongoDB, CouchDB)**
   ```json
   // Collection: Users (like table, but flexible)

   // Document 1 (like row, but JSON-like)
   {
       "_id": "507f1f77bcf86cd799439011",
       "name": "Akshay",
       "email": "akshay@example.com",
       "age": 30,
       "address": {
           "city": "Mumbai",
           "country": "India"
       },
       "hobbies": ["coding", "teaching"]
   }

   // Document 2 (can have different fields!)
   {
       "_id": "507f1f77bcf86cd799439012",
       "name": "Tejas",
       "email": "tejas@example.com",
       "skills": ["JavaScript", "Node.js"],
       "projects": 15
       // No 'age' or 'address' - flexible schema!
   }
   ```

   **B. Key-Value Stores (Redis, DynamoDB)**
   ```
   Key: "user:1001"        Value: { name: "Akshay", email: "a@b.com" }
   Key: "session:xyz123"   Value: { userId: 1001, expires: 1234567890 }
   Key: "cache:homepage"   Value: "<html>...</html>"

   Super fast lookups by key (O(1) complexity)
   Like a giant hash map/dictionary
   ```

   **C. Column-Family (Cassandra, HBase)**
   ```
   Used for wide-column storage
   Great for time-series data, analytics
   Scales to petabytes

   Example: Netflix uses Cassandra
   - User viewing history
   - Billions of records
   - Distributed across data centers
   ```

   **D. Graph Databases (Neo4j, Amazon Neptune)**
   ```
   Stores data as nodes and relationships
   Perfect for social networks, recommendation engines

   Example:
   (User: Akshay) --[FOLLOWS]--> (User: Tejas)
   (User: Akshay) --[LIKES]--> (Post: "Node.js Tutorial")
   (User: Tejas) --[COMMENTED_ON]--> (Post: "Node.js Tutorial")

   Queries like "Friends of friends" are super fast
   ```

   **Key Characteristics:**
   - **Flexible Schema**: No predefined structure required
   - **Horizontal Scaling**: Easier to distribute across servers
   - **BASE Properties**: Eventual consistency (not always ACID)
   - **Specialized**: Each type optimized for specific use cases

4. **ACID vs BASE**

   **ACID (SQL Databases):**
   ```
   A - Atomicity: All or nothing (transaction succeeds completely or fails completely)
   C - Consistency: Data always in valid state (follows all rules)
   I - Isolation: Concurrent transactions don't interfere
   D - Durability: Once committed, data persists (even if system crashes)

   Example:
   Bank Transfer: $100 from Account A to Account B

   1. Deduct $100 from A
   2. Add $100 to B

   ACID ensures:
   - Both steps happen or neither (Atomicity)
   - Total money stays same (Consistency)
   - Other transactions don't see intermediate state (Isolation)
   - Data persists after commit (Durability)

   If power failure during transaction → entire transaction rolls back
   ```

   **BASE (Many NoSQL Databases):**
   ```
   B - Basically Available: System available most of the time
   A - Soft state: State may change over time (eventual consistency)
   S - Eventual consistency: Data becomes consistent eventually
   E - Eventual consistency: (not immediately)

   Example:
   Social Media Post:
   1. You post a photo
   2. Different users might see it at different times
   3. Eventually, everyone sees it (within seconds/minutes)

   Trade-off: Availability and performance over immediate consistency
   ```

5. **CAP Theorem (Fundamental Distributed Systems Concept)**
   ```
   CAP Theorem: You can have at most 2 of 3:

   C - Consistency: All nodes see same data at same time
   A - Availability: System always responds to requests
   P - Partition Tolerance: System works despite network failures

   In distributed systems, network failures WILL happen (P is mandatory)
   So choice is between C and A:

   CP (Consistency + Partition Tolerance):
   - Sacrifice availability for consistency
   - Example: Traditional SQL databases, MongoDB (default)
   - Use when: Banking, financial transactions

   AP (Availability + Partition Tolerance):
   - Sacrifice consistency for availability
   - Example: Cassandra, DynamoDB
   - Use when: Social media, analytics, caching

   CA (Consistency + Availability):
   - Not possible in distributed systems!
   - (Network partitions are inevitable)
   ```

6. **When to Use SQL vs NoSQL**

   **Use SQL (Relational) When:**
   ```
   ✅ Complex relationships between data
   ✅ Need ACID transactions (banking, e-commerce)
   ✅ Data structure is well-defined and stable
   ✅ Need powerful query capabilities (JOINs, aggregations)
   ✅ Data integrity is critical

   Examples:
   - Banking systems (transactions must be ACID)
   - E-commerce (inventory, orders, users)
   - CRM systems (customers, sales, interactions)
   - Accounting software
   - Any app with complex relationships
   ```

   **Use NoSQL When:**
   ```
   ✅ Flexible/evolving schema (startup, rapid iteration)
   ✅ Massive scale (millions/billions of records)
   ✅ High write throughput needed
   ✅ Simple queries (get by ID, no complex JOINs)
   ✅ Horizontal scaling required

   Examples:
   - Social media (posts, likes, comments - billions of records)
   - Real-time analytics (logs, metrics, time-series data)
   - Caching (session data, temporary data)
   - Content management (flexible document structure)
   - IoT data (sensor readings, high volume)
   ```

---

### **💻 Code Example & Detailed Explanation:**

**Example 1: SQL Database Structure**

```sql
-- Creating tables with relationships (SQL)

-- Users table
CREATE TABLE Users (
    UserID INT PRIMARY KEY AUTO_INCREMENT,
    Name VARCHAR(100) NOT NULL,
    Email VARCHAR(100) UNIQUE NOT NULL,
    Password VARCHAR(255) NOT NULL,
    CreatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Posts table (belongs to Users)
CREATE TABLE Posts (
    PostID INT PRIMARY KEY AUTO_INCREMENT,
    UserID INT NOT NULL,
    Title VARCHAR(200) NOT NULL,
    Content TEXT,
    CreatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (UserID) REFERENCES Users(UserID) ON DELETE CASCADE
);

-- Comments table (belongs to Posts and Users)
CREATE TABLE Comments (
    CommentID INT PRIMARY KEY AUTO_INCREMENT,
    PostID INT NOT NULL,
    UserID INT NOT NULL,
    Content TEXT NOT NULL,
    CreatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (PostID) REFERENCES Posts(PostID) ON DELETE CASCADE,
    FOREIGN KEY (UserID) REFERENCES Users(UserID) ON DELETE CASCADE
);

-- Inserting data
INSERT INTO Users (Name, Email, Password)
VALUES ('Akshay', 'akshay@example.com', 'hashed_password_123');

INSERT INTO Posts (UserID, Title, Content)
VALUES (1, 'Learning Node.js', 'Node.js is awesome for backend development!');

INSERT INTO Comments (PostID, UserID, Content)
VALUES (1, 1, 'Great post! Very helpful.');

-- Complex query with JOINs
SELECT
    u.Name AS AuthorName,
    p.Title AS PostTitle,
    c.Content AS CommentText,
    c.CreatedAt AS CommentDate
FROM Comments c
INNER JOIN Users u ON c.UserID = u.UserID
INNER JOIN Posts p ON c.PostID = p.PostID
WHERE p.PostID = 1
ORDER BY c.CreatedAt DESC;
```

**📝 Explanation:**

**Fixed Schema:**
- Every table has predefined columns with specific data types
- `UserID INT` - must be integer
- `Email VARCHAR(100)` - string, max 100 characters
- Can't add random fields without altering table structure

**Relationships:**
- `FOREIGN KEY (UserID) REFERENCES Users(UserID)` - enforces relationship
- If you delete a user, all their posts are deleted too (`ON DELETE CASCADE`)
- Data integrity guaranteed by database

**ACID Transactions:**
```sql
START TRANSACTION;

INSERT INTO Posts (UserID, Title, Content)
VALUES (1, 'New Post', 'Content');

INSERT INTO Comments (PostID, UserID, Content)
VALUES (LAST_INSERT_ID(), 1, 'First comment');

COMMIT; -- Both succeed or both fail (Atomicity)
```

**Powerful Queries:**
- JOIN multiple tables
- Aggregate functions (COUNT, SUM, AVG)
- Complex WHERE clauses
- Subqueries

---

**Example 2: NoSQL Document Database (MongoDB)**

```javascript
// mongodb-example.js

const { MongoClient } = require('mongodb');

async function mongoDBExample() {
    const client = new MongoClient('mongodb://localhost:27017');

    try {
        await client.connect();
        console.log("Connected to MongoDB");

        const db = client.db('blogApp');
        const users = db.collection('users');
        const posts = db.collection('posts');

        // 1. Insert user (flexible schema - no predefined structure!)
        const user = {
            name: "Akshay",
            email: "akshay@example.com",
            age: 30,
            skills: ["JavaScript", "Node.js", "MongoDB"],
            address: {
                city: "Mumbai",
                country: "India"
            },
            createdAt: new Date()
        };

        const userResult = await users.insertOne(user);
        console.log("User inserted:", userResult.insertedId);

        // 2. Insert post (embedded document approach)
        const post = {
            userId: userResult.insertedId,
            title: "Learning Node.js",
            content: "Node.js is awesome!",
            tags: ["nodejs", "javascript", "backend"],
            comments: [  // Embedded comments (denormalized)
                {
                    userId: userResult.insertedId,
                    text: "Great post!",
                    createdAt: new Date()
                }
            ],
            likes: 0,
            createdAt: new Date()
        };

        const postResult = await posts.insertOne(post);
        console.log("Post inserted:", postResult.insertedId);

        // 3. Insert another user with DIFFERENT schema (flexibility!)
        const anotherUser = {
            name: "Tejas",
            email: "tejas@example.com",
            // No 'age' field
            // No 'address' field
            expertise: "Full-Stack Development",  // New field!
            projects: [  // Different structure
                { name: "E-commerce", tech: "MERN" },
                { name: "Blog", tech: "Next.js" }
            ],
            socialMedia: {  // Nested object with different fields
                twitter: "@tejas",
                github: "tejas-dev"
            }
        };

        await users.insertOne(anotherUser);
        console.log("Another user with different schema inserted!");

        // 4. Query - Find user by email
        const foundUser = await users.findOne({ email: "akshay@example.com" });
        console.log("Found user:", foundUser);

        // 5. Update - Add new comment to post (push to array)
        await posts.updateOne(
            { _id: postResult.insertedId },
            {
                $push: {
                    comments: {
                        userId: userResult.insertedId,
                        text: "Updated with new comment!",
                        createdAt: new Date()
                    }
                },
                $inc: { likes: 1 }  // Increment likes
            }
        );
        console.log("Post updated with new comment");

        // 6. Complex query - Find posts by user with specific tag
        const userPosts = await posts.find({
            userId: userResult.insertedId,
            tags: { $in: ["nodejs"] }  // Array contains "nodejs"
        }).toArray();

        console.log("User's posts with 'nodejs' tag:", userPosts.length);

        // 7. Aggregation pipeline (similar to SQL JOINs)
        const postsWithUserInfo = await posts.aggregate([
            {
                $lookup: {  // Like SQL JOIN
                    from: "users",
                    localField: "userId",
                    foreignField: "_id",
                    as: "author"
                }
            },
            {
                $unwind: "$author"  // Flatten array
            },
            {
                $project: {  // Select fields
                    title: 1,
                    "author.name": 1,
                    "author.email": 1,
                    commentsCount: { $size: "$comments" }
                }
            }
        ]).toArray();

        console.log("Posts with author info:", postsWithUserInfo);

    } finally {
        await client.close();
    }
}

mongoDBExample();
```

**Output:**
```
Connected to MongoDB
User inserted: 507f1f77bcf86cd799439011
Post inserted: 507f1f77bcf86cd799439012
Another user with different schema inserted!
Found user: {
  _id: 507f1f77bcf86cd799439011,
  name: 'Akshay',
  email: 'akshay@example.com',
  age: 30,
  skills: [ 'JavaScript', 'Node.js', 'MongoDB' ],
  address: { city: 'Mumbai', country: 'India' },
  createdAt: 2024-01-15T10:30:00.000Z
}
Post updated with new comment
User's posts with 'nodejs' tag: 1
Posts with author info: [ ... ]
```

**📝 Detailed Explanation:**

**Flexible Schema:**
- First user has `age`, `address`, `skills`
- Second user has `expertise`, `projects`, `socialMedia`
- Both valid! No schema enforcement
- Easy to evolve over time

**Embedded Documents:**
- `comments` array embedded in post document
- No need for separate Comments table
- Read optimization: Get post + comments in single query
- Trade-off: Updates to comments update entire document

**NoSQL Advantages Here:**
- Rapid development (no migrations for schema changes)
- Natural JSON structure (matches JavaScript objects)
- Denormalization for read performance
- Flexible data modeling

**NoSQL Challenges:**
- No referential integrity (can have orphaned references)
- Complex queries harder (no native JOINs, use aggregation)
- Data duplication (denormalization)
- Eventual consistency in distributed setups

---

**Example 3: SQL vs NoSQL - E-Commerce Scenario**

**SQL Approach (Normalized):**

```sql
-- Normalized schema (reduce redundancy)

CREATE TABLE Customers (
    CustomerID INT PRIMARY KEY,
    Name VARCHAR(100),
    Email VARCHAR(100),
    Phone VARCHAR(15)
);

CREATE TABLE Products (
    ProductID INT PRIMARY KEY,
    Name VARCHAR(200),
    Price DECIMAL(10, 2),
    Stock INT
);

CREATE TABLE Orders (
    OrderID INT PRIMARY KEY,
    CustomerID INT,
    OrderDate TIMESTAMP,
    Status VARCHAR(50),
    FOREIGN KEY (CustomerID) REFERENCES Customers(CustomerID)
);

CREATE TABLE OrderItems (
    OrderItemID INT PRIMARY KEY,
    OrderID INT,
    ProductID INT,
    Quantity INT,
    Price DECIMAL(10, 2),
    FOREIGN KEY (OrderID) REFERENCES Orders(OrderID),
    FOREIGN KEY (ProductID) REFERENCES Products(ProductID)
);

-- Query: Get order with all items and customer info
SELECT
    c.Name AS CustomerName,
    o.OrderID,
    o.OrderDate,
    p.Name AS ProductName,
    oi.Quantity,
    oi.Price
FROM Orders o
JOIN Customers c ON o.CustomerID = c.CustomerID
JOIN OrderItems oi ON o.OrderID = oi.OrderID
JOIN Products p ON oi.ProductID = p.ProductID
WHERE o.OrderID = 1001;
```

**NoSQL Approach (Denormalized):**

```javascript
// MongoDB - Embedded document approach

// Single document contains everything
{
    _id: ObjectId("..."),
    orderNumber: "ORD-1001",
    orderDate: ISODate("2024-01-15"),
    status: "shipped",
    customer: {  // Embedded customer info
        customerId: "CUST-5001",
        name: "Akshay Saini",
        email: "akshay@example.com",
        phone: "+91-9876543210"
    },
    items: [  // Embedded order items
        {
            productId: "PROD-2001",
            productName: "Laptop",
            quantity: 1,
            price: 50000,
            sku: "LAP-HP-15"
        },
        {
            productId: "PROD-2002",
            productName: "Mouse",
            quantity: 2,
            price: 500,
            sku: "MOU-LOG-M1"
        }
    ],
    totalAmount: 51000,
    shippingAddress: {
        street: "123 Main St",
        city: "Mumbai",
        pincode: "400001"
    }
}

// Single query to get everything!
db.orders.findOne({ orderNumber: "ORD-1001" });

// No JOINs needed - all data in one document
// Fast reads, but data duplication
// If customer email changes, need to update all their orders
```

**Comparison:**

| Aspect | SQL (Normalized) | NoSQL (Denormalized) |
|--------|------------------|----------------------|
| **Query** | Multiple JOINs | Single lookup |
| **Read Speed** | Slower (multiple tables) | Faster (one document) |
| **Write Speed** | Faster (no duplication) | Slower (update duplicates) |
| **Data Integrity** | Enforced by DB | Application responsibility |
| **Storage** | Efficient (no duplication) | More space (duplication) |
| **Consistency** | Always consistent | May be inconsistent temporarily |
| **Use Case** | Transactional (e-commerce checkout) | Read-heavy (order history) |

---

**Example 4: Real-World Hybrid Approach**

```javascript
// modern-ecommerce-architecture.js

// Many companies use BOTH SQL and NoSQL!

// 1. PostgreSQL (SQL) - Transactional data
// - User accounts
// - Inventory management
// - Order processing (ACID required)
// - Payment transactions

const { Pool } = require('pg');
const pool = new Pool({ connectionString: 'postgresql://...' });

// Handle order placement (ACID transaction)
async function placeOrder(userId, items) {
    const client = await pool.connect();

    try {
        await client.query('BEGIN');  // Start transaction

        // 1. Check and reserve inventory
        for (const item of items) {
            const result = await client.query(
                'UPDATE products SET stock = stock - $1 WHERE product_id = $2 AND stock >= $1 RETURNING *',
                [item.quantity, item.productId]
            );

            if (result.rowCount === 0) {
                throw new Error(`Insufficient stock for product ${item.productId}`);
            }
        }

        // 2. Create order
        const orderResult = await client.query(
            'INSERT INTO orders (user_id, total_amount, status) VALUES ($1, $2, $3) RETURNING order_id',
            [userId, calculateTotal(items), 'pending']
        );

        const orderId = orderResult.rows[0].order_id;

        // 3. Create order items
        for (const item of items) {
            await client.query(
                'INSERT INTO order_items (order_id, product_id, quantity, price) VALUES ($1, $2, $3, $4)',
                [orderId, item.productId, item.quantity, item.price]
            );
        }

        await client.query('COMMIT');  // All or nothing!

        // 4. After SQL transaction, cache in Redis for fast access
        await cacheOrder(orderId);

        // 5. Store order snapshot in MongoDB for analytics
        await archiveOrder(orderId);

        return orderId;

    } catch (err) {
        await client.query('ROLLBACK');  // Undo everything
        throw err;
    } finally {
        client.release();
    }
}

// 2. Redis (NoSQL Key-Value) - Caching & Sessions
const redis = require('redis');
const redisClient = redis.createClient();

// Cache frequently accessed data
async function cacheOrder(orderId) {
    const order = await getOrderFromSQL(orderId);
    await redisClient.set(
        `order:${orderId}`,
        JSON.stringify(order),
        'EX', 3600  // Expire in 1 hour
    );
}

// Fast session management
async function createSession(userId) {
    const sessionId = generateSessionId();
    await redisClient.set(
        `session:${sessionId}`,
        JSON.stringify({ userId, createdAt: Date.now() }),
        'EX', 86400  // 24 hours
    );
    return sessionId;
}

// 3. MongoDB (NoSQL Document) - Product catalog, reviews, logs
const { MongoClient } = require('mongodb');
const mongoClient = new MongoClient('mongodb://...');

// Flexible product catalog
async function updateProductCatalog(productId, updates) {
    const db = mongoClient.db('ecommerce');

    // Products can have different attributes
    await db.collection('products').updateOne(
        { _id: productId },
        {
            $set: updates,
            $push: {
                priceHistory: {
                    price: updates.price,
                    date: new Date()
                }
            }
        }
    );
}

// Store user reviews with flexible schema
async function addReview(productId, userId, review) {
    const db = mongoClient.db('ecommerce');

    await db.collection('reviews').insertOne({
        productId,
        userId,
        rating: review.rating,
        comment: review.comment,
        images: review.images || [],  // Optional
        verified: review.verified || false,
        helpful: 0,
        createdAt: new Date()
    });
}

// 4. Elasticsearch (NoSQL Search) - Product search
// Fast full-text search across millions of products

// 5. Cassandra (NoSQL Column-Family) - Analytics, logs
// Time-series data: page views, user behavior, etc.

// This is how real companies architecture their databases!
// Each database for what it does best
```

**Real-World Architecture:**

```
┌─────────────────────────────────────────────────┐
│            E-Commerce Application               │
├─────────────────────────────────────────────────┤
│                                                 │
│  ┌──────────────┐  ┌───────────────┐           │
│  │ PostgreSQL   │  │   MongoDB     │           │
│  │ (SQL)        │  │   (NoSQL)     │           │
│  │              │  │               │           │
│  │ - Users      │  │ - Products    │           │
│  │ - Inventory  │  │ - Reviews     │           │
│  │ - Orders     │  │ - Logs        │           │
│  │ - Payments   │  │ - Analytics   │           │
│  └──────────────┘  └───────────────┘           │
│                                                 │
│  ┌──────────────┐  ┌───────────────┐           │
│  │    Redis     │  │ Elasticsearch │           │
│  │ (Key-Value)  │  │   (Search)    │           │
│  │              │  │               │           │
│  │ - Sessions   │  │ - Product     │           │
│  │ - Cache      │  │   Search      │           │
│  │ - Rate Limit │  │ - Auto-       │           │
│  │              │  │   complete    │           │
│  └──────────────┘  └───────────────┘           │
│                                                 │
└─────────────────────────────────────────────────┘

Each database does what it's best at!
```

---

### **❌ Common Mistakes & Fixes:**

1. **Mistake: Using NoSQL for Everything Because "It's Trendy"**
   - ❌ Problem: Choosing MongoDB for a banking app
   ```javascript
   // ❌ BAD - Banking transactions in MongoDB
   db.accounts.updateOne(
       { accountId: "ACC-1001" },
       { $inc: { balance: -1000 } }
   );

   db.accounts.updateOne(
       { accountId: "ACC-2002" },
       { $inc: { balance: 1000 } }
   );

   // If power failure between updates:
   // - First account debited
   // - Second account NOT credited
   // - Money lost! No ACID guarantees
   ```
   - ✅ Fix: Use SQL for transactional data
   ```javascript
   // ✅ GOOD - SQL with ACID transaction
   BEGIN TRANSACTION;

   UPDATE accounts SET balance = balance - 1000 WHERE account_id = 'ACC-1001';
   UPDATE accounts SET balance = balance + 1000 WHERE account_id = 'ACC-2002';

   COMMIT;
   // Both succeed or both fail - no money lost!
   ```
   - 💡 **Rule**: Banking, payments, inventory = SQL. Flexible content, analytics = NoSQL.

2. **Mistake: Not Understanding Schema Design Differences**
   - ❌ Problem: Designing NoSQL database like SQL (over-normalization)
   ```javascript
   // ❌ BAD - Normalized approach in MongoDB (defeats purpose)

   // Users collection
   { _id: "user1", name: "Akshay" }

   // Posts collection
   { _id: "post1", userId: "user1", title: "..." }

   // Comments collection
   { _id: "comment1", postId: "post1", userId: "user1", text: "..." }

   // Now need to do multiple queries (like SQL JOINs)
   // Loses NoSQL performance advantage!
   ```
   - ✅ Fix: Denormalize appropriately for read patterns
   ```javascript
   // ✅ GOOD - Embedded documents for read optimization
   {
       _id: "post1",
       author: {  // Embedded user info
           userId: "user1",
           name: "Akshay",
           avatar: "url"
       },
       title: "Learning Node.js",
       content: "...",
       comments: [  // Embedded comments
           {
               userId: "user2",
               userName: "Tejas",  // Denormalized
               text: "Great post!",
               createdAt: ISODate("...")
           }
       ]
   }

   // Single query gets everything!
   // Trade-off: If user changes name, need to update all posts/comments
   // But that's rare compared to reading posts
   ```

3. **Mistake: Not Planning for Scale from the Start**
   - ❌ Problem: Starting with wrong database for expected scale
   ```javascript
   // Startup growing from 1000 to 1M users

   // Initially: SQLite (file-based)
   // 1000 users: Works fine
   // 10,000 users: Getting slow
   // 100,000 users: Database crashes
   // 1M users: Need complete rewrite!

   // Cost: Months of migration work, downtime, risk
   ```
   - ✅ Fix: Choose database based on growth projections
   ```javascript
   // ✅ BETTER - Plan for scale

   // Expected scale: 100K-1M users in 2 years

   // Option 1: PostgreSQL
   // - Can handle millions of users
   // - Vertical scaling (bigger server)
   // - Master-slave replication
   // - Mature, battle-tested

   // Option 2: MongoDB + sharding
   // - Horizontal scaling (more servers)
   // - Flexible schema for rapid iteration
   // - Built for distributed systems

   // Both can scale - choose based on:
   // - Data model (relational vs documents)
   // - Team expertise
   // - Transaction requirements
   ```

4. **Mistake: Ignoring Query Performance and Indexing**
   - ❌ Problem: Not creating indexes on frequently queried fields
   ```javascript
   // ❌ BAD - No index on email field
   // 1 million users in database

   db.users.findOne({ email: "akshay@example.com" });
   // Database scans ALL 1 million documents
   // Takes 10+ seconds!
   ```
   - ✅ Fix: Create appropriate indexes
   ```javascript
   // ✅ GOOD - Index on email
   db.users.createIndex({ email: 1 }, { unique: true });

   db.users.findOne({ email: "akshay@example.com" });
   // Uses index - returns in milliseconds!

   // SQL equivalent:
   CREATE INDEX idx_email ON users(email);

   // Rule: Index fields you query/filter by frequently
   // Trade-off: Indexes speed up reads, slow down writes
   ```

---

### **❓ Interview Questions:**

1. **Q: What's the difference between SQL and NoSQL databases? When would you use each?**
   - **A:**

   **SQL (Relational) Databases:**

   **Structure:**
   - Data in tables (rows and columns)
   - Fixed schema (predefined structure)
   - Relationships via foreign keys
   - Normalized (reduce data duplication)

   **Characteristics:**
   - ACID compliant (strong consistency)
   - Powerful query language (SQL)
   - JOINs for complex queries
   - Vertical scaling (scale up)

   **Examples:** PostgreSQL, MySQL, Oracle, SQL Server

   **Use When:**
   ✅ Complex relationships between data
   ✅ Need ACID transactions (banking, e-commerce orders)
   ✅ Data structure is stable and well-defined
   ✅ Need complex queries with JOINs
   ✅ Data integrity is critical

   **NoSQL Databases:**

   **Structure:**
   - Flexible schemas (or schema-less)
   - Various types: Document, Key-Value, Column-Family, Graph
   - Denormalized (data duplication accepted)

   **Characteristics:**
   - BASE properties (eventual consistency)
   - Horizontal scaling (scale out)
   - Optimized for specific use cases
   - High performance for simple queries

   **Examples:** MongoDB, Redis, Cassandra, DynamoDB

   **Use When:**
   ✅ Flexible/evolving schema
   ✅ Massive scale (millions/billions of records)
   ✅ High write throughput
   ✅ Simple queries (no complex JOINs)
   ✅ Horizontal scaling required

   **Real-World Examples:**

   | Application | Database Choice | Why |
   |-------------|----------------|-----|
   | **Banking** | PostgreSQL (SQL) | ACID transactions mandatory |
   | **Facebook** | MySQL + Cassandra | SQL for user data, NoSQL for feeds |
   | **Instagram** | PostgreSQL | Complex relationships, data integrity |
   | **Netflix** | Cassandra (NoSQL) | Billions of records, high availability |
   | **Twitter** | MySQL + Manhattan | SQL for tweets, NoSQL for timelines |
   | **Uber** | PostgreSQL + Redis | SQL for trips, Redis for real-time |

   **Hybrid Approach (Best Practice):**
   ```
   Use BOTH based on specific needs:
   - PostgreSQL: Users, orders, payments (ACID)
   - MongoDB: Product catalog, reviews (flexible)
   - Redis: Caching, sessions (fast K-V)
   - Elasticsearch: Search (full-text)
   ```

2. **Q: Explain ACID properties with a real-world example.**
   - **A:**

   **ACID Properties:**

   **A - Atomicity** (All or Nothing)
   - Transaction either completes fully or doesn't happen at all
   - No partial updates

   **C - Consistency** (Valid State)
   - Database always in valid state
   - All rules/constraints satisfied

   **I - Isolation** (No Interference)
   - Concurrent transactions don't interfere
   - Each transaction thinks it's alone

   **D - Durability** (Permanent)
   - Once committed, data persists
   - Survives crashes, power failures

   **Real-World Example: Bank Transfer**

   ```sql
   -- Transfer $500 from Account A to Account B

   BEGIN TRANSACTION;

   -- Step 1: Deduct from Account A
   UPDATE accounts
   SET balance = balance - 500
   WHERE account_id = 'A';

   -- Step 2: Add to Account B
   UPDATE accounts
   SET balance = balance + 500
   WHERE account_id = 'B';

   COMMIT;
   ```

   **How ACID Applies:**

   **Atomicity:**
   ```
   Scenario: Power failure after Step 1

   Without Atomicity:
   ❌ Account A debited $500
   ❌ Account B NOT credited
   ❌ $500 disappeared!

   With Atomicity:
   ✅ Transaction rolled back
   ✅ Account A balance restored
   ✅ No money lost
   ```

   **Consistency:**
   ```
   Rule: Total money in system must remain constant

   Before: A = $1000, B = $500, Total = $1500
   After:  A = $500,  B = $1000, Total = $1500

   ✅ Total unchanged (consistent)

   If somehow total changed:
   ❌ Database would reject transaction
   ```

   **Isolation:**
   ```
   Scenario: Two transfers happening simultaneously

   Transaction 1: A → B ($500)
   Transaction 2: C → A ($300)

   Without Isolation:
   ❌ Both read A's balance at same time
   ❌ Both update simultaneously
   ❌ One update lost (race condition)

   With Isolation:
   ✅ Transactions serialize
   ✅ T1 completes, then T2
   ✅ No lost updates
   ```

   **Durability:**
   ```
   Scenario: System crash right after COMMIT

   Without Durability:
   ❌ Transaction might be lost
   ❌ Money transfer reversed

   With Durability:
   ✅ Data written to disk
   ✅ Survives crash
   ✅ Transfer permanent
   ```

   **Why It Matters:**
   - Banking systems MUST be ACID
   - E-commerce orders need ACID
   - Payment systems need ACID
   - Any financial transaction needs ACID

3. **Q: What is the CAP theorem? Explain with examples.**
   - **A:**

   **CAP Theorem:**

   In a distributed database system, you can have at most **2 out of 3**:

   **C - Consistency**
   - All nodes see the same data at the same time
   - Read always returns most recent write

   **A - Availability**
   - Every request gets a response (success or failure)
   - System always operational

   **P - Partition Tolerance**
   - System continues despite network failures
   - Nodes can't communicate but system works

   **Key Insight:**
   ```
   In distributed systems, network partitions WILL happen
   → Partition Tolerance is MANDATORY
   → Real choice is between Consistency and Availability
   ```

   **CP Systems (Consistency + Partition Tolerance):**
   ```
   Sacrifice: Availability
   Behavior: Returns error if can't guarantee consistency

   Example: MongoDB (default), HBase, Redis (single master)

   Scenario:
   - Network partition occurs
   - Nodes can't communicate
   - System refuses writes to prevent inconsistency
   - Returns error instead of potentially wrong data

   Use When:
   ✅ Correctness more important than availability
   ✅ Banking, financial systems
   ✅ Inventory management
   ```

   **AP Systems (Availability + Partition Tolerance):**
   ```
   Sacrifice: Consistency
   Behavior: Always accepts reads/writes, may return stale data

   Example: Cassandra, DynamoDB, Riak

   Scenario:
   - Network partition occurs
   - Each partition accepts writes
   - Data temporarily inconsistent
   - Eventually becomes consistent (minutes/hours)

   Use When:
   ✅ Availability more important than immediate consistency
   ✅ Social media (likes, views)
   ✅ Analytics dashboards
   ✅ Shopping cart (can merge conflicts)
   ```

   **Real-World Example: Social Media Post**

   **CP Approach:**
   ```javascript
   // Instagram-like (CP system)

   User posts photo in New York

   Network partition: NY datacenter can't reach LA

   CP System behavior:
   - NY datacenter accepts post
   - LA datacenter rejects requests (can't guarantee consistency)
   - LA users get error: "Service temporarily unavailable"
   - When partition heals, sync happens, everyone sees post

   Trade-off: Temporary unavailability for consistency
   ```

   **AP Approach:**
   ```javascript
   // Twitter-like (AP system)

   User tweets in New York

   Network partition: NY can't reach LA

   AP System behavior:
   - NY datacenter accepts tweet
   - LA datacenter accepts different tweets
   - LA users see old tweets (before partition)
   - NY users see new tweets
   - Eventually (minutes later), all users see all tweets

   Trade-off: Temporary inconsistency for availability
   ```

   **Which is Better?**
   ```
   It depends on use case!

   Banking: CP (never show wrong balance)
   Social Media: AP (acceptable to see stale likes)
   E-commerce Checkout: CP (prevent overselling)
   E-commerce Browsing: AP (OK if product count slightly off)
   ```

4. **Q: How would you design a database for a social media application?**
   - **A:**

   **Requirements Analysis:**
   ```
   Features:
   - User profiles
   - Posts (text, images, videos)
   - Followers/Following
   - Likes, comments, shares
   - News feed
   - Notifications
   - Search
   - Direct messages

   Scale:
   - 100M users
   - 1B posts
   - 10B likes
   - Billions of read requests/day
   ```

   **Hybrid Database Architecture:**

   **1. PostgreSQL (SQL) - User Management**
   ```sql
   -- User data needs ACID (email, phone must be unique)

   CREATE TABLE users (
       user_id BIGSERIAL PRIMARY KEY,
       username VARCHAR(50) UNIQUE NOT NULL,
       email VARCHAR(100) UNIQUE NOT NULL,
       password_hash VARCHAR(255) NOT NULL,
       created_at TIMESTAMP DEFAULT NOW()
   );

   CREATE TABLE user_profiles (
       user_id BIGINT PRIMARY KEY,
       full_name VARCHAR(100),
       bio TEXT,
       avatar_url VARCHAR(255),
       location VARCHAR(100),
       FOREIGN KEY (user_id) REFERENCES users(user_id)
   );

   -- Followers relationship
   CREATE TABLE followers (
       follower_id BIGINT,
       following_id BIGINT,
       followed_at TIMESTAMP DEFAULT NOW(),
       PRIMARY KEY (follower_id, following_id),
       FOREIGN KEY (follower_id) REFERENCES users(user_id),
       FOREIGN KEY (following_id) REFERENCES users(user_id)
   );

   CREATE INDEX idx_followers_user ON followers(following_id);
   ```

   **2. MongoDB (NoSQL) - Posts & Content**
   ```javascript
   // Flexible schema for different post types

   {
       _id: ObjectId("..."),
       userId: "12345",
       type: "image",  // text, image, video, poll
       content: {
           text: "Check out this photo!",
           images: ["url1.jpg", "url2.jpg"],
           hashtags: ["nodejs", "javascript"]
       },
       metadata: {
           location: { lat: 19.0760, lng: 72.8777 },
           device: "iPhone 13"
       },
       stats: {
           likes: 1523,
           comments: 89,
           shares: 45,
           views: 15234
       },
       createdAt: ISODate("2024-01-15T10:30:00Z")
   }

   // Easy to add new post types without schema changes
   ```

   **3. Redis (Key-Value) - Caching & Real-Time**
   ```javascript
   // Cache hot data
   Key: "user:12345:profile"
   Value: { name: "Akshay", followers: 10000, ... }
   TTL: 3600 seconds

   // User sessions
   Key: "session:abc123"
   Value: { userId: 12345, loggedInAt: ... }
   TTL: 86400 seconds (24 hours)

   // News feed cache (list of post IDs)
   Key: "feed:12345"
   Value: ["post1", "post2", "post3", ...]
   TTL: 300 seconds (5 minutes)

   // Real-time counters
   Key: "post:67890:likes"
   Value: 1523 (increment with INCR command)
   ```

   **4. Cassandra - Activity Feed & Notifications**
   ```javascript
   // Time-series data, billions of records

   // User activity feed (who to show posts to)
   CREATE TABLE user_feed (
       user_id bigint,
       post_id bigint,
       author_id bigint,
       created_at timestamp,
       PRIMARY KEY (user_id, created_at, post_id)
   ) WITH CLUSTERING ORDER BY (created_at DESC);

   // Query: Get latest 50 posts for user's feed
   SELECT * FROM user_feed
   WHERE user_id = 12345
   LIMIT 50;
   ```

   **5. Elasticsearch - Search**
   ```javascript
   // Full-text search on users, posts, hashtags

   {
       "user": "Akshay Saini",
       "bio": "Teaching Node.js and System Design",
       "posts": ["Node.js tutorial", "JavaScript tips"]
   }

   // Fast autocomplete, fuzzy search
   // Search: "akshay node" → finds user and posts
   ```

   **6. S3 (Object Storage) - Media Files**
   ```
   - Images, videos stored in cloud storage
   - CDN for fast delivery worldwide
   - URLs stored in MongoDB posts
   ```

   **Architecture Diagram:**
   ```
   ┌─────────────────────────────────────────┐
   │         API Gateway / Load Balancer     │
   └────────────────┬────────────────────────┘
                    │
        ┌─────��─────┴───────────┐
        │                       │
   ┌────▼─────┐          ┌─────▼──────┐
   │PostgreSQL│          │  MongoDB   │
   │          │          │            │
   │ Users    │          │ Posts      │
   │ Auth     │          │ Comments   │
   │ Relations│          │ Content    │
   └──────────┘          └────────────┘
                    │
        ┌───────────┴───────────┐
        │                       │
   ┌────▼─────┐          ┌─────▼──────┐
   │  Redis   │          │ Cassandra  │
   │          │          │            │
   │ Cache    │          │ Feeds      │
   │ Sessions │          │ Timelines  │
   │ Counters │          │ Activity   │
   └──────────┘          └────────────┘
                    │
              ┌─────▼──────┐
              │Elasticsearch│
              │            │
              │  Search    │
              └────────────┘
   ```

   **Why This Design:**
   - **PostgreSQL**: ACID for critical user data
   - **MongoDB**: Flexible posts, rapid iteration
   - **Redis**: Lightning-fast cache, real-time features
   - **Cassandra**: Massive scale for feeds (billions of rows)
   - **Elasticsearch**: Powerful search capabilities

   Each database doing what it does best! 🚀

---

### **🌐 Real-World Usage:**

**How Top Companies Use Databases:**

1. **Instagram: PostgreSQL**
   - Started with PostgreSQL, still uses it
   - Stores users, photos, relationships
   - Sharded across hundreds of PostgreSQL instances
   - Handles billions of likes/day
   - Why SQL: Complex relationships, data integrity

2. **Facebook: MySQL + TAO + Cassandra**
   - MySQL: Core user data, friend relationships
   - TAO: Caching layer on top of MySQL
   - Cassandra: Messaging, inbox
   - Why: Started with MySQL, invested heavily in scaling it

3. **Netflix: Cassandra**
   - Viewing history (billions of records)
   - Distributed globally for low latency
   - AP system (availability over consistency)
   - Why: Massive scale, eventual consistency OK

4. **Uber: PostgreSQL + Redis + Cassandra**
   - PostgreSQL: Trip data, payments (ACID required)
   - Redis: Real-time driver locations, caching
   - Cassandra: Trip history, analytics
   - Why: Different databases for different needs

5. **Twitter: MySQL + Manhattan (custom)**
   - MySQL: Tweets, users, relationships
   - Manhattan: Distributed key-value store
   - Why: SQL for core data, NoSQL for scale

---

### **⚡ Summary & Quick Revision:**

✅ **SQL (Relational)**: Fixed schema, ACID transactions, complex relationships - use for **transactional data** (banking, e-commerce orders, user authentication)!

✅ **NoSQL**: Flexible schema, horizontal scaling, BASE properties - use for **flexible content** (social media posts, logs, analytics, caching)!

✅ **CAP Theorem**: Can't have all three (Consistency, Availability, Partition Tolerance) - choose **CP for correctness**, **AP for availability**!

✅ **Real-world**: Most companies use **BOTH** SQL and NoSQL - PostgreSQL for critical data, MongoDB for flexible content, Redis for caching, Cassandra for scale! 🚀

---

**🎯 Decision Framework:**

```
Need ACID transactions? → SQL
Need flexible schema? → NoSQL (MongoDB)
Need caching/sessions? → NoSQL (Redis)
Need massive scale (billions)? → NoSQL (Cassandra)
Need full-text search? → Elasticsearch
Need graph relationships? → Neo4j

When in doubt: Start with PostgreSQL (can scale far!)
```

---

**Next Episode Preview:** Episode-13 will dive into **Creating a Database & MongoDB** - hands-on MongoDB setup, CRUD operations, schema design, and building your first database-backed application! 🔥