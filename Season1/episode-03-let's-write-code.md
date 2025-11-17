## Episode-03 | Let's Write Code

### **Lecture Title:**
Let's Write Code - Your First Node.js Programs & Understanding Execution

---

### **🔥 Core Concept:**
Ab theory bahut ho gayi, ab code likhte hain! 🚀 This episode is all about **getting your hands dirty** with actual Node.js code. Akshay bhaiya takes you through writing your first server-side JavaScript programs, understanding how Node.js executes code, exploring the **REPL (Read-Eval-Print Loop)**, and discovering **global objects** available in Node.js runtime.

You'll see the **practical differences** between browser JavaScript and Node.js - how to run files, what global objects are available, how to take input/output, and how Node.js processes your code. This is where theory meets practice, and you start feeling like a backend developer!

**Why it's important:**
- Hands-on experience solidifies theoretical concepts learned so far
- Understanding Node.js execution environment is crucial for debugging
- Learning global objects (`process`, `__dirname`, `__filename`) used in every real project
- Getting comfortable with terminal/command line for Node.js development
- Building muscle memory for Node.js development workflow

---

### **💡 Key Definitions:**

- **REPL (Read-Eval-Print Loop)**: An interactive Node.js shell that reads input, evaluates it, prints the result, and loops back - great for testing JavaScript snippets quickly.

- **`global` object**: The top-level namespace object in Node.js (equivalent to `window` in browsers) - contains globally accessible functions and properties.

- **`process` object**: A global object providing information about and control over the current Node.js process (arguments, environment variables, exit codes, etc.).

- **`__dirname`**: A special variable containing the absolute path of the directory containing the currently executing file.

- **`__filename`**: A special variable containing the absolute path of the currently executing file (including the filename).

- **`module` object**: Represents the current module - contains information about the file and controls what gets exported.

- **`require()` function**: A built-in function to import modules, JSON files, or local files into the current file.

- **`console` object**: Same as browser - used for logging output (available in both browser and Node.js).

---

### **⚙️ Step-by-Step Explanation:**

**How to Write and Run Node.js Code:**

1. **Method 1: REPL (Interactive Mode)**
   - Open terminal/command prompt
   - Type `node` and press Enter
   - Get interactive JavaScript shell
   - Perfect for quick testing and experimentation
   - Use `.exit` or Ctrl+C twice to exit

2. **Method 2: Script Files (Production Way)**
   - Create a `.js` file (e.g., `app.js`)
   - Write JavaScript code in the file
   - Run using `node filename.js` in terminal
   - This is how real applications run

3. **Node.js Execution Flow**
   ```
   Your Code (app.js)
        ↓
   Node.js reads file
        ↓
   V8 Engine compiles JS to machine code
        ↓
   Code executes line by line
        ↓
   If async operations → libuv handles them
        ↓
   Output to console/files/network
        ↓
   Process exits (unless server keeps running)
   ```

4. **Global Objects Available in Node.js**
   - **Browser has**: `window`, `document`, `localStorage`, `alert`
   - **Node.js has**: `global`, `process`, `Buffer`, `__dirname`, `__filename`
   - **Common to both**: `console`, `setTimeout`, `setInterval`, `Promise`

5. **Understanding `process` Object**
   - Contains runtime information
   - `process.argv` - command line arguments
   - `process.env` - environment variables
   - `process.cwd()` - current working directory
   - `process.exit()` - terminate the program
   - `process.version` - Node.js version

6. **File Path Variables**
   - `__dirname` - directory path of current file
   - `__filename` - full path with filename
   - Essential for file operations and module paths

---

### **💻 Code Example & Detailed Explanation:**

**Example 1: Using Node.js REPL**

```bash
# Open terminal and type:
$ node

# You'll see:
Welcome to Node.js v18.17.0.
Type ".help" for more information.
>

# Now you can write JavaScript:
> console.log("Hello from REPL!");
Hello from REPL!
undefined

> let x = 10;
undefined

> let y = 20;
undefined

> x + y
30

> Math.sqrt(144)
12

> const greet = (name) => `Namaste ${name}!`;
undefined

> greet("Node.js")
'Namaste Node.js!'

> .exit  // Exit REPL
```

