## Episode-13 | Creating a Database & MongoDB

### **Lecture Title:**
Creating a Database & MongoDB - Hands-On MongoDB Setup, CRUD Operations & Schema Design

---

### **🔥 Core Concept:**
Ab theory se practice mein aate hain! 💻 This episode is pure **hands-on MongoDB** - Akshay bhaiya takes you from installation to building a complete database-backed application. You'll learn how to **set up MongoDB**, perform **CRUD operations** (Create, Read, Update, Delete), design **schemas**, and connect MongoDB with your Node.js applications using both the **native driver** and **Mongoose ODM**.

This is where your Node.js applications come alive with data persistence! You'll understand MongoDB's **document-based structure**, why it's perfect for Node.js (both use JSON), and how to design schemas for real-world applications. By the end, you'll be able to build full-stack applications with MongoDB as your database.

**Why it's important:**
- **MongoDB is the most popular NoSQL database** for Node.js applications
- CRUD operations are fundamental to every backend application
- Understanding MongoDB helps you work with the MERN stack (MongoDB, Express, React, Node.js)
- Common interview topic: "Implement CRUD operations in MongoDB"
- Schema design skills are crucial for scalable applications
- Foundation for building REST APIs, authentication systems, and full-stack apps

---

### **💡 Key Definitions:**

- **MongoDB**: A document-oriented NoSQL database that stores data in flexible, JSON-like documents (BSON format).

- **Database**: A container for collections - one MongoDB server can have multiple databases.

- **Collection**: A group of MongoDB documents - equivalent to a table in SQL databases.

- **Document**: A record in a collection - JSON-like object with key-value pairs (stored as BSON).

- **BSON (Binary JSON)**: Binary representation of JSON-like documents - MongoDB's internal storage format.

- **_id**: Unique identifier automatically created for each document (like primary key in SQL).

- **ObjectId**: MongoDB's 12-byte unique identifier type used for _id fields.

- **CRUD Operations**: Create, Read, Update, Delete - the four basic database operations.

- **MongoDB Driver**: Official Node.js client library for connecting to MongoDB (native driver).

- **Mongoose**: Popular ODM (Object Data Modeling) library for MongoDB and Node.js - provides schema validation, middleware, and more.

- **Schema**: In Mongoose, a blueprint that defines the structure, types, and validation rules for documents.

- **Model**: In Mongoose, a compiled version of a schema - provides an interface to the database.

- **Query**: A request to retrieve or manipulate data from the database.

- **Aggregation**: Pipeline operations for complex data transformations and analytics in MongoDB.

- **Index**: Data structure that improves query performance by allowing fast lookups.

- **Replica Set**: Group of MongoDB instances that maintain the same dataset for high availability.

- **Sharding**: Horizontal partitioning of data across multiple servers for scalability.

---

### **⚙️ Step-by-Step Explanation:**

**MongoDB Fundamentals:**

1. **MongoDB Architecture**
   ```
   MongoDB Server
   ├── Database 1 (e.g., "blogApp")
   │   ├── Collection 1 (e.g., "users")
   │   │   ├── Document 1 { _id, name, email, ... }
   │   │   ├── Document 2 { _id, name, email, ... }
   │   │   └── Document 3 { _id, name, email, ... }
   │   │
   │   ├── Collection 2 (e.g., "posts")
   │   │   ├── Document 1 { _id, title, content, ... }
   │   │   └── Document 2 { _id, title, content, ... }
   │   │
   │   └── Collection 3 (e.g., "comments")
   │
   ├── Database 2 (e.g., "ecommerce")
   │   ├── Collection 1 (e.g., "products")
   │   └── Collection 2 (e.g., "orders")
   │
   └── Database 3 (e.g., "analytics")
   ```

2. **Document Structure (JSON-like)**
   ```javascript
   // A MongoDB document looks like a JavaScript object
   {
       _id: ObjectId("507f1f77bcf86cd799439011"),  // Auto-generated unique ID
       name: "Akshay Saini",
       email: "akshay@example.com",
       age: 30,
       skills: ["JavaScript", "Node.js", "MongoDB"],  // Array
       address: {  // Nested object
           city: "Mumbai",
           country: "India"
       },
       createdAt: ISODate("2024-01-15T10:30:00Z"),
       isActive: true
   }

   // Flexible schema - another document can have different fields
   {
       _id: ObjectId("507f1f77bcf86cd799439012"),
       name: "Tejas",
       email: "tejas@example.com",
       expertise: "Full-Stack",  // Different field
       projects: 15  // Different field
       // No 'age', 'skills', or 'address' - totally fine!
   }
   ```

3. **MongoDB vs SQL Terminology**
   ```
   SQL Database    →    MongoDB
   ──────────────────────────────
   Database        →    Database
   Table           →    Collection
   Row/Record      →    Document
   Column          →    Field
   Primary Key     →    _id (automatic)
   Foreign Key     →    Reference (manual)
   JOIN            →    $lookup (aggregation)
   ```

4. **CRUD Operations Overview**
   ```
   CREATE (Insert):
   - insertOne() - Insert single document
   - insertMany() - Insert multiple documents

   READ (Query):
   - find() - Find multiple documents
   - findOne() - Find single document
   - countDocuments() - Count documents

   UPDATE (Modify):
   - updateOne() - Update single document
   - updateMany() - Update multiple documents
   - replaceOne() - Replace entire document

   DELETE (Remove):
   - deleteOne() - Delete single document
   - deleteMany() - Delete multiple documents
   ```

5. **Connection String Format**
   ```
   mongodb://localhost:27017  (Local MongoDB)

   mongodb+srv://username:password@cluster.mongodb.net/dbname  (MongoDB Atlas - Cloud)

   Components:
   - Protocol: mongodb:// or mongodb+srv://
   - Host: localhost or cluster address
   - Port: 27017 (default for local)
   - Database name: optional in connection string
   ```

---

### **💻 Code Example & Detailed Explanation:**

**Example 1: Installing and Setting Up MongoDB**

**Installation Steps:**

```bash
# macOS (using Homebrew)
brew tap mongodb/brew
brew install mongodb-community
brew services start mongodb-community

# Windows
# Download installer from mongodb.com
# Install and run MongoDB as a service

# Linux (Ubuntu)
sudo apt-get install mongodb
sudo systemctl start mongodb

# Verify installation
mongosh --version
# Or connect to local MongoDB
mongosh
```

