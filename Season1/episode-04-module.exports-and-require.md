## Episode-04 | module.exports & require

### **Lecture Title:**
module.exports & require - Mastering Node.js Module System

---

### **🔥 Core Concept:**
Ab hum seekhenge **modular programming** - the backbone of any professional Node.js application! 💪 This episode dives deep into **CommonJS module system**, which is Node.js's way of organizing code into separate, reusable files (modules).

Akshay bhaiya explains how `require()` and `module.exports` work together to create clean, maintainable codebases. You'll understand **why modules are essential** (imagine writing all code in one giant file! 😱), how Node.js **caches modules**, the difference between `module.exports` and `exports`, and how to structure real-world applications.

This is the foundation for building scalable applications and working with npm packages!

**Why it's important:**
- Every professional Node.js project uses modules - it's not optional!
- Understanding module system is crucial for using npm packages
- Enables **code reusability**, **maintainability**, and **separation of concerns**
- Common interview topic - "Explain module.exports vs exports"
- Foundation for understanding how Express.js, React, and all frameworks work
- Prepares you for modern ES6 modules (import/export)

---

### **💡 Key Definitions:**

- **Module**: A reusable block of code whose existence does not impact other code (encapsulated file with specific functionality).

- **CommonJS**: The module system used by Node.js - defines how modules are created and imported using `require()` and `module.exports`.

- **`require()`**: A built-in Node.js function to import/load modules, JSON files, or local JavaScript files.

- **`module.exports`**: An object that a module returns when it's required by another file - defines what gets exported from a module.

- **`exports`**: A shorthand reference to `module.exports` - can be used for convenience but has limitations.

- **Module Wrapper Function**: Node.js wraps every module in a function to provide module-specific variables (`require`, `module`, `exports`, `__dirname`, `__filename`).

- **Module Caching**: Once a module is loaded, Node.js caches it - subsequent `require()` calls return the cached version (performance optimization).

- **Built-in/Core Modules**: Modules that come with Node.js (`fs`, `http`, `path`, etc.) - don't need installation.

- **Local/File Modules**: Custom modules you create in your project (imported with relative paths like `./utils`).

- **Third-Party/NPM Modules**: Modules installed via npm (like `express`, `lodash`) - imported by package name.

---

### **⚙️ Step-by-Step Explanation:**

**How Node.js Module System Works:**

1. **The Problem Without Modules**
   - All code in one file becomes unmanageable
   - Name conflicts (same variable/function names)
   - Difficult to test individual pieces
   - Hard to reuse code across projects
   - Team collaboration becomes messy

2. **The Solution: Modules**
   - Break code into logical, separate files
   - Each file is a **module** with its own scope
   - Export only what's needed
   - Import only what's required
   - Clean, organized, maintainable codebase