**📝 Explanation:**
- REPL executes each line immediately
- Returns `undefined` for statements that don't return a value
- Perfect for testing small snippets, calculations, or API exploration
- `.help` shows available REPL commands
- `.exit` or Ctrl+C (twice) to quit

---

**Example 2: Your First Node.js Script**

```javascript
// first-program.js

console.log("🚀 Welcome to Node.js!");
console.log("This is running on the server, not in a browser!");

// Variables work same as browser JavaScript
const message = "JavaScript on Server";
const year = 2024;

console.log(`${message} - ${year}`);

// Functions work exactly the same
function add(a, b) {
    return a + b;
}

const result = add(15, 25);
console.log("15 + 25 =", result);

// Arrow functions
const multiply = (a, b) => a * b;
console.log("5 × 6 =", multiply(5, 6));

// Arrays and objects
const technologies = ["Node.js", "Express", "MongoDB"];
console.log("Backend Stack:", technologies);

const developer = {
    name: "Akshay",
    role: "Backend Developer",
    language: "JavaScript"
};

console.log("Developer Info:", developer);
```

**Run this file:**
```bash
$ node first-program.js
```

**Output:**
```
🚀 Welcome to Node.js!
This is running on the server, not in a browser!
JavaScript on Server - 2024
15 + 25 = 40
5 × 6 = 30
Backend Stack: [ 'Node.js', 'Express', 'MongoDB' ]
Developer Info: { name: 'Akshay', role: 'Backend Developer', language: 'JavaScript' }
```

**📝 Explanation:**
- Standard JavaScript code works perfectly in Node.js
- `console.log()` outputs to terminal (not browser console)
- ES6+ features (const, let, arrow functions, template literals) fully supported
- Objects and arrays work identically to browser JavaScript
- No HTML, no DOM - pure JavaScript execution

---

**Example 3: Exploring Global Objects**

```javascript
// global-objects.js

console.log("=== Node.js Global Objects ===\n");

// 1. Global object (like 'window' in browsers)
console.log("1. Global Object Type:", typeof global);
// Note: In browsers it's 'window', in Node.js it's 'global'

// 2. __filename - full path of current file
console.log("\n2. Current File Path:");
console.log("   __filename:", __filename);

// 3. __dirname - directory path of current file
console.log("\n3. Current Directory:");
console.log("   __dirname:", __dirname);

// 4. Process object - information about current Node.js process
console.log("\n4. Process Information:");
console.log("   Node.js Version:", process.version);
console.log("   Platform:", process.platform);  // 'win32', 'darwin', 'linux'
console.log("   Process ID:", process.pid);

// 5. Current Working Directory
console.log("\n5. Working Directory:");
console.log("   process.cwd():", process.cwd());

// 6. Command Line Arguments
console.log("\n6. Command Line Arguments:");
console.log("   process.argv:", process.argv);
// process.argv[0] - path to node executable
// process.argv[1] - path to JavaScript file
// process.argv[2+] - your custom arguments

// 7. Environment Variables
console.log("\n7. Environment Variables:");
console.log("   NODE_ENV:", process.env.NODE_ENV || "not set");
console.log("   USER:", process.env.USER || process.env.USERNAME);
```

**Run this file:**
```bash
$ node global-objects.js
```

**Output (example):**
```
=== Node.js Global Objects ===

1. Global Object Type: object

2. Current File Path:
   __filename: /Users/akshay/projects/namaste-node/global-objects.js

3. Current Directory:
   __dirname: /Users/akshay/projects/namaste-node

4. Process Information:
   Node.js Version: v18.17.0
   Platform: darwin
   Process ID: 12345

5. Working Directory:
   process.cwd(): /Users/akshay/projects/namaste-node

6. Command Line Arguments:
   process.argv: [
     '/usr/local/bin/node',
     '/Users/akshay/projects/namaste-node/global-objects.js'
   ]

7. Environment Variables:
   NODE_ENV: not set
   USER: akshay
```

**📝 Explanation:**

- **`__filename`**: Absolute path to the current file - useful when you need to reference the file itself or build paths relative to it

- **`__dirname`**: Absolute path to the directory containing current file - crucial for file operations (reading files, creating folders relative to script location)

- **`process.version`**: Shows Node.js version (e.g., v18.17.0) - helps ensure compatibility