**MongoDB Atlas (Cloud - Recommended for Beginners):**

```
1. Go to mongodb.com/cloud/atlas
2. Create free account
3. Create free cluster (M0 - 512MB)
4. Create database user (username + password)
5. Whitelist IP address (0.0.0.0/0 for testing)
6. Get connection string:
   mongodb+srv://username:password@cluster.mongodb.net/
```

---

**Example 2: Basic MongoDB Operations with MongoDB Shell**

```javascript
// Open MongoDB shell
// Command: mongosh

// Show all databases
show dbs

// Create/Switch to database
use blogApp

// Show current database
db

// Create collection and insert document
db.users.insertOne({
    name: "Akshay Saini",
    email: "akshay@example.com",
    age: 30,
    skills: ["JavaScript", "Node.js", "MongoDB"],
    createdAt: new Date()
})

// Output:
// {
//   acknowledged: true,
//   insertedId: ObjectId("507f1f77bcf86cd799439011")
// }

// Insert multiple documents
db.users.insertMany([
    {
        name: "Tejas",
        email: "tejas@example.com",
        age: 25,
        skills: ["React", "Node.js"]
    },
    {
        name: "Priya",
        email: "priya@example.com",
        age: 28,
        skills: ["Python", "Django"]
    }
])

// Find all documents
db.users.find()

// Find with filter
db.users.find({ age: { $gte: 25 } })  // age >= 25

// Find one document
db.users.findOne({ email: "akshay@example.com" })

// Update document
db.users.updateOne(
    { email: "akshay@example.com" },  // Filter
    { $set: { age: 31 } }  // Update
)

// Update multiple
db.users.updateMany(
    { age: { $lt: 30 } },  // age < 30
    { $set: { status: "junior" } }
)

// Delete document
db.users.deleteOne({ email: "tejas@example.com" })

// Delete multiple
db.users.deleteMany({ age: { $lt: 25 } })

// Count documents
db.users.countDocuments()
db.users.countDocuments({ age: { $gte: 30 } })

// Show all collections
show collections

// Drop collection
db.users.drop()
```

---

**Example 3: Connecting to MongoDB from Node.js (Native Driver)**

```javascript
// mongodb-native.js

const { MongoClient, ObjectId } = require('mongodb');

// Connection URL
const url = 'mongodb://localhost:27017';
// const url = 'mongodb+srv://username:password@cluster.mongodb.net/';  // For Atlas

const client = new MongoClient(url);

async function main() {
    try {
        // Connect to MongoDB
        await client.connect();
        console.log("✅ Connected to MongoDB");

        // Get database reference
        const db = client.db('blogApp');

        // Get collection reference
        const usersCollection = db.collection('users');

        // ========================================
        // CREATE - Insert Operations
        // ========================================

        console.log("\n=== INSERT OPERATIONS ===");

        // Insert one document
        const insertResult = await usersCollection.insertOne({
            name: "Akshay Saini",
            email: "akshay@example.com",
            age: 30,
            skills: ["JavaScript", "Node.js", "MongoDB"],
            address: {
                city: "Mumbai",
                country: "India"
            },
            createdAt: new Date(),
            isActive: true
        });

        console.log("Inserted document with _id:", insertResult.insertedId);

        // Insert multiple documents
        const insertManyResult = await usersCollection.insertMany([
            {
                name: "Tejas",
                email: "tejas@example.com",
                age: 25,
                skills: ["React", "Node.js"],
                createdAt: new Date()
            },
            {
                name: "Priya",
                email: "priya@example.com",
                age: 28,
                skills: ["Python", "Django", "PostgreSQL"],
                createdAt: new Date()
            }
        ]);

        console.log("Inserted documents count:", insertManyResult.insertedCount);

        // ========================================
        // READ - Query Operations
        // ========================================

        console.log("\n=== QUERY OPERATIONS ===");

        // Find all documents
        const allUsers = await usersCollection.find().toArray();
        console.log("All users:", allUsers.length);

        // Find with filter
        const jsDevs = await usersCollection.find({
            skills: "JavaScript"  // Array contains "JavaScript"
        }).toArray();
        console.log("JavaScript developers:", jsDevs.length);

        // Find one document
        const user = await usersCollection.findOne({ email: "akshay@example.com" });
        console.log("Found user:", user?.name);

        // Find with operators
        const youngUsers = await usersCollection.find({
            age: { $lt: 30 }  // Less than 30
        }).toArray();
        console.log("Users under 30:", youngUsers.length);

        // Find with projection (select specific fields)
        const names = await usersCollection.find(
            {},
            { projection: { name: 1, email: 1, _id: 0 } }  // Only name and email
        ).toArray();
        console.log("Names only:", names);

        // Find with sorting
        const sortedUsers = await usersCollection.find()
            .sort({ age: -1 })  // -1 for descending, 1 for ascending
            .toArray();
        console.log("Users sorted by age (desc):", sortedUsers.map(u => u.name));

        // Find with limit and skip (pagination)
        const page = await usersCollection.find()
            .skip(0)   // Skip first 0 documents
            .limit(2)  // Take 2 documents
            .toArray();
        console.log("Page 1 (limit 2):", page.length);

        // Count documents
        const count = await usersCollection.countDocuments({ age: { $gte: 25 } });
        console.log("Users 25 or older:", count);

        // ========================================
        // UPDATE - Modification Operations
        // ========================================

        console.log("\n=== UPDATE OPERATIONS ===");

        // Update one document
        const updateResult = await usersCollection.updateOne(
            { email: "akshay@example.com" },  // Filter
            {
                $set: { age: 31 },  // Update age
                $push: { skills: "System Design" }  // Add to array
            }
        );
        console.log("Modified count:", updateResult.modifiedCount);

        // Update multiple documents
        const updateManyResult = await usersCollection.updateMany(
            { age: { $lt: 30 } },
            { $set: { level: "junior" } }
        );
        console.log("Updated multiple documents:", updateManyResult.modifiedCount);

        // Increment a field
        await usersCollection.updateOne(
            { email: "akshay@example.com" },
            { $inc: { loginCount: 1 } }  // Increment by 1 (creates if doesn't exist)
        );

        // Upsert (update or insert if not exists)
        const upsertResult = await usersCollection.updateOne(
            { email: "new@example.com" },
            { $set: { name: "New User", age: 22 } },
            { upsert: true }  // Insert if doesn't exist
        );
        console.log("Upserted:", upsertResult.upsertedId ? "Yes" : "No");

        // ========================================
        // DELETE - Remove Operations
        // ========================================

        console.log("\n=== DELETE OPERATIONS ===");

        // Delete one document
        const deleteResult = await usersCollection.deleteOne({
            email: "new@example.com"
        });
        console.log("Deleted count:", deleteResult.deletedCount);

        // Delete multiple documents
        const deleteManyResult = await usersCollection.deleteMany({
            age: { $lt: 25 }
        });
        console.log("Deleted multiple:", deleteManyResult.deletedCount);

        // ========================================
        // Advanced Queries
        // ========================================

        console.log("\n=== ADVANCED QUERIES ===");

        // Complex query with multiple conditions
        const advanced = await usersCollection.find({
            $and: [
                { age: { $gte: 25 } },
                { age: { $lte: 35 } },
                { skills: "Node.js" }
            ]
        }).toArray();
        console.log("Node.js developers aged 25-35:", advanced.length);

        // Query with OR
        const orQuery = await usersCollection.find({
            $or: [
                { age: { $lt: 25 } },
                { skills: "Python" }
            ]
        }).toArray();
        console.log("Users under 25 OR Python developers:", orQuery.length);

        // Query nested fields
        const mumbaiUsers = await usersCollection.find({
            "address.city": "Mumbai"  // Nested field
        }).toArray();
        console.log("Users in Mumbai:", mumbaiUsers.length);

        // Array queries
        const multiSkilled = await usersCollection.find({
            skills: { $size: 3 }  // Exactly 3 skills
        }).toArray();
        console.log("Users with exactly 3 skills:", multiSkilled.length);

    } catch (err) {
        console.error("❌ Error:", err);
    } finally {
        // Close connection
        await client.close();
        console.log("\n🔌 MongoDB connection closed");
    }
}

main();
```