3. **Module Wrapper Function (Hidden by Node.js)**
   ```javascript
   // What you write:
   const x = 10;
   console.log("Hello");

   // What Node.js actually executes:
   (function(exports, require, module, __filename, __dirname) {
       const x = 10;
       console.log("Hello");
   });
   ```
   - This wrapper provides module-scope (variables don't leak)
   - Gives access to `module`, `exports`, `require`, `__filename`, `__dirname`

4. **Module Loading Process**
   ```
   require('./myModule')
        ↓
   1. Resolve: Find the file (check .js, .json, .node extensions)
        ↓
   2. Load: Read the file content
        ↓
   3. Wrap: Wrap in module wrapper function
        ↓
   4. Evaluate: Execute the wrapped code
        ↓
   5. Cache: Store in module cache
        ↓
   6. Return: Return module.exports object
   ```

5. **Types of Modules**
   - **Core/Built-in**: `require('fs')`, `require('http')`
   - **Local**: `require('./utils')`, `require('../config/db')`
   - **NPM**: `require('express')`, `require('lodash')`

6. **Module Caching Mechanism**
   - First `require()` loads and caches module
   - Subsequent `require()` returns cached version
   - Same instance shared across entire application
   - Can access cache via `require.cache`

---

### **💻 Code Example & Detailed Explanation:**

**Example 1: Basic Module Export and Import**

```javascript
// math.js - A simple math utilities module

// Variables inside module are private (not accessible outside)
const PI = 3.14159;
const E = 2.71828;

// Functions to export
function add(a, b) {
    return a + b;
}

function subtract(a, b) {
    return a - b;
}

function multiply(a, b) {
    return a * b;
}

function divide(a, b) {
    if (b === 0) {
        throw new Error("Cannot divide by zero");
    }
    return a / b;
}

// Private function (not exported)
function secretFunction() {
    return "This is private!";
}

// Export multiple functions and variables
module.exports = {
    add: add,
    subtract: subtract,
    multiply: multiply,
    divide: divide,
    PI: PI,
    E: E
};

// ES6 shorthand (when key and value names are same):
// module.exports = { add, subtract, multiply, divide, PI, E };
```

```javascript
// app.js - Using the math module

// Import the math module
const math = require('./math');  // .js extension is optional

console.log("=== Math Module Demo ===\n");

// Use exported functions
console.log("Addition: 10 + 5 =", math.add(10, 5));        // 15
console.log("Subtraction: 10 - 5 =", math.subtract(10, 5)); // 5
console.log("Multiplication: 10 * 5 =", math.multiply(10, 5)); // 50
console.log("Division: 10 / 5 =", math.divide(10, 5));      // 2

// Access exported constants
console.log("\nConstants:");
console.log("PI =", math.PI);  // 3.14159
console.log("E =", math.E);    // 2.71828

// Try to access private function
console.log("\nPrivate function:", math.secretFunction);  // undefined
// secretFunction is NOT exported, so it's not accessible!

// This would cause an error:
// math.secretFunction();  // TypeError: math.secretFunction is not a function
```

**Output:**
```
=== Math Module Demo ===

Addition: 10 + 5 = 15
Subtraction: 10 - 5 = 5
Multiplication: 10 * 5 = 50
Division: 10 / 5 = 2

Constants:
PI = 3.14159
E = 2.71828

Private function: undefined
```

**📝 Detailed Explanation:**

1. **`module.exports = { ... }`**
   - Creates an object with functions and variables to export
   - Only these exported items are accessible when module is required
   - Everything else remains private to the module

2. **`require('./math')`**
   - `./` means current directory (relative path)
   - `.js` extension is optional - Node.js automatically adds it
   - Returns whatever was assigned to `module.exports`
   - Stored in `math` variable

3. **Module Scope (Privacy)**
   - `secretFunction` is NOT in `module.exports`
   - It's private - only accessible within math.js
   - This is **encapsulation** - hiding implementation details

4. **Relative Paths**
   - `./math` - same directory
   - `../math` - parent directory
   - `./utils/math` - subdirectory

---

**Example 2: Different Ways to Export**

```javascript
// calculator.js - Method 1: Export entire object at once

function square(x) {
    return x * x;
}

function cube(x) {
    return x * x * x;
}

// Export everything at the end
module.exports = {
    square,
    cube
};
```

```javascript
// greet.js - Method 2: Export one by one

// Add properties to module.exports directly
module.exports.sayHello = function(name) {
    return `Hello, ${name}!`;
};

module.exports.sayGoodbye = function(name) {
    return `Goodbye, ${name}!`;
};

module.exports.defaultGreeting = "Namaste!";
```

```javascript
// user.js - Method 3: Export a single function/class

// Export a single constructor function
module.exports = function User(name, age) {
    this.name = name;
    this.age = age;

    this.getDetails = function() {
        return `${this.name} is ${this.age} years old`;
    };
};

// Or export a class (ES6)
// module.exports = class User { ... };
```

```javascript
// config.js - Method 4: Export simple values

module.exports = {
    appName: "Namaste Node",
    version: "1.0.0",
    port: 3000,
    database: {
        host: "localhost",
        port: 27017,
        name: "myapp"
    }
};
```

```javascript
// main.js - Using all different export styles

const calculator = require('./calculator');
const greet = require('./greet');
const User = require('./user');
const config = require('./config');

console.log("=== Different Export Styles ===\n");

// Using calculator (object with methods)
console.log("Square of 5:", calculator.square(5));  // 25
console.log("Cube of 3:", calculator.cube(3));      // 27

// Using greet (object built incrementally)
console.log(greet.sayHello("Akshay"));              // Hello, Akshay!
console.log(greet.sayGoodbye("Node.js"));           // Goodbye, Node.js!
console.log(greet.defaultGreeting);                 // Namaste!

// Using User (constructor function)
const user1 = new User("Akshay", 30);
console.log(user1.getDetails());                    // Akshay is 30 years old

// Using config (simple object)
console.log(`${config.appName} v${config.version}`); // Namaste Node v1.0.0
console.log(`Running on port: ${config.port}`);      // Running on port: 3000
```

**📝 Explanation:**

- **Multiple export styles** are valid - choose based on use case
- **Method 1**: Clean, shows all exports at once (recommended)
- **Method 2**: Add exports incrementally (useful for large files)
- **Method 3**: Export single entity (function, class, value)
- **Method 4**: Export configuration objects

---

**Example 3: module.exports vs exports (The Confusion!)**

```javascript
// CORRECT USAGE

// correct1.js - Using module.exports (RECOMMENDED)
module.exports.add = (a, b) => a + b;
module.exports.multiply = (a, b) => a * b;
// ✅ Works perfectly

// correct2.js - Using exports shorthand
exports.add = (a, b) => a + b;
exports.multiply = (a, b) => a * b;
// ✅ Also works (exports is reference to module.exports)

// correct3.js - Replacing entire module.exports
module.exports = {
    add: (a, b) => a + b,
    multiply: (a, b) => a * b
};
// ✅ Works perfectly
```

```javascript
// INCORRECT USAGE - COMMON MISTAKE!

// wrong.js - DON'T DO THIS!
exports = {
    add: (a, b) => a + b,
    multiply: (a, b) => a * b
};
// ❌ DOESN'T WORK! Reassigning 'exports' breaks the reference!

// What actually happens:
// 1. Initially: exports = module.exports = {}
// 2. You reassign: exports = { add, multiply }
// 3. Now exports points to new object
// 4. But module.exports still points to original {}
// 5. Node.js returns module.exports (which is empty {})
```

```javascript
// test-exports.js - Testing the difference

console.log("=== Testing module.exports vs exports ===\n");

// This works
const correct1 = require('./correct1');
console.log("correct1.add(2, 3):", correct1.add(2, 3));  // 5

// This works
const correct2 = require('./correct2');
console.log("correct2.add(2, 3):", correct2.add(2, 3));  // 5

// This works
const correct3 = require('./correct3');
console.log("correct3.add(2, 3):", correct3.add(2, 3));  // 5

// This DOESN'T work
const wrong = require('./wrong');
console.log("wrong:", wrong);  // {} (empty object!)
console.log("wrong.add:", wrong.add);  // undefined
// console.log(wrong.add(2, 3));  // ❌ TypeError: wrong.add is not a function
```

**📝 Understanding the Difference:**

```javascript
// Behind the scenes in Node.js:

// 1. Node.js creates module object
const module = { exports: {} };

// 2. Creates 'exports' as reference to module.exports
const exports = module.exports;  // Same reference!

// 3. Your code runs...

// ✅ CORRECT - Adding properties to reference
exports.add = function() {};  // Also adds to module.exports
module.exports.add = function() {};  // Directly adds to module.exports

// ❌ WRONG - Reassigning breaks reference
exports = { add: function() {} };  // Now exports points elsewhere
// module.exports is still {}

// ✅ CORRECT - Reassigning module.exports works
module.exports = { add: function() {} };  // This is what gets returned

// 4. Node.js returns module.exports (NOT exports)
return module.exports;
```

**🎯 Golden Rule:**
- **Use `exports.xyz`** to add properties ✅
- **Use `module.exports`** to replace entire export ✅
- **Never reassign `exports`** directly ❌

---

**Example 4: Module Caching in Action**

```javascript
// counter.js - Module with state

let count = 0;

function increment() {
    count++;
    console.log(`Counter incremented to: ${count}`);
    return count;
}

function getCount() {
    return count;
}

module.exports = {
    increment,
    getCount
};
```

```javascript
// app-cache.js - Demonstrating module caching

console.log("=== Module Caching Demo ===\n");

// First import
const counter1 = require('./counter');
console.log("First import - Initial count:", counter1.getCount());  // 0

counter1.increment();  // Counter incremented to: 1
counter1.increment();  // Counter incremented to: 2
counter1.increment();  // Counter incremented to: 3

// Second import - same module
const counter2 = require('./counter');
console.log("\nSecond import - Count is:", counter2.getCount());  // 3 (not 0!)

counter2.increment();  // Counter incremented to: 4

// Third import
const counter3 = require('./counter');
console.log("\nThird import - Count is:", counter3.getCount());  // 4

// All three are the SAME object!
console.log("\nAre they the same?");
console.log("counter1 === counter2:", counter1 === counter2);  // true
console.log("counter2 === counter3:", counter2 === counter3);  // true

// Check module cache
console.log("\nModule cache keys:");
console.log(Object.keys(require.cache));
```

**Output:**
```
=== Module Caching Demo ===

First import - Initial count: 0
Counter incremented to: 1
Counter incremented to: 2
Counter incremented to: 3

Second import - Count is: 3
Counter incremented to: 4

Third import - Count is: 4

Are they the same?
counter1 === counter2: true
counter2 === counter3: true

Module cache keys:
[
  '/path/to/app-cache.js',
  '/path/to/counter.js'
]
```

**📝 Explanation:**

1. **First `require('./counter')`**
   - Loads counter.js
   - Executes the code
   - Caches the result (module.exports object)
   - Returns the cached object

2. **Second `require('./counter')`**
   - Checks cache - finds counter.js already loaded
   - Returns the SAME cached object (doesn't re-execute)
   - Count is still 3 (state is preserved)

3. **All imports share the same instance**
   - Singleton pattern by default
   - Changes in one reference affect all others
   - Useful for shared configurations, database connections

4. **Module Cache**
   - Accessible via `require.cache`
   - Keys are absolute file paths
   - Can delete from cache to force reload: `delete require.cache[require.resolve('./counter')]`

---

**Example 5: Built-in, Local, and NPM Modules**

```javascript
// modules-demo.js

console.log("=== Types of Modules ===\n");

// 1. BUILT-IN/CORE MODULES (no ./ or path needed)
const fs = require('fs');        // File System
const path = require('path');    // Path utilities
const http = require('http');    // HTTP server
const os = require('os');        // Operating System info

console.log("1. Built-in Modules:");
console.log("   OS Platform:", os.platform());
console.log("   Node version:", process.version);

// 2. LOCAL/FILE MODULES (need ./ or relative path)
const myMath = require('./math');           // Same directory
const config = require('../config/app');    // Parent directory
const utils = require('./helpers/utils');   // Subdirectory

console.log("\n2. Local Modules:");
console.log("   Math.add(5, 3):", myMath.add(5, 3));

// 3. NPM/THIRD-PARTY MODULES (installed via npm)
// First install: npm install lodash
// const _ = require('lodash');
// const express = require('express');

console.log("\n3. NPM Modules:");
console.log("   (Need to install via: npm install <package-name>)");
```

**📝 Module Resolution Order:**

When you do `require('something')`:

1. **Check if built-in module** (fs, http, path, etc.)
   - If yes, load and return it

2. **If starts with `./` or `../` or `/`**
   - Treat as file/local module
   - Look for:
     - `something.js`
     - `something.json`
     - `something.node`
     - `something/index.js`

3. **Otherwise, treat as NPM module**
   - Look in `node_modules` folder in current directory
   - If not found, check parent directory's `node_modules`
   - Keep going up until root
   - If not found, throw "Cannot find module" error

---

**Example 6: Destructuring Imports (Modern Approach)**

```javascript
// utilities.js

module.exports = {
    formatDate: (date) => date.toISOString(),
    formatCurrency: (amount) => `$${amount.toFixed(2)}`,
    capitalize: (str) => str.charAt(0).toUpperCase() + str.slice(1),
    slugify: (str) => str.toLowerCase().replace(/\s+/g, '-'),
    randomNumber: (min, max) => Math.floor(Math.random() * (max - min + 1)) + min
};
```

```javascript
// app-destructure.js

// OLD WAY - Import entire object
const utilities = require('./utilities');
utilities.formatDate(new Date());
utilities.capitalize('hello');

// NEW WAY - Destructure (recommended!)
const { formatDate, capitalize, slugify } = require('./utilities');

console.log("=== Destructuring Imports ===\n");

// Use directly without prefix
console.log("Date:", formatDate(new Date()));
console.log("Capitalize:", capitalize('hello world'));
console.log("Slugify:", slugify('Hello World 2024'));

// Benefits:
// ✅ Cleaner code
// ✅ Only import what you need
// ✅ Easier to read
// ✅ Similar to ES6 import syntax
```

**Output:**
```
=== Destructuring Imports ===

Date: 2024-01-15T10:30:00.000Z
Capitalize: Hello world
Slugify: hello-world-2024
```

---

### **❌ Common Mistakes & Fixes:**

1. **Mistake: Reassigning `exports` Instead of `module.exports`**
   - ❌ Problem: The most common module export mistake!
   ```javascript
   // ❌ WRONG - This doesn't work!
   exports = {
       myFunction: function() {}
   };
   // exports now points to new object, but Node.js returns module.exports
   ```
   - ✅ Fix: Use `module.exports` for reassignment
   ```javascript
   // ✅ CORRECT - Option 1: Replace entire exports
   module.exports = {
       myFunction: function() {}
   };

   // ✅ CORRECT - Option 2: Add to exports
   exports.myFunction = function() {};
   module.exports.myFunction = function() {};  // Same thing
   ```
   - 💡 **Remember**: `exports` is just a reference to `module.exports`. Reassigning `exports` breaks that reference, but `module.exports` is what Node.js actually returns!

2. **Mistake: Forgetting `./` for Local Modules**
   - ❌ Problem: Treating local file as npm module
   ```javascript
   // ❌ WRONG - Node.js will look in node_modules!
   const myModule = require('myModule');
   // Error: Cannot find module 'myModule'
   ```
   - ✅ Fix: Use relative path with `./` or `../`
   ```javascript
   // ✅ CORRECT - Tell Node.js it's a local file
   const myModule = require('./myModule');      // Same directory
   const config = require('../config/app');     // Parent directory
   const utils = require('./helpers/utils');    // Subdirectory
   ```
   - 🎯 **Rule**: Built-in and npm modules - no path. Local modules - always use path!

3. **Mistake: Circular Dependencies**
   - ❌ Problem: Two modules require each other
   ```javascript
   // a.js
   const b = require('./b');
   console.log('A:', b.name);
   module.exports = { name: 'Module A' };

   // b.js
   const a = require('./a');
   console.log('B:', a.name);  // undefined! (circular dependency)
   module.exports = { name: 'Module B' };
   ```
   - ✅ Fix: Restructure code to avoid circular dependencies
   ```javascript
   // Better: Create a third module that both can use
   // shared.js
   module.exports = {
       sharedData: 'Common data'
   };

   // a.js
   const shared = require('./shared');
   // Use shared, don't require b

   // b.js
   const shared = require('./shared');
   // Use shared, don't require a
   ```
   - 💡 **Design principle**: If two modules need each other, they're probably too coupled - refactor into separate concerns!

4. **Mistake: Expecting Module Re-execution on Multiple Requires**
   - ❌ Problem: Thinking module code runs every time
   ```javascript
   // logger.js
   console.log("Logger initialized!");  // Only runs ONCE
   let count = 0;
   module.exports = {
       log: (msg) => console.log(`[${++count}]`, msg)
   };

   // app.js
   const logger1 = require('./logger');  // "Logger initialized!" prints
   const logger2 = require('./logger');  // Nothing prints (cached!)
   logger1.log('Test');  // [1] Test
   logger2.log('Test');  // [2] Test (same count, same instance!)
   ```
   - ✅ Fix: Understand caching - use factory pattern if you need new instances
   ```javascript
   // logger-factory.js
   module.exports = function createLogger() {
       let count = 0;
       return {
           log: (msg) => console.log(`[${++count}]`, msg)
       };
   };

   // app.js
   const createLogger = require('./logger-factory');
   const logger1 = createLogger();  // New instance
   const logger2 = createLogger();  // Another new instance
   logger1.log('Test');  // [1] Test
   logger2.log('Test');  // [1] Test (separate count!)
   ```

---

### **❓ Interview Questions:**

1. **Q: Explain the difference between `module.exports` and `exports`. When would each fail?**
   - **A:** This is a very common interview question!

   **The Relationship:**
   ```javascript
   // Initially, Node.js does:
   const module = { exports: {} };
   const exports = module.exports;  // 'exports' is a reference!
   ```

   **`module.exports`:**
   - The actual object that gets returned when module is required
   - Can be reassigned completely
   - Final authority - this is what Node.js returns

   **`exports`:**
   - Shorthand reference to `module.exports`
   - Convenient for adding properties
   - Cannot be reassigned (loses reference if you do)

   **When `exports` works:**
   ```javascript
   // ✅ Adding properties - works!
   exports.add = (a, b) => a + b;
   exports.name = "Calculator";
   // These also add to module.exports
   ```

   **When `exports` fails:**
   ```javascript
   // ❌ Reassignment - fails!
   exports = { add: (a, b) => a + b };
   // exports now points to new object
   // module.exports still points to original {}
   // Node.js returns module.exports (empty)
   ```

   **When `module.exports` always works:**
   ```javascript
   // ✅ Reassignment - works!
   module.exports = { add: (a, b) => a + b };

   // ✅ Adding properties - works!
   module.exports.add = (a, b) => a + b;
   ```

   **Best Practice:**
   - Use `module.exports` for full object replacement
   - Use `exports.xyz` for adding properties (if you prefer)
   - Stay consistent within a project
   - When in doubt, use `module.exports` - it always works!

2. **Q: What is module caching in Node.js? What are its implications?**
   - **A:**

   **What is Module Caching:**
   - When a module is loaded via `require()`, Node.js caches it
   - Cache key is the **absolute file path**
   - Subsequent `require()` calls return the **cached version**
   - Module code executes only **once** (on first load)

   **How it works:**
   ```javascript
   // First require
   const mod1 = require('./myModule');  // Loads, executes, caches

   // Second require
   const mod2 = require('./myModule');  // Returns cached version

   // They're the SAME object
   console.log(mod1 === mod2);  // true
   ```

   **Implications:**

   **1. Performance Benefits:**
   - Faster - no need to read and execute file again
   - Memory efficient - single instance in memory

   **2. Shared State:**
   ```javascript
   // database.js
   let connection = null;
   module.exports = {
       connect: () => { connection = createConnection(); },
       getConnection: () => connection
   };

   // All files share same connection (singleton pattern)
   ```

   **3. Initialization Runs Once:**
   ```javascript
   // config.js
   console.log("Loading config...");  // Prints only once
   module.exports = { port: 3000 };
   ```

   **4. Potential Issues:**
   - Modules with mutable state can cause unexpected behavior
   - Hard to test - cached state persists between tests
   - Circular dependencies can cause partial exports

   **Accessing Cache:**
   ```javascript
   console.log(require.cache);  // Object with all cached modules
   ```

   **Clearing Cache (for testing):**
   ```javascript
   delete require.cache[require.resolve('./myModule')];
   const fresh = require('./myModule');  // Loads fresh
   ```

   **Best Practice:**
   - Embrace caching for singletons (DB connections, configs)
   - Be aware of shared state
   - Use factory functions if you need multiple instances

3. **Q: How does Node.js resolve modules? Explain the resolution algorithm.**
   - **A:**

   **Module Resolution Order:**

   When you call `require('X')`:

   **Step 1: Check if X is a core module**
   ```javascript
   require('fs')      // ✅ Core module - load immediately
   require('http')    // ✅ Core module - load immediately
   require('express') // ❌ Not core - continue to step 2
   ```

   **Step 2: Check if X starts with '/', './', or '../'**
   ```javascript
   require('./utils')     // ✅ Relative - load as file
   require('../config')   // ✅ Relative - load as file
   require('/abs/path')   // ✅ Absolute - load as file
   ```

   **For file modules, try:**
   1. `X.js`
   2. `X.json`
   3. `X.node` (compiled addon)
   4. `X/index.js`
   5. `X/index.json`
   6. `X/package.json` (check "main" field)

   **Step 3: Otherwise, look in node_modules**
   ```javascript
   require('express')  // Look in node_modules
   ```

   **Search path (from current directory upward):**
   ```
   /current/directory/node_modules/express
   /current/node_modules/express
   /node_modules/express
   (repeat until filesystem root)
   ```

   **Example:**
   ```
   Project: /home/user/project/src/app.js

   require('express')

   Searches:
   1. /home/user/project/src/node_modules/express
   2. /home/user/project/node_modules/express  ← Usually found here
   3. /home/user/node_modules/express
   4. /home/node_modules/express
   5. /node_modules/express
   ```

   **If not found anywhere:**
   ```
   Error: Cannot find module 'X'
   ```

   **Special Case - package.json:**
   ```json
   {
     "name": "my-package",
     "main": "lib/index.js"
   }
   ```
   ```javascript
   require('my-package')  // Loads lib/index.js (not index.js)
   ```

4. **Q: What are the different types of modules in Node.js?**
   - **A:**

   **1. Core/Built-in Modules:**
   - Come bundled with Node.js installation
   - Don't need npm installation
   - Highest priority in resolution

   **Examples:**
   ```javascript
   const fs = require('fs');           // File System
   const http = require('http');       // HTTP server
   const path = require('path');       // Path utilities
   const crypto = require('crypto');   // Cryptography
   const os = require('os');           // OS information
   const events = require('events');   // Event Emitter
   ```

   **Usage:**
   - No path needed
   - Optimized and well-tested
   - Documentation on nodejs.org

   **2. Local/File Modules:**
   - Created by you in your project
   - Need relative or absolute path
   - Encapsulate your application logic

   **Examples:**
   ```javascript
   const config = require('./config');           // Same directory
   const db = require('./database/connection');  // Subdirectory
   const utils = require('../helpers/utils');    // Parent directory
   ```

   **Usage:**
   - Always use `./`, `../`, or absolute path
   - Organize by feature or layer

   **3. Third-Party/NPM Modules:**
   - Installed via npm or yarn
   - Located in `node_modules` folder
   - Created by community or companies

   **Examples:**
   ```javascript
   const express = require('express');     // Web framework
   const mongoose = require('mongoose');   // MongoDB ODM
   const lodash = require('lodash');       // Utility library
   const axios = require('axios');         // HTTP client
   ```

   **Usage:**
   - Install first: `npm install <package-name>`
   - Listed in `package.json` dependencies
   - No path needed

   **4. JSON Modules:**
   - JSON files loaded as objects
   - Automatically parsed

   **Example:**
   ```javascript
   const config = require('./config.json');
   const package = require('./package.json');

   console.log(config.appName);
   console.log(package.version);
   ```

   **Summary Table:**

   | Type | Path Needed? | Installation? | Example |
   |------|--------------|---------------|---------|
   | Core | No | No (built-in) | `require('fs')` |
   | Local | Yes (`./` or `../`) | No (you create) | `require('./utils')` |
   | NPM | No | Yes (`npm install`) | `require('express')` |
   | JSON | Yes (`./` or `../`) | No | `require('./data.json')` |

---

### **🌐 Real-World Usage:**

**How Modules Are Used in Production Applications:**

1. **Separation of Concerns**
   ```
   project/
   ├── models/           (Database models)
   │   ├── User.js
   │   └── Product.js
   ├── controllers/      (Business logic)
   │   ├── userController.js
   │   └── productController.js
   ├── routes/           (API routes)
   │   ├── userRoutes.js
   │   └── productRoutes.js
   ├── middleware/       (Custom middleware)
   │   ├── auth.js
   │   └── errorHandler.js
   ├── config/           (Configuration)
   │   ├── database.js
   │   └── app.js
   └── utils/            (Helper functions)
       ├── validators.js
       └── formatters.js
   ```

2. **Database Connection (Singleton via Caching)**
   ```javascript
   // config/database.js
   const mongoose = require('mongoose');

   let connection = null;

   module.exports = {
       connect: async () => {
           if (!connection) {
               connection = await mongoose.connect(process.env.DB_URL);
               console.log('Database connected');
           }
           return connection;
       },
       getConnection: () => connection
   };

   // Used in multiple files, but connects only once!
   ```

3. **Express.js Application Structure**
   ```javascript
   // app.js
   const express = require('express');          // NPM module
   const userRoutes = require('./routes/user'); // Local module
   const config = require('./config/app');      // Local module

   const app = express();
   app.use('/api/users', userRoutes);
   app.listen(config.port);
   ```

4. **Shared Utilities Across Team**
   ```javascript
   // Company creates private npm package
   // @company/validators

   // Backend uses it
   const { validateEmail } = require('@company/validators');

   // Frontend also uses it
   import { validateEmail } from '@company/validators';
   ```

5. **Environment-Specific Configuration**
   ```javascript
   // config/index.js
   const env = process.env.NODE_ENV || 'development';

   const configs = {
       development: require('./dev'),
       production: require('./prod'),
       test: require('./test')
   };

   module.exports = configs[env];
   ```

---

### **⚡ Summary & Quick Revision:**

✅ **CommonJS modules** use `require()` to import and `module.exports` to export - every Node.js file is a module with its own scope!

✅ **`module.exports`** is what gets returned; **`exports`** is just a reference - use `module.exports =` for full replacement, `exports.xyz =` for adding properties!

✅ **Module caching** means code executes once and result is cached - all `require()` calls return the same instance (singleton pattern by default)!

✅ **Three types**: Core modules (no path), Local modules (use `./`), NPM modules (install first) - organize code into logical, reusable pieces! 🚀

---

**🎯 Pro Tip:**
Modern Node.js also supports **ES6 modules** (`import/export`) with `.mjs` extension or `"type": "module"` in package.json. But CommonJS is still the standard - learn it well first!

---

**Next Episode Preview:** Episode-05 will take you on a tour of the **Node.js GitHub repository** - you'll learn how to explore the source code, understand Node.js internals, and even prepare to contribute! 🔥