- **`process.platform`**: OS platform (win32, darwin for macOS, linux) - useful for platform-specific code

- **`process.pid`**: Process ID - useful for debugging, logging, or process management

- **`process.cwd()`**: Current Working Directory - where you ran `node` command from (can differ from `__dirname`)

- **`process.argv`**: Array of command-line arguments - first two are always node path and script path, rest are your custom args

- **`process.env`**: Environment variables - used for configuration (API keys, database URLs, etc.)

---

**Example 4: Working with Command Line Arguments**

```javascript
// calculator.js - A simple command-line calculator

// process.argv structure:
// [0] - node executable path
// [1] - script file path
// [2] - first argument (our first number)
// [3] - second argument (operator)
// [4] - third argument (our second number)

const args = process.argv;

console.log("All arguments:", args);
console.log("\n");

// Skip first two elements (node and file path)
const num1 = parseFloat(args[2]);
const operator = args[3];
const num2 = parseFloat(args[4]);

// Validate input
if (isNaN(num1) || isNaN(num2)) {
    console.log("❌ Error: Please provide valid numbers");
    console.log("Usage: node calculator.js <num1> <operator> <num2>");
    console.log("Example: node calculator.js 10 + 5");
    process.exit(1);  // Exit with error code
}

// Perform calculation
let result;

switch(operator) {
    case '+':
        result = num1 + num2;
        break;
    case '-':
        result = num1 - num2;
        break;
    case '*':
    case 'x':
        result = num1 * num2;
        break;
    case '/':
        if (num2 === 0) {
            console.log("❌ Error: Cannot divide by zero!");
            process.exit(1);
        }
        result = num1 / num2;
        break;
    default:
        console.log("❌ Error: Invalid operator. Use +, -, *, or /");
        process.exit(1);
}

// Display result
console.log(`✅ Result: ${num1} ${operator} ${num2} = ${result}`);

// Exit successfully
process.exit(0);
```

**Run with different arguments:**

```bash
$ node calculator.js 10 + 5
All arguments: [
  '/usr/local/bin/node',
  '/Users/akshay/projects/calculator.js',
  '10',
  '+',
  '5'
]

✅ Result: 10 + 5 = 15

$ node calculator.js 20 * 3
✅ Result: 20 * 3 = 60

$ node calculator.js 100 / 4
✅ Result: 100 / 4 = 25

$ node calculator.js hello + 5
❌ Error: Please provide valid numbers
Usage: node calculator.js <num1> <operator> <num2>
Example: node calculator.js 10 + 5
```

**📝 Explanation:**

- **`process.argv`**: Array containing command-line arguments
  - `args[0]`: Path to Node.js executable
  - `args[1]`: Path to the JavaScript file being executed
  - `args[2+]`: Your custom arguments

- **`parseFloat()`**: Converts string arguments to numbers (command line args are always strings)

- **`process.exit(1)`**: Terminates the program with error code 1 (non-zero means error)

- **`process.exit(0)`**: Terminates with success code 0 (convention: 0 = success)

- **Real-world usage**: CLI tools like `npm`, `webpack`, `eslint` all work this way - they read command-line arguments to know what to do!

---

**Example 5: Browser vs Node.js - What Works Where?**

```javascript
// compatibility-test.js

console.log("=== Testing Browser vs Node.js Features ===\n");

// ✅ WORKS IN BOTH (Browser & Node.js)
console.log("✅ console.log - Works everywhere");

const timeout = setTimeout(() => {
    console.log("✅ setTimeout - Works everywhere");
}, 1000);

const numbers = [1, 2, 3, 4, 5];
console.log("✅ Array methods:", numbers.map(n => n * 2));

const promise = new Promise((resolve) => resolve("✅ Promises work"));
promise.then(msg => console.log(msg));

// ✅ WORKS IN NODE.JS ONLY
console.log("\n✅ Node.js-only features:");
console.log("   __dirname:", __dirname);
console.log("   process.version:", process.version);

// To use 'require', it must be in the file
const fs = require('fs');  // ✅ File System module - Node.js only
console.log("   fs module loaded:", typeof fs);

// ❌ DOES NOT WORK IN NODE.JS (Browser-only)
console.log("\n❌ Browser-only features (will cause errors in Node.js):");
console.log("   document:", typeof document);        // 'undefined'
console.log("   window:", typeof window);            // 'undefined'
console.log("   localStorage:", typeof localStorage);// 'undefined'

// Uncomment these to see errors:
// document.getElementById('test');  // ❌ ReferenceError: document is not defined
// window.alert("Hello");            // ❌ ReferenceError: window is not defined
// localStorage.setItem('k', 'v');   // ❌ ReferenceError: localStorage is not defined
```