**Output:**
```
✅ Connected to MongoDB

=== INSERT OPERATIONS ===
Inserted document with _id: 507f1f77bcf86cd799439011
Inserted documents count: 2

=== QUERY OPERATIONS ===
All users: 3
JavaScript developers: 2
Found user: Akshay Saini
Users under 30: 2
Names only: [ { name: 'Akshay Saini', email: 'akshay@example.com' }, ... ]
Users sorted by age (desc): [ 'Akshay Saini', 'Priya', 'Tejas' ]
Page 1 (limit 2): 2
Users 25 or older: 3

=== UPDATE OPERATIONS ===
Modified count: 1
Updated multiple documents: 2
Upserted: Yes

=== DELETE OPERATIONS ===
Deleted count: 1
Deleted multiple: 0

=== ADVANCED QUERIES ===
Node.js developers aged 25-35: 2
Users under 25 OR Python developers: 1
Users in Mumbai: 1
Users with exactly 3 skills: 1

🔌 MongoDB connection closed
```

**📝 Key MongoDB Query Operators:**

```javascript
// Comparison Operators
$eq  - Equal to
$ne  - Not equal to
$gt  - Greater than
$gte - Greater than or equal
$lt  - Less than
$lte - Less than or equal
$in  - In array
$nin - Not in array

// Logical Operators
$and - AND condition
$or  - OR condition
$not - NOT condition
$nor - NOR condition

// Array Operators
$size      - Array size
$all       - Contains all elements
$elemMatch - Element matches condition

// Update Operators
$set   - Set field value
$unset - Remove field
$inc   - Increment number
$push  - Add to array
$pull  - Remove from array
$pop   - Remove first/last from array
```

---

**Example 4: Using Mongoose ODM (Object Data Modeling)**

```javascript
// mongoose-example.js

const mongoose = require('mongoose');

// Connect to MongoDB
mongoose.connect('mongodb://localhost:27017/blogApp', {
    // useNewUrlParser: true,  // These options are now default
    // useUnifiedTopology: true
});

// Connection events
mongoose.connection.on('connected', () => {
    console.log('✅ Mongoose connected to MongoDB');
});

mongoose.connection.on('error', (err) => {
    console.error('❌ Mongoose connection error:', err);
});

// ========================================
// Define Schema (Blueprint for documents)
// ========================================

const userSchema = new mongoose.Schema({
    name: {
        type: String,
        required: [true, 'Name is required'],
        trim: true,  // Remove whitespace
        minlength: [2, 'Name must be at least 2 characters'],
        maxlength: [50, 'Name cannot exceed 50 characters']
    },
    email: {
        type: String,
        required: true,
        unique: true,  // Create unique index
        lowercase: true,  // Convert to lowercase
        match: [/^\S+@\S+\.\S+$/, 'Please enter a valid email']
    },
    age: {
        type: Number,
        min: [0, 'Age cannot be negative'],
        max: [150, 'Age cannot exceed 150']
    },
    skills: {
        type: [String],  // Array of strings
        default: []
    },
    address: {
        street: String,
        city: String,
        country: String,
        pincode: String
    },
    isActive: {
        type: Boolean,
        default: true
    },
    loginCount: {
        type: Number,
        default: 0
    },
    createdAt: {
        type: Date,
        default: Date.now  // Auto-set current date
    },
    updatedAt: {
        type: Date,
        default: Date.now
    }
}, {
    timestamps: true  // Auto-manage createdAt and updatedAt
});

// ========================================
// Schema Methods (Instance methods)
// ========================================

userSchema.methods.getFullInfo = function() {
    return `${this.name} (${this.email}) - ${this.skills.join(', ')}`;
};

userSchema.methods.incrementLoginCount = async function() {
    this.loginCount += 1;
    return await this.save();
};

// ========================================
// Schema Statics (Model methods)
// ========================================

userSchema.statics.findByEmail = function(email) {
    return this.findOne({ email: email.toLowerCase() });
};

userSchema.statics.findActiveUsers = function() {
    return this.find({ isActive: true });
};

// ========================================
// Middleware (Hooks)
// ========================================

// Pre-save hook (runs before saving)
userSchema.pre('save', function(next) {
    console.log(`About to save user: ${this.name}`);
    this.updatedAt = Date.now();
    next();
});

// Post-save hook (runs after saving)
userSchema.post('save', function(doc) {
    console.log(`User saved successfully: ${doc._id}`);
});

// ========================================
// Create Model (compiled schema)
// ========================================

const User = mongoose.model('User', userSchema);

// ========================================
// CRUD Operations with Mongoose
// ========================================

async function mongooseExamples() {
    try {
        // ========================================
        // CREATE
        // ========================================

        console.log("\n=== CREATE OPERATIONS ===");

        // Create single document
        const user1 = new User({
            name: "Akshay Saini",
            email: "akshay@example.com",
            age: 30,
            skills: ["JavaScript", "Node.js", "MongoDB"],
            address: {
                city: "Mumbai",
                country: "India"
            }
        });

        await user1.save();
        console.log("User created:", user1._id);

        // Create using create() method (shorthand)
        const user2 = await User.create({
            name: "Tejas",
            email: "tejas@example.com",
            age: 25,
            skills: ["React", "Node.js"]
        });
        console.log("User created:", user2._id);

        // Create multiple documents
        const users = await User.insertMany([
            { name: "Priya", email: "priya@example.com", age: 28 },
            { name: "Rahul", email: "rahul@example.com", age: 32 }
        ]);
        console.log("Created users:", users.length);

        // ========================================
        // READ
        // ========================================

        console.log("\n=== READ OPERATIONS ===");

        // Find all
        const allUsers = await User.find();
        console.log("All users:", allUsers.length);

        // Find with filter
        const jsDevs = await User.find({ skills: "JavaScript" });
        console.log("JavaScript developers:", jsDevs.length);

        // Find one
        const foundUser = await User.findOne({ email: "akshay@example.com" });
        console.log("Found user:", foundUser?.name);

        // Find by ID
        const userById = await User.findById(user1._id);
        console.log("User by ID:", userById?.name);

        // Find with select (projection)
        const names = await User.find().select('name email -_id');
        console.log("Names:", names.map(u => u.name));

        // Find with sort
        const sorted = await User.find().sort({ age: -1 });  // Descending
        console.log("Sorted by age:", sorted.map(u => `${u.name}(${u.age})`));

        // Find with limit and skip
        const page1 = await User.find().limit(2).skip(0);
        console.log("Page 1:", page1.length);

        // Count
        const count = await User.countDocuments({ age: { $gte: 25 } });
        console.log("Users 25+:", count);

        // Using custom static method
        const activeUsers = await User.findActiveUsers();
        console.log("Active users:", activeUsers.length);

        // Using custom instance method
        const info = foundUser?.getFullInfo();
        console.log("User info:", info);

        // ========================================
        // UPDATE
        // ========================================

        console.log("\n=== UPDATE OPERATIONS ===");

        // Update one
        const updateResult = await User.updateOne(
            { email: "akshay@example.com" },
            { $set: { age: 31 }, $push: { skills: "System Design" } }
        );
        console.log("Modified:", updateResult.modifiedCount);

        // Find and update (returns updated document)
        const updated = await User.findOneAndUpdate(
            { email: "tejas@example.com" },
            { $set: { age: 26 } },
            { new: true }  // Return updated document
        );
        console.log("Updated user:", updated?.name, updated?.age);

        // Update by ID
        const updatedById = await User.findByIdAndUpdate(
            user1._id,
            { $inc: { loginCount: 1 } },
            { new: true }
        );
        console.log("Login count:", updatedById?.loginCount);

        // Using instance method
        await foundUser?.incrementLoginCount();
        console.log("Incremented login count");

        // ========================================
        // DELETE
        // ========================================

        console.log("\n=== DELETE OPERATIONS ===");

        // Delete one
        const deleteResult = await User.deleteOne({ email: "rahul@example.com" });
        console.log("Deleted:", deleteResult.deletedCount);

        // Find and delete (returns deleted document)
        const deleted = await User.findOneAndDelete({ email: "priya@example.com" });
        console.log("Deleted user:", deleted?.name);

        // Delete by ID
        const deletedById = await User.findByIdAndDelete(user2._id);
        console.log("Deleted by ID:", deletedById?.name);

        // ========================================
        // Validation
        // ========================================

        console.log("\n=== VALIDATION ===");

        try {
            // Missing required field
            await User.create({ age: 25 });  // No name or email
        } catch (err) {
            console.log("Validation error:", err.message);
        }

        try {
            // Invalid email format
            await User.create({
                name: "Test",
                email: "invalid-email"
            });
        } catch (err) {
            console.log("Email validation error:", err.message);
        }

    } catch (err) {
        console.error("Error:", err);
    } finally {
        await mongoose.connection.close();
        console.log("\n🔌 Mongoose connection closed");
    }
}

mongooseExamples();
```

**Output:**
```
✅ Mongoose connected to MongoDB

=== CREATE OPERATIONS ===
About to save user: Akshay Saini
User saved successfully: 507f1f77bcf86cd799439011
User created: 507f1f77bcf86cd799439011
User created: 507f1f77bcf86cd799439012
Created users: 2

=== READ OPERATIONS ===
All users: 4
JavaScript developers: 1
Found user: Akshay Saini
User by ID: Akshay Saini
Names: [ 'Akshay Saini', 'Tejas', 'Priya', 'Rahul' ]
Sorted by age: [ 'Rahul(32)', 'Akshay Saini(30)', 'Priya(28)', 'Tejas(25)' ]
Page 1: 2
Users 25+: 4
Active users: 4
User info: Akshay Saini (akshay@example.com) - JavaScript, Node.js, MongoDB

=== UPDATE OPERATIONS ===
Modified: 1
Updated user: Tejas 26
Login count: 1
Incremented login count

=== DELETE OPERATIONS ===
Deleted: 1
Deleted user: Priya
Deleted by ID: Tejas

=== VALIDATION ===
Validation error: User validation failed: name: Name is required, email: Path `email` is required.
Email validation error: User validation failed: email: Please enter a valid email

🔌 Mongoose connection closed
```

**📝 Mongoose Advantages:**

```javascript
// ✅ Schema Validation
// - Type checking
// - Required fields
// - Custom validators
// - Default values

// ✅ Middleware (Hooks)
// - Pre/post save
// - Pre/post update
// - Pre/post remove

// ✅ Methods
// - Instance methods (document level)
// - Static methods (model level)

// ✅ Virtuals
// - Computed properties
// - Not stored in DB

// ✅ Population
// - Like SQL JOINs
// - Reference other documents

// ✅ Plugins
// - Reusable schema enhancements
```