**Output:**
```
=== Testing Browser vs Node.js Features ===

✅ console.log - Works everywhere
✅ Array methods: [ 2, 4, 6, 8, 10 ]
✅ Promises work
✅ setTimeout - Works everywhere

✅ Node.js-only features:
   __dirname: /Users/akshay/projects/namaste-node
   process.version: v18.17.0
   fs module loaded: object

❌ Browser-only features (will cause errors in Node.js):
   document: undefined
   window: undefined
   localStorage: undefined
```

**📝 Explanation:**

**Works in BOTH:**
- Core JavaScript: variables, functions, arrays, objects, classes
- Async features: setTimeout, setInterval, Promise, async/await
- console object: console.log, console.error, console.table
- Math, Date, JSON, Array methods, String methods

**Node.js ONLY:**
- File system: `fs` module
- Server creation: `http`, `https` modules
- Path manipulation: `path` module
- Global objects: `global`, `process`, `Buffer`, `__dirname`, `__filename`
- Module system: `require`, `module.exports`

**Browser ONLY:**
- DOM: `document`, `window`
- Storage: `localStorage`, `sessionStorage`
- Navigation: `location`, `history`
- UI: `alert`, `confirm`, `prompt`
- Browser APIs: `fetch` (with CORS restrictions), Geolocation, WebRTC

---

### **❌ Common Mistakes & Fixes:**

1. **Mistake: Trying to Use DOM in Node.js**
   - ❌ Problem: Writing browser code in Node.js files
   ```javascript
   // ❌ This will crash in Node.js!
   const button = document.querySelector('.btn');  // ReferenceError!
   button.addEventListener('click', () => {
       console.log('Clicked');
   });
   ```
   - ✅ Fix: Remember your environment! Node.js has no DOM.
   ```javascript
   // ✅ In Node.js, work with files, servers, databases
   const fs = require('fs');
   const http = require('http');

   // Create a server instead of manipulating DOM
   const server = http.createServer((req, res) => {
       res.end('Hello from server!');
   });
   ```
   - 💡 **Rule of thumb**: If it interacts with UI → Browser. If it deals with files/servers/databases → Node.js.

2. **Mistake: Confusing `__dirname` with `process.cwd()`**
   - ❌ Problem: Using them interchangeably without understanding the difference
   ```javascript
   // These are NOT always the same!
   console.log(__dirname);      // Directory of THIS file
   console.log(process.cwd());  // Directory where you RAN node command
   ```
   - ✅ Fix: Understand when to use which:
   ```javascript
   // Use __dirname for file operations relative to script location
   const fs = require('fs');
   const path = require('path');

   // ✅ CORRECT - Reads file relative to script location
   const filePath = path.join(__dirname, 'data.txt');
   fs.readFile(filePath, 'utf8', (err, data) => {
       console.log(data);
   });

   // ❌ WRONG - Will fail if you run script from different directory
   fs.readFile('./data.txt', 'utf8', callback);  // Relative to cwd()
   ```
   - **Example scenario**:
     ```
     Project structure:
     /project
       /scripts
         script.js
       data.txt

     If you run: cd /project && node scripts/script.js
     - process.cwd() = /project
     - __dirname = /project/scripts
     ```

3. **Mistake: Not Handling Command-Line Arguments Properly**
   - ❌ Problem: Forgetting that `process.argv` always includes node and script path
   ```javascript
   // ❌ Wrong - Will get node path as first argument
   const name = process.argv[0];  // Gets '/usr/local/bin/node'
   console.log(`Hello ${name}`);
   ```
   - ✅ Fix: Always skip first two elements
   ```javascript
   // ✅ Correct - Skip first two
   const name = process.argv[2];  // Gets actual first argument

   if (!name) {
       console.log("Usage: node script.js <your-name>");
       process.exit(1);
   }

   console.log(`Hello ${name}!`);
   ```
   - Better approach using destructuring:
   ```javascript
   // ✅ Best - Destructure and skip first two
   const [, , name, age, city] = process.argv;

   console.log(`Name: ${name}, Age: ${age}, City: ${city}`);
   ```