---

**Example 5: MongoDB Relationships & Population**

```javascript
// relationships-mongoose.js

const mongoose = require('mongoose');

mongoose.connect('mongodb://localhost:27017/blogApp');

// ========================================
// User Schema
// ========================================

const userSchema = new mongoose.Schema({
    name: { type: String, required: true },
    email: { type: String, required: true, unique: true }
});

const User = mongoose.model('User', userSchema);

// ========================================
// Post Schema (with reference to User)
// ========================================

const postSchema = new mongoose.Schema({
    title: { type: String, required: true },
    content: { type: String, required: true },
    author: {
        type: mongoose.Schema.Types.ObjectId,  // Reference to User
        ref: 'User',  // Model name to reference
        required: true
    },
    tags: [String],
    likes: { type: Number, default: 0 },
    comments: [{  // Embedded subdocuments
        user: {
            type: mongoose.Schema.Types.ObjectId,
            ref: 'User'
        },
        text: String,
        createdAt: { type: Date, default: Date.now }
    }],
    createdAt: { type: Date, default: Date.now }
});

const Post = mongoose.model('Post', postSchema);

// ========================================
// Examples
// ========================================

async function relationshipExamples() {
    try {
        // Create user
        const user = await User.create({
            name: "Akshay Saini",
            email: "akshay@example.com"
        });

        // Create post with user reference
        const post = await Post.create({
            title: "Learning MongoDB",
            content: "MongoDB is a great NoSQL database...",
            author: user._id,  // Store user's ObjectId
            tags: ["mongodb", "database", "nosql"]
        });

        console.log("Post created:", post._id);

        // ========================================
        // Population (like SQL JOIN)
        // ========================================

        // Find post and populate author
        const postWithAuthor = await Post.findById(post._id)
            .populate('author');  // Replace ObjectId with actual user document

        console.log("Post author:", postWithAuthor.author.name);

        // Populate with specific fields
        const postWithAuthorName = await Post.findById(post._id)
            .populate('author', 'name email -_id');  // Only name and email

        console.log("Author:", postWithAuthorName.author);

        // Add comment to post
        await Post.findByIdAndUpdate(post._id, {
            $push: {
                comments: {
                    user: user._id,
                    text: "Great post!"
                }
            }
        });

        // Populate nested references
        const postWithComments = await Post.findById(post._id)
            .populate('author')
            .populate('comments.user');  // Populate user in each comment

        console.log("Comments:", postWithComments.comments.length);

        // ========================================
        // Aggregation Pipeline
        // ========================================

        const stats = await Post.aggregate([
            // Stage 1: Match posts with specific tag
            {
                $match: { tags: "mongodb" }
            },
            // Stage 2: Lookup (JOIN) with users collection
            {
                $lookup: {
                    from: 'users',
                    localField: 'author',
                    foreignField: '_id',
                    as: 'authorInfo'
                }
            },
            // Stage 3: Unwind array
            {
                $unwind: '$authorInfo'
            },
            // Stage 4: Project (select fields)
            {
                $project: {
                    title: 1,
                    authorName: '$authorInfo.name',
                    commentsCount: { $size: '$comments' },
                    likes: 1
                }
            }
        ]);

        console.log("Aggregation result:", stats);

    } catch (err) {
        console.error("Error:", err);
    } finally {
        await mongoose.connection.close();
    }
}

relationshipExamples();
```

---

### **❌ Common Mistakes & Fixes:**

1. **Mistake: Not Handling Connection Errors**
   - ❌ Problem: Assuming connection always succeeds
   ```javascript
   // ❌ BAD - No error handling
   const { MongoClient } = require('mongodb');
   const client = new MongoClient(url);

   await client.connect();  // What if this fails?
   const db = client.db('mydb');
   // App crashes if connection fails!
   ```
   - ✅ Fix: Always handle connection errors
   ```javascript
   // ✅ GOOD - Proper error handling
   const { MongoClient } = require('mongodb');
   const client = new MongoClient(url);

   try {
       await client.connect();
       console.log("✅ Connected to MongoDB");

       const db = client.db('mydb');
       // Use database...

   } catch (err) {
       console.error("❌ Failed to connect to MongoDB:", err);
       process.exit(1);  // Exit if database not available
   } finally {
       await client.close();
   }

   // Or with Mongoose
   mongoose.connection.on('error', (err) => {
       console.error('MongoDB connection error:', err);
       process.exit(1);
   });
   ```

2. **Mistake: Not Closing Database Connections**
   - ❌ Problem: Leaving connections open
   ```javascript
   // ❌ BAD - Connection never closed
   async function getData() {
       const client = new MongoClient(url);
       await client.connect();
       const db = client.db('mydb');
       const data = await db.collection('users').find().toArray();
       return data;  // Connection still open!
   }

   // Call this 1000 times → 1000 open connections!
   ```
   - ✅ Fix: Always close connections
   ```javascript
   // ✅ GOOD - Connection properly managed
   async function getData() {
       const client = new MongoClient(url);

       try {
           await client.connect();
           const db = client.db('mydb');
           const data = await db.collection('users').find().toArray();
           return data;
       } finally {
           await client.close();  // Always close
       }
   }

   // Or better: Use connection pooling
   // Create client once, reuse for all operations
   const client = new MongoClient(url);
   await client.connect();  // Connect once

   // Reuse client for all operations
   async function getData() {
       const db = client.db('mydb');
       return await db.collection('users').find().toArray();
   }

   // Close when app shuts down
   process.on('SIGINT', async () => {
       await client.close();
       process.exit(0);
   });
   ```

3. **Mistake: Not Using Indexes for Queries**
   - ❌ Problem: Slow queries on large collections
   ```javascript
   // ❌ BAD - No index on email field
   // 1 million users in database

   const user = await User.findOne({ email: "akshay@example.com" });
   // Scans ALL 1 million documents (collection scan)
   // Takes 10+ seconds!
   ```
   - ✅ Fix: Create indexes on frequently queried fields
   ```javascript
   // ✅ GOOD - Create index

   // Using Mongoose
   const userSchema = new mongoose.Schema({
       name: String,
       email: {
           type: String,
           unique: true,  // Creates unique index automatically
           index: true    // Or regular index
       },
       age: Number
   });

   // Compound index (multiple fields)
   userSchema.index({ age: 1, name: 1 });

   // Using native driver
   await collection.createIndex({ email: 1 }, { unique: true });
   await collection.createIndex({ age: 1, name: 1 });

   // Now query is fast!
   const user = await User.findOne({ email: "akshay@example.com" });
   // Uses index - returns in milliseconds!

   // Check if query uses index
   const explain = await User.find({ email: "test@example.com" }).explain();
   console.log(explain.executionStats.executionTimeMillis);
   ```