---

### **❓ Interview Questions:**

1. **Q: What is Node.js REPL? When would you use it?**
   - **A:** REPL stands for **Read-Eval-Print Loop** - an interactive Node.js shell.

   **How it works:**
   - **Read**: Takes user input (JavaScript code)
   - **Eval**: Evaluates/executes the input
   - **Print**: Prints the result
   - **Loop**: Repeats the process

   **How to use:**
   - Type `node` in terminal (without filename)
   - Get interactive prompt `>`
   - Write JavaScript and see immediate results
   - Exit with `.exit` or Ctrl+C (twice)

   **When to use REPL:**
   ✅ Quick testing of JavaScript snippets
   ✅ Exploring Node.js APIs and modules
   ✅ Debugging - test functions or logic
   ✅ Math calculations or data transformations
   ✅ Learning - experiment with new features

   **When NOT to use:**
   ❌ Writing production code (use files instead)
   ❌ Complex multi-line programs
   ❌ Code that needs to be saved and version-controlled

   **Special REPL commands:**
   - `.help` - Show available commands
   - `.exit` - Exit REPL
   - `.save filename` - Save session to file
   - `.load filename` - Load file into session

2. **Q: Explain `global`, `process`, `__dirname`, and `__filename` in Node.js.**
   - **A:**

   **1. `global` object:**
   - Top-level namespace in Node.js (like `window` in browsers)
   - Contains globally accessible properties and functions
   - Anything attached to `global` is available everywhere
   ```javascript
   global.myVar = "Hello";
   console.log(myVar);  // Accessible without 'global.'

   // But avoid polluting global namespace!
   ```

   **2. `process` object:**
   - Provides info about current Node.js process
   - Control over process execution

   **Key properties/methods:**
   - `process.argv` - Command-line arguments array
   - `process.env` - Environment variables object
   - `process.cwd()` - Current working directory
   - `process.exit(code)` - Exit process (0=success, 1=error)
   - `process.version` - Node.js version
   - `process.platform` - OS platform
   - `process.pid` - Process ID

   **3. `__dirname` (2 underscores):**
   - **Absolute path** to directory containing current file
   - NOT a global property - specific to each module
   - Essential for file operations
   ```javascript
   // If file is at: /Users/akshay/project/src/app.js
   console.log(__dirname);  // /Users/akshay/project/src
   ```

   **4. `__filename` (2 underscores):**
   - **Absolute path** to current file (including filename)
   - Also module-specific
   ```javascript
   // If file is: /Users/akshay/project/src/app.js
   console.log(__filename);  // /Users/akshay/project/src/app.js
   ```

   **Important difference:**
   - `__dirname` vs `process.cwd()`:
     - `__dirname` = where the FILE is located
     - `process.cwd()` = where you RAN the node command from

3. **Q: How do you pass and access command-line arguments in Node.js?**
   - **A:**

   **Passing arguments:**
   ```bash
   node script.js arg1 arg2 arg3
   ```

   **Accessing in code:**
   ```javascript
   // process.argv is an array:
   // [0] - path to node executable
   // [1] - path to JavaScript file
   // [2+] - your arguments

   const args = process.argv;

   console.log(args[0]);  // /usr/local/bin/node
   console.log(args[1]);  // /path/to/script.js
   console.log(args[2]);  // arg1 (first custom argument)
   console.log(args[3]);  // arg2
   console.log(args[4]);  // arg3
   ```

   **Better approach - skip first two:**
   ```javascript
   const [,, firstName, lastName, age] = process.argv;

   console.log(`Name: ${firstName} ${lastName}`);
   console.log(`Age: ${age}`);
   ```

   **Or slice the array:**
   ```javascript
   const userArgs = process.argv.slice(2);
   console.log("All user arguments:", userArgs);
   ```

   **Real-world example:**
   ```javascript
   // deploy.js
   const [,, environment, version] = process.argv;

   if (!environment || !version) {
       console.log("Usage: node deploy.js <env> <version>");
       console.log("Example: node deploy.js production 1.2.0");
       process.exit(1);
   }

   console.log(`Deploying version ${version} to ${environment}...`);
   // ... deployment logic
   ```

   **Running it:**
   ```bash
   node deploy.js production 1.2.0
   # Deploying version 1.2.0 to production...
   ```

   **For complex CLI apps:** Use libraries like `yargs` or `commander` which provide advanced argument parsing (flags, options, defaults, validation).