4. **Mistake: Storing Sensitive Data Without Encryption**
   - ❌ Problem: Storing passwords in plain text
   ```javascript
   // ❌ TERRIBLE - Plain text password!
   await User.create({
       email: "user@example.com",
       password: "mypassword123"  // Visible in database!
   });

   // If database is compromised, all passwords exposed!
   ```
   - ✅ Fix: Hash passwords before storing
   ```javascript
   // ✅ GOOD - Hash password with bcrypt
   const bcrypt = require('bcrypt');

   const userSchema = new mongoose.Schema({
       email: String,
       password: String
   });

   // Hash password before saving
   userSchema.pre('save', async function(next) {
       if (this.isModified('password')) {
           this.password = await bcrypt.hash(this.password, 10);
       }
       next();
   });

   // Method to verify password
   userSchema.methods.comparePassword = function(candidatePassword) {
       return bcrypt.compare(candidatePassword, this.password);
   };

   const User = mongoose.model('User', userSchema);

   // Create user
   const user = await User.create({
       email: "user@example.com",
       password: "mypassword123"  // Will be hashed automatically
   });
   // Stored in DB: $2b$10$eKjHXz... (hashed)

   // Verify password
   const isMatch = await user.comparePassword("mypassword123");
   console.log("Password correct:", isMatch);  // true
   ```

5. **Mistake: Not Validating Data Before Insertion**
   - ❌ Problem: Inserting invalid or malicious data
   ```javascript
   // ❌ BAD - No validation
   app.post('/users', async (req, res) => {
       const user = await User.create(req.body);  // Directly from request!
       // What if req.body contains malicious data?
       // What if required fields are missing?
       res.json(user);
   });
   ```
   - ✅ Fix: Always validate data
   ```javascript
   // ✅ GOOD - Mongoose schema validation
   const userSchema = new mongoose.Schema({
       name: {
           type: String,
           required: [true, 'Name is required'],
           trim: true,
           minlength: 2,
           maxlength: 50
       },
       email: {
           type: String,
           required: true,
           unique: true,
           match: [/^\S+@\S+\.\S+$/, 'Invalid email format']
       },
       age: {
           type: Number,
           min: 0,
           max: 150
       }
   });

   app.post('/users', async (req, res) => {
       try {
           // Only allow specific fields
           const { name, email, age } = req.body;

           const user = await User.create({ name, email, age });
           res.json(user);
       } catch (err) {
           if (err.name === 'ValidationError') {
               res.status(400).json({ error: err.message });
           } else {
               res.status(500).json({ error: 'Server error' });
           }
       }
   });

   // Or use validation library like Joi/Yup
   const Joi = require('joi');

   const userSchema = Joi.object({
       name: Joi.string().min(2).max(50).required(),
       email: Joi.string().email().required(),
       age: Joi.number().min(0).max(150)
   });

   app.post('/users', async (req, res) => {
       const { error, value } = userSchema.validate(req.body);

       if (error) {
           return res.status(400).json({ error: error.details[0].message });
       }

       const user = await User.create(value);
       res.json(user);
   });
   ```

---

### **❓ Interview Questions:**

1. **Q: Explain CRUD operations in MongoDB with examples.**
   - **A:**

   **CRUD = Create, Read, Update, Delete**

   **CREATE (Insert):**
   ```javascript
   // Insert one document
   const result = await collection.insertOne({
       name: "Akshay",
       email: "akshay@example.com"
   });
   console.log("Inserted ID:", result.insertedId);

   // Insert multiple documents
   await collection.insertMany([
       { name: "User1", email: "user1@example.com" },
       { name: "User2", email: "user2@example.com" }
   ]);

   // With Mongoose
   const user = await User.create({ name: "Akshay", email: "..." });
   ```

   **READ (Query):**
   ```javascript
   // Find all
   const users = await collection.find().toArray();

   // Find with filter
   const user = await collection.findOne({ email: "akshay@example.com" });

   // Find with conditions
   const adults = await collection.find({
       age: { $gte: 18 }  // age >= 18
   }).toArray();

   // With projection (select fields)
   const names = await collection.find(
       {},
       { projection: { name: 1, _id: 0 } }
   ).toArray();

   // With Mongoose
   const users = await User.find({ age: { $gte: 18 } })
       .select('name email')
       .sort({ age: -1 })
       .limit(10);
   ```

   **UPDATE (Modify):**
   ```javascript
   // Update one
   await collection.updateOne(
       { email: "akshay@example.com" },  // Filter
       { $set: { age: 31 } }  // Update
   );

   // Update multiple
   await collection.updateMany(
       { age: { $lt: 18 } },
       { $set: { category: "minor" } }
   );

   // Update operators
   await collection.updateOne(
       { email: "akshay@example.com" },
       {
           $set: { age: 31 },           // Set field
           $inc: { loginCount: 1 },     // Increment
           $push: { skills: "MongoDB" }, // Add to array
           $unset: { tempField: "" }    // Remove field
       }
   );

   // With Mongoose (returns updated doc)
   const updated = await User.findOneAndUpdate(
       { email: "akshay@example.com" },
       { $set: { age: 31 } },
       { new: true }  // Return updated document
   );
   ```

   **DELETE (Remove):**
   ```javascript
   // Delete one
   await collection.deleteOne({ email: "temp@example.com" });

   // Delete multiple
   await collection.deleteMany({ age: { $lt: 18 } });

   // With Mongoose (returns deleted doc)
   const deleted = await User.findOneAndDelete({
       email: "temp@example.com"
   });
   ```

2. **Q: What's the difference between MongoDB native driver and Mongoose?**
   - **A:**

   | Feature | MongoDB Native Driver | Mongoose |
   |---------|----------------------|----------|
   | **Type** | Official driver | ODM (Object Data Modeling) library |
   | **Schema** | No schema | Schema required |
   | **Validation** | Manual | Built-in schema validation |
   | **Learning Curve** | Lower | Higher |
   | **Flexibility** | More flexible | More structure |
   | **Code** | More verbose | More concise |
   | **Features** | Basic operations | Middleware, methods, virtuals, plugins |

   **Native Driver:**
   ```javascript
   // More manual, more flexible
   const { MongoClient } = require('mongodb');
   const client = new MongoClient(url);

   await client.connect();
   const db = client.db('mydb');
   const collection = db.collection('users');

   // No schema - can insert anything
   await collection.insertOne({
       name: "Akshay",
       randomField: "anything"  // Allowed
   });

   // Manual validation
   if (!email || !name) {
       throw new Error("Validation failed");
   }
   ```

   **Mongoose:**
   ```javascript
   // More structure, less manual work
   const mongoose = require('mongoose');

   // Define schema
   const userSchema = new mongoose.Schema({
       name: { type: String, required: true },
       email: { type: String, required: true, unique: true }
   });

   // Add methods
   userSchema.methods.getInfo = function() {
       return `${this.name} (${this.email})`;
   };

   const User = mongoose.model('User', userSchema);

   // Auto-validation
   try {
       await User.create({ name: "Akshay" });  // Missing email
   } catch (err) {
       console.log("Validation failed:", err.message);
   }
   ```

   **When to use Native Driver:**
   - ✅ Flexible schema needed
   - ✅ Performance critical (slight overhead in Mongoose)
   - ✅ Simple use cases
   - ✅ Want full control

   **When to use Mongoose:**
   - ✅ Need schema validation
   - ✅ Want middleware (hooks)
   - ✅ Complex relationships
   - ✅ Team prefers structure
   - ✅ Rapid development

3. **Q: How do you handle relationships in MongoDB?**
   - **A:**

   **Two Main Approaches:**

   **1. Embedding (Denormalization)**
   ```javascript
   // Store related data within same document

   {
       _id: ObjectId("..."),
       title: "Learning Node.js",
       author: {  // Embedded author info
           name: "Akshay Saini",
           email: "akshay@example.com"
       },
       comments: [  // Embedded comments
           {
               user: "Tejas",
               text: "Great post!",
               date: ISODate("...")
           },
           {
               user: "Priya",
               text: "Very helpful",
               date: ISODate("...")
           }
       ]
   }

   // Advantages:
   ✅ Fast reads (single query gets everything)
   ✅ Atomic updates (update entire document)

   // Disadvantages:
   ❌ Data duplication
   ❌ Document size limit (16MB)
   ❌ If author email changes, must update all posts

   // Use When:
   - Data is read together frequently
   - Relationship is one-to-few
   - Data doesn't change often
   ```

   **2. Referencing (Normalization)**
   ```javascript
   // Store references (like SQL foreign keys)

   // Users collection
   {
       _id: ObjectId("507f1f77bcf86cd799439011"),
       name: "Akshay Saini",
       email: "akshay@example.com"
   }

   // Posts collection
   {
       _id: ObjectId("507f1f77bcf86cd799439012"),
       title: "Learning Node.js",
       authorId: ObjectId("507f1f77bcf86cd799439011"),  // Reference
       content: "..."
   }

   // Query with populate (Mongoose)
   const post = await Post.findById(postId)
       .populate('authorId');  // Fetches author document

   // Or manual lookup (native driver)
   const post = await posts.findOne({ _id: postId });
   const author = await users.findOne({ _id: post.authorId });

   // Advantages:
   ✅ No data duplication
   ✅ Easy to update (author email changes once)
   ✅ Smaller documents

   // Disadvantages:
   ❌ Slower reads (multiple queries or $lookup)
   ❌ No foreign key constraints (manual integrity)

   // Use When:
   - Relationship is one-to-many or many-to-many
   - Data changes frequently
   - Need to query related data independently
   ```

   **Hybrid Approach (Best Practice):**
   ```javascript
   // Store both reference and frequently-used fields

   {
       _id: ObjectId("..."),
       title: "Learning Node.js",
       authorId: ObjectId("..."),  // Reference for lookups
       authorName: "Akshay Saini",  // Denormalized for display
       authorAvatar: "url...",      // Denormalized for display
       content: "...",
       comments: [  // Embedded (one-to-few)
           { text: "...", userId: ObjectId("..."), userName: "Tejas" }
       ]
   }

   // Benefits:
   ✅ Fast reads (no need to populate for display)
   ✅ Can still populate for full author info
   ✅ Balance between performance and data integrity
   ```