4. **Q: What's the difference between running code in browser console vs Node.js REPL?**
   - **A:**

   | Feature | Browser Console | Node.js REPL |
   |---------|----------------|--------------|
   | **Environment** | Browser (Chrome, Firefox, etc.) | Node.js runtime |
   | **Access to** | DOM, Window APIs | File system, OS, server APIs |
   | **Global Object** | `window` | `global` |
   | **Purpose** | Debug web pages, manipulate DOM | Test server code, run scripts |
   | **Modules** | ES6 imports (with bundler) | `require()`, ES6 imports |
   | **File Access** | No (security) | Yes (`fs` module) |
   | **Network** | fetch (with CORS) | http/https (no CORS) |
   | **Use Case** | Frontend development | Backend development |

   **Example - Same code, different results:**
   ```javascript
   // Both environments:
   console.log(typeof console);  // 'object' in both

   // Browser console:
   console.log(typeof window);   // 'object'
   console.log(typeof document); // 'object'
   console.log(typeof process);  // 'undefined'

   // Node.js REPL:
   console.log(typeof window);   // 'undefined'
   console.log(typeof document); // 'undefined'
   console.log(typeof process);  // 'object'
   ```

---

### **🌐 Real-World Usage:**

**Where These Concepts Are Used:**

1. **Command-Line Tools (CLI)**
   - **npm, webpack, eslint** - all use `process.argv` to parse commands
   ```bash
   npm install express --save
   webpack --config webpack.prod.js
   eslint --fix src/
   ```
   - Tools read arguments to determine what action to perform

2. **Build Scripts & Automation**
   ```javascript
   // build.js
   const env = process.env.NODE_ENV || 'development';

   if (env === 'production') {
       // Minify, optimize, remove console.logs
   } else {
       // Include source maps, keep debugging info
   }
   ```

3. **Configuration Management**
   ```javascript
   // config.js
   const config = {
       db: {
           host: process.env.DB_HOST || 'localhost',
           port: process.env.DB_PORT || 5432,
           name: process.env.DB_NAME || 'myapp'
       },
       apiKey: process.env.API_KEY  // Sensitive data from env
   };
   ```

4. **File Operations**
   ```javascript
   // Always use __dirname for reliable paths
   const path = require('path');
   const fs = require('fs');

   const dataPath = path.join(__dirname, 'data', 'users.json');
   const data = fs.readFileSync(dataPath, 'utf8');
   ```

5. **Development vs Production Checks**
   ```javascript
   if (process.env.NODE_ENV === 'production') {
       // Use CDN, enable caching
   } else {
       // Use local assets, detailed error messages
   }
   ```

6. **Docker & Containerization**
   - Environment variables (`process.env`) used extensively
   - Pass configuration without hardcoding
   ```bash
   docker run -e NODE_ENV=production -e PORT=8080 myapp
   ```

---

### **⚡ Summary & Quick Revision:**

✅ **REPL** (`node` command) provides interactive JavaScript shell - perfect for quick testing; production code goes in `.js` files run with `node filename.js`!

✅ **Global objects in Node.js**: `global` (namespace), `process` (runtime info), `__dirname` (directory path), `__filename` (file path) - NOT available in browsers!

✅ **`process.argv`** array contains command-line arguments - always skip first two (node path, script path) to get your custom arguments!

✅ **Browser vs Node.js**: Both run JavaScript but different APIs - Browser has DOM/Window, Node.js has fs/http/process - know your environment! 🚀

---

**🎯 Practical Exercise:**
Create a greeting CLI tool:
```bash
node greet.js Akshay Saini
# Output: Hello Akshay Saini! Welcome to Node.js 🚀
```

Try implementing this using `process.argv`! 💻

---

**Next Episode Preview:** Episode-04 will dive into **module.exports & require** - the module system that lets you organize code into reusable, maintainable pieces! 🔥