4. **Q: How would you design a schema for a social media application in MongoDB?**
   - **A:**

   **Schema Design for Social Media App:**

   **1. Users Collection:**
   ```javascript
   const userSchema = new mongoose.Schema({
       username: {
           type: String,
           required: true,
           unique: true,
           lowercase: true,
           trim: true
       },
       email: {
           type: String,
           required: true,
           unique: true,
           lowercase: true
       },
       password: String,  // Hashed
       profile: {
           fullName: String,
           bio: String,
           avatar: String,
           location: String,
           website: String
       },
       stats: {
           followers: { type: Number, default: 0 },
           following: { type: Number, default: 0 },
           posts: { type: Number, default: 0 }
       },
       verified: { type: Boolean, default: false },
       private: { type: Boolean, default: false },
       createdAt: { type: Date, default: Date.now }
   });

   // Indexes
   userSchema.index({ username: 1 });
   userSchema.index({ email: 1 });
   ```

   **2. Posts Collection:**
   ```javascript
   const postSchema = new mongoose.Schema({
       userId: {
           type: mongoose.Schema.Types.ObjectId,
           ref: 'User',
           required: true
       },
       // Denormalized for performance
       username: String,
       userAvatar: String,

       type: {
           type: String,
           enum: ['photo', 'video', 'text'],
           required: true
       },
       caption: String,
       media: [{
           url: String,
           type: String,  // image/video
           thumbnail: String
       }],
       hashtags: [String],
       mentions: [String],
       location: {
           name: String,
           coordinates: {
               lat: Number,
               lng: Number
           }
       },
       stats: {
           likes: { type: Number, default: 0 },
           comments: { type: Number, default: 0 },
           shares: { type: Number, default: 0 },
           views: { type: Number, default: 0 }
       },
       createdAt: { type: Date, default: Date.now }
   });

   // Indexes
   postSchema.index({ userId: 1, createdAt: -1 });
   postSchema.index({ hashtags: 1 });
   postSchema.index({ createdAt: -1 });
   ```

   **3. Followers Collection (for relationships):**
   ```javascript
   const followerSchema = new mongoose.Schema({
       userId: {
           type: mongoose.Schema.Types.ObjectId,
           ref: 'User',
           required: true
       },
       followerId: {
           type: mongoose.Schema.Types.ObjectId,
           ref: 'User',
           required: true
       },
       createdAt: { type: Date, default: Date.now }
   });

   // Compound index for unique constraint and queries
   followerSchema.index({ userId: 1, followerId: 1 }, { unique: true });
   followerSchema.index({ followerId: 1, userId: 1 });
   ```

   **4. Likes Collection:**
   ```javascript
   const likeSchema = new mongoose.Schema({
       postId: {
           type: mongoose.Schema.Types.ObjectId,
           ref: 'Post',
           required: true
       },
       userId: {
           type: mongoose.Schema.Types.ObjectId,
           ref: 'User',
           required: true
       },
       createdAt: { type: Date, default: Date.now }
   });

   // Ensure user can't like same post twice
   likeSchema.index({ postId: 1, userId: 1 }, { unique: true });
   ```

   **5. Comments Collection:**
   ```javascript
   const commentSchema = new mongoose.Schema({
       postId: {
           type: mongoose.Schema.Types.ObjectId,
           ref: 'Post',
           required: true
       },
       userId: {
           type: mongoose.Schema.Types.ObjectId,
           ref: 'User',
           required: true
       },
       // Denormalized
       username: String,
       userAvatar: String,

       text: { type: String, required: true },
       likes: { type: Number, default: 0 },
       replies: [{
           userId: mongoose.Schema.Types.ObjectId,
           username: String,
           text: String,
           createdAt: Date
       }],
       createdAt: { type: Date, default: Date.now }
   });

   commentSchema.index({ postId: 1, createdAt: -1 });
   ```

   **Design Decisions:**

   ```
   ✅ Denormalization where needed:
   - Store username/avatar in posts for fast display
   - No need to populate user for every post

   ✅ Separate collections for relationships:
   - Followers, Likes (many-to-many)
   - Easier to query and manage

   ✅ Embedded for one-to-few:
   - Comment replies (limited number)

   ✅ Stats denormalized:
   - Follower count, like count stored in documents
   - Updated with increment operations
   - Faster than counting documents

   ✅ Proper indexing:
   - Compound indexes for common queries
   - Unique indexes for constraints
   ```

---

### **🌐 Real-World Usage:**

**How MongoDB is Used in Production:**

1. **Content Management Systems**
   ```javascript
   // Blog/CMS - flexible content structure
   const articleSchema = new mongoose.Schema({
       title: String,
       slug: String,
       content: String,  // Or rich text/markdown
       author: { type: ObjectId, ref: 'User' },
       category: String,
       tags: [String],
       metadata: {
           seo: {
               title: String,
               description: String,
               keywords: [String]
           },
           social: {
               ogImage: String,
               twitterCard: String
           }
       },
       status: { type: String, enum: ['draft', 'published'] },
       publishedAt: Date
   });
   ```

2. **E-Commerce Product Catalogs**
   ```javascript
   // Products with varying attributes
   const productSchema = new mongoose.Schema({
       name: String,
       sku: { type: String, unique: true },
       category: String,
       price: Number,
       images: [String],
       attributes: Schema.Types.Mixed,  // Flexible attributes
       // Different products have different attributes:
       // Laptop: { processor, ram, storage }
       // Shirt: { size, color, material }
       inventory: Number,
       reviews: [{
           user: ObjectId,
           rating: Number,
           comment: String
       }]
   });
   ```

3. **Real-Time Analytics/Logging**
   ```javascript
   // High-volume time-series data
   const logSchema = new mongoose.Schema({
       timestamp: { type: Date, default: Date.now },
       level: String,  // info, warn, error
       service: String,
       message: String,
       metadata: Schema.Types.Mixed,
       userId: ObjectId
   });

   // TTL index - auto-delete old logs
   logSchema.index({ timestamp: 1 }, { expireAfterSeconds: 2592000 });  // 30 days
   ```

4. **User Sessions**
   ```javascript
   // Session store for Express
   const session = require('express-session');
   const MongoStore = require('connect-mongo');

   app.use(session({
       secret: 'your-secret',
       store: MongoStore.create({
           mongoUrl: 'mongodb://localhost/sessions'
       }),
       cookie: { maxAge: 1000 * 60 * 60 * 24 }  // 1 day
   }));
   ```

5. **Mobile App Backend**
   ```javascript
   // Flexible schema for rapid iteration
   // Easy to add features without migrations
   const userSchema = new mongoose.Schema({
       // Core fields
       email: String,
       phone: String,

       // New feature added? Just add field
       pushToken: String,  // Added for push notifications
       preferences: {       // Added for settings
           language: String,
           theme: String
       }
       // No need to migrate existing documents!
   });
   ```

---

### **⚡ Summary & Quick Revision:**

✅ **MongoDB stores data as documents** (JSON-like) in collections - flexible schema allows different documents to have different fields!

✅ **CRUD operations**: insertOne/Many (create), find/findOne (read), updateOne/Many (update), deleteOne/Many (delete) - core database operations!

✅ **Mongoose adds structure** - schema validation, middleware, methods, virtuals - makes MongoDB safer and easier to work with!

✅ **Schema design**: Embed for one-to-few relationships (comments in posts), reference for one-to-many (posts by user) - hybrid approach often best! 🚀

---

**🎯 Quick Setup Checklist:**
```bash
# 1. Install MongoDB
brew install mongodb-community  # macOS
# Or use MongoDB Atlas (cloud)

# 2. Install packages
npm install mongodb     # Native driver
npm install mongoose    # ODM

# 3. Start MongoDB
brew services start mongodb-community

# 4. Connect from Node.js
const mongoose = require('mongoose');
mongoose.connect('mongodb://localhost:27017/mydb');
```

---

**Next Steps:**
- Practice CRUD operations
- Build a simple REST API with MongoDB
- Learn aggregation pipelines
- Explore MongoDB Atlas (cloud)
- Study indexing strategies

**Season 1 Complete! 🎉** You now have a solid foundation in Node.js - from basics to databases. Ready to build full-stack applications! 💪

