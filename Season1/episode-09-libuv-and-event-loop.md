## Episode-09 | libuv & Event Loop

### **Lecture Title:**
libuv & Event Loop - The Complete Event Loop Mechanism Explained

---

### **🔥 Core Concept:**
This is THE episode that brings everything together! 🎯 After learning about libuv (Episode 06) and seeing async code in action (Episode 07), now Akshay bhaiya dives **deep into the Event Loop** - the heart that makes Node.js tick. You'll understand the **complete event loop architecture**, all its **phases**, and how callbacks are executed in a specific order.

This episode answers the ultimate question: **"How does Node.js handle thousands of concurrent operations on a single thread?"** You'll see the event loop diagram, understand each phase (timers, I/O callbacks, poll, check, close), learn about **microtasks vs macrotasks**, and finally understand why code executes in the order it does.

**Why it's important:**
- **Event loop IS the core of Node.js** - understanding it is non-negotiable for serious developers
- Most common advanced interview question: "Explain the Node.js event loop in detail"
- Helps you predict execution order of complex async code
- Essential for debugging performance issues and race conditions
- Explains why certain patterns are faster than others
- Foundation for writing high-performance, scalable Node.js applications

---

### **💡 Key Definitions:**

- **Event Loop**: The mechanism that takes callbacks from the event queue and executes them in the call stack when the stack is empty.

- **Phases**: The event loop has 6 distinct phases, each with a specific purpose and callback queue.

- **Timers Phase**: Executes callbacks scheduled by `setTimeout()` and `setInterval()`.

- **Pending Callbacks Phase**: Executes I/O callbacks deferred to the next loop iteration (system operations like TCP errors).

- **Idle, Prepare Phase**: Internal operations used by Node.js (not for user code).

- **Poll Phase**: Retrieves new I/O events, executes I/O-related callbacks (most application callbacks execute here).

- **Check Phase**: Executes callbacks scheduled by `setImmediate()`.

- **Close Callbacks Phase**: Executes close event callbacks (e.g., `socket.on('close')`).

- **Microtasks**: High-priority tasks that execute between event loop phases (`process.nextTick()`, `Promise.then()`).

- **Macrotasks**: Regular tasks that execute in their respective event loop phases (timers, I/O, setImmediate).

- **nextTick Queue**: Highest priority queue - executes before any event loop phase begins.

- **Promise Queue**: Microtask queue for Promise callbacks - executes after nextTick but before phases.

---

### **⚙️ Step-by-Step Explanation:**

**Complete Event Loop Architecture:**

1. **The Big Picture - Event Loop Cycle**
   ```
   ┌───────────────────────────┐
   │   Start Event Loop        │
   │   (while there are tasks) │
   └─────────────┬─────────────┘
                 │
                 ▼
   ┌─────────────────────────────────────┐
   │  1. TIMERS PHASE                    │
   │  Execute setTimeout/setInterval     │
   └─────────────┬───────────────────────┘
                 │
                 ▼
   ┌─────────────────────────────────────┐
   │  2. PENDING CALLBACKS PHASE         │
   │  Execute I/O callbacks (TCP errors) │
   └─────────────┬───────────────────────┘
                 │
                 ▼
   ┌─────────────────────────────────────┐
   │  3. IDLE, PREPARE PHASE             │
   │  Internal use only                  │
   └─────────────┬───────────────────────┘
                 │
                 ▼
   ┌─────────────────────────────────────┐
   │  4. POLL PHASE                      │
   │  ├─ Retrieve new I/O events         │
   │  ├─ Execute I/O callbacks           │
   │  └─ May block waiting for I/O       │
   └─────────────┬───────────────────────┘
                 │
                 ▼
   ┌─────────────────────────────────────┐
   │  5. CHECK PHASE                     │
   │  Execute setImmediate() callbacks   │
   └─────────────┬───────────────────────┘
                 │
                 ▼
   ┌─────────────────────────────────────┐
   │  6. CLOSE CALLBACKS PHASE           │
   │  Execute close event callbacks      │
   └─────────────┬───────────────────────┘
                 │
                 ▼
   ┌─────────────────────────────────────┐
   │  Check if loop should continue      │
   │  ├─ Pending timers?                 │
   │  ├─ Pending I/O?                    │
   │  ├─ Pending setImmediate?           │
   │  └─ If yes: Loop again              │
   │      If no:  Exit                   │
   └─────────────────────────────────────┘
   ```

2. **Microtasks Between Phases**
   ```
   After EACH phase:

   1. Execute ALL process.nextTick() callbacks
   2. Execute ALL Promise microtasks
   3. Move to next phase

   Example:

   Timers Phase → nextTick + Promises → Pending Callbacks Phase
                     ↑
              Microtasks executed
              between EVERY phase!
   ```

3. **Phase-by-Phase Breakdown**

   **PHASE 1: TIMERS**
   ```
   Purpose: Execute scheduled timers

   Callbacks from:
   - setTimeout()
   - setInterval()

   How it works:
   1. Check if any timers have expired
   2. Execute their callbacks (in order of expiration)
   3. Continue until no more expired timers

   Note: Doesn't guarantee exact timing!
   setTimeout(cb, 100) means "execute after AT LEAST 100ms"
   ```

   **PHASE 2: PENDING CALLBACKS**
   ```
   Purpose: Execute I/O callbacks deferred from previous iteration

   Callbacks from:
   - System operations (TCP errors, etc.)
   - Some OS callbacks

   Most I/O callbacks execute in POLL phase, not here!
   This is for callbacks that were deferred.
   ```

   **PHASE 3: IDLE, PREPARE**
   ```
   Purpose: Internal Node.js operations

   Not used by application code
   Used by libuv internally
   You can ignore this phase for practical purposes
   ```

   **PHASE 4: POLL (Most Important!)**
   ```
   Purpose: Retrieve and execute I/O callbacks

   This is where most application code executes!

   Operations:
   1. Calculate how long to block/wait
   2. Process events in poll queue
   3. Execute I/O callbacks:
      - fs.readFile() callbacks
      - HTTP request callbacks
      - Database query callbacks
      - Network I/O callbacks

   Special behavior:
   - If poll queue is empty:
     a. If setImmediate() is scheduled → go to CHECK phase
     b. If timers are scheduled → go back to TIMERS phase
     c. Otherwise → wait for new I/O events

   - If poll queue has callbacks:
     → Execute them until queue is empty or system limit
   ```

   **PHASE 5: CHECK**
   ```
   Purpose: Execute setImmediate() callbacks

   Callbacks from:
   - setImmediate()

   Why separate from timers?
   - setImmediate() designed for I/O callbacks
   - Executes immediately after poll phase
   - More predictable than setTimeout(0)
   ```

   **PHASE 6: CLOSE CALLBACKS**
   ```
   Purpose: Execute close event callbacks

   Callbacks from:
   - socket.on('close', callback)
   - process.on('exit', callback)

   Cleanup operations happen here
   ```

4. **Complete Execution Priority (with Microtasks)**
   ```
   Priority Order (Highest to Lowest):

   1. Synchronous Code (Call Stack)
      ↓
   2. process.nextTick() queue (All callbacks)
      ↓
   3. Promise Microtask queue (All callbacks)
      ↓
   4. TIMERS PHASE
      ↓
   5. process.nextTick() + Promises (between phases)
      ↓
   6. PENDING CALLBACKS PHASE
      ↓
   7. process.nextTick() + Promises (between phases)
      ↓
   8. IDLE, PREPARE PHASE
      ↓
   9. process.nextTick() + Promises (between phases)
      ↓
   10. POLL PHASE
       ↓
   11. process.nextTick() + Promises (between phases)
       ↓
   12. CHECK PHASE (setImmediate)
       ↓
   13. process.nextTick() + Promises (between phases)
       ↓
   14. CLOSE CALLBACKS PHASE
       ↓
   15. process.nextTick() + Promises (between phases)
       ↓
   16. Check if loop should continue or exit
   ```

5. **When Event Loop Exits**
   ```
   Event loop exits when:
   - No more pending timers
   - No more pending I/O operations
   - No more setImmediate() callbacks
   - No active handles or requests

   Example:
   console.log("Done");  // Exits immediately (no async ops)

   Example:
   setTimeout(() => console.log("Hi"), 1000);
   // Exits after 1 second (when timer completes)

   Example:
   setInterval(() => console.log("Hi"), 1000);
   // NEVER exits (infinite timer)

   Example:
   const server = http.createServer();
   server.listen(3000);
   // NEVER exits (server keeps listening)
   ```

---

### **💻 Code Example & Detailed Explanation:**

**Example 1: Complete Event Loop Phases Demo**

```javascript
// event-loop-phases.js

const fs = require('fs');

console.log("🚀 Start");

// 1. TIMERS PHASE - setTimeout
setTimeout(() => {
    console.log("⏰ setTimeout 0ms");
}, 0);

setTimeout(() => {
    console.log("⏰ setTimeout 100ms");
}, 100);

// 2. CHECK PHASE - setImmediate
setImmediate(() => {
    console.log("⚡ setImmediate");
});

// 3. POLL PHASE - I/O (fs.readFile)
fs.readFile(__filename, () => {
    console.log("📄 File I/O callback");

    // Inside I/O callback
    setTimeout(() => {
        console.log("   ⏰ setTimeout inside I/O");
    }, 0);

    setImmediate(() => {
        console.log("   ⚡ setImmediate inside I/O");
    });

    process.nextTick(() => {
        console.log("   🎯 nextTick inside I/O");
    });
});

// 4. MICROTASKS - process.nextTick
process.nextTick(() => {
    console.log("🎯 nextTick 1");
});

process.nextTick(() => {
    console.log("🎯 nextTick 2");
});

// 5. MICROTASKS - Promises
Promise.resolve().then(() => {
    console.log("✨ Promise 1");
});

Promise.resolve().then(() => {
    console.log("✨ Promise 2");
});

console.log("🏁 End");
```

**Output:**
```
🚀 Start
🏁 End
🎯 nextTick 1
🎯 nextTick 2
✨ Promise 1
✨ Promise 2
⏰ setTimeout 0ms
⚡ setImmediate
📄 File I/O callback
   🎯 nextTick inside I/O
   ⚡ setImmediate inside I/O
   ⏰ setTimeout inside I/O
⏰ setTimeout 100ms
```

**📝 Detailed Execution Flow:**

```
STEP 1: Synchronous Code (Call Stack)
─────────────────────────────────────
✅ console.log("🚀 Start")
✅ Register setTimeout(0)
✅ Register setTimeout(100)
✅ Register setImmediate()
✅ Start fs.readFile() (async, callback registered)
✅ Register process.nextTick() × 2
✅ Register Promise.then() × 2
✅ console.log("🏁 End")

Output: 🚀 Start, 🏁 End


STEP 2: Microtasks (Before Event Loop Phases)
──────────────────────────────────────────────
✅ Execute ALL nextTick callbacks first
   → console.log("🎯 nextTick 1")
   → console.log("🎯 nextTick 2")

✅ Execute ALL Promise callbacks
   → console.log("✨ Promise 1")
   → console.log("✨ Promise 2")

Output: 🎯 nextTick 1, 🎯 nextTick 2, ✨ Promise 1, ✨ Promise 2


STEP 3: Event Loop - TIMERS PHASE
──────────────────────────────────
✅ Check timers: setTimeout(0) has expired
✅ Execute callback
   → console.log("⏰ setTimeout 0ms")

❌ setTimeout(100) hasn't expired yet (skip)

Output: ⏰ setTimeout 0ms


STEP 4: Microtasks (After Timers Phase)
────────────────────────────────────────
(No microtasks queued, continue)


STEP 5: Event Loop - PENDING CALLBACKS PHASE
─────────────────────────────────────────────
(No pending callbacks, continue)


STEP 6: Event Loop - POLL PHASE
────────────────────────────────
✅ Check poll queue: No I/O events yet
✅ Check if setImmediate() scheduled: YES!
✅ Exit poll phase, go to CHECK phase


STEP 7: Event Loop - CHECK PHASE
─────────────────────────────────
✅ Execute setImmediate() callback
   → console.log("⚡ setImmediate")

Output: ⚡ setImmediate


STEP 8: Event Loop - CLOSE CALLBACKS PHASE
───────────────────────────────────────────
(No close callbacks, continue)


STEP 9: Next Event Loop Iteration
──────────────────────────────────
✅ Back to TIMERS phase
✅ Back to POLL phase
✅ File I/O completes! Callback ready
✅ Execute fs.readFile callback
   → console.log("📄 File I/O callback")
   → Register nextTick (inside I/O)
   → Register setImmediate (inside I/O)
   → Register setTimeout (inside I/O)

Output: 📄 File I/O callback


STEP 10: Microtasks (After I/O callback)
─────────────────────────────────────────
✅ Execute nextTick inside I/O
   → console.log("   🎯 nextTick inside I/O")

Output:    🎯 nextTick inside I/O


STEP 11: Event Loop - CHECK PHASE
──────────────────────────────────
✅ setImmediate (from inside I/O) is in check phase
   → console.log("   ⚡ setImmediate inside I/O")

Output:    ⚡ setImmediate inside I/O


STEP 12: Event Loop - Next Iteration TIMERS
────────────────────────────────────────────
✅ setTimeout (from inside I/O) executes
   → console.log("   ⏰ setTimeout inside I/O")

Output:    ⏰ setTimeout inside I/O


STEP 13: Wait for 100ms Timer
──────────────────────────────
... time passes ...
✅ setTimeout(100) expires
   → console.log("⏰ setTimeout 100ms")

Output: ⏰ setTimeout 100ms


STEP 14: Event Loop Check
──────────────────────────
❌ No more timers
❌ No more I/O
❌ No more setImmediate
✅ EXIT EVENT LOOP
```

**🎯 Key Observations:**

1. **Inside I/O callback**, order is guaranteed:
   - nextTick → setImmediate → setTimeout
   - This is ALWAYS true within I/O callbacks!

2. **At top level**, setTimeout(0) vs setImmediate order is NOT guaranteed:
   - Depends on system performance
   - Could be either order

3. **Microtasks always execute first** before any phase begins

---

**Example 2: setTimeout vs setImmediate (Edge Case)**

```javascript
// settimeout-vs-setimmediate.js

// Case 1: At top level (order NOT guaranteed)
console.log("=== Case 1: Top Level ===");

setTimeout(() => {
    console.log("setTimeout");
}, 0);

setImmediate(() => {
    console.log("setImmediate");
});

// Output: Could be either order!
// setTimeout, setImmediate  OR  setImmediate, setTimeout


// Case 2: Inside I/O callback (order GUARANTEED)
console.log("\n=== Case 2: Inside I/O ===");

const fs = require('fs');

fs.readFile(__filename, () => {
    setTimeout(() => {
        console.log("setTimeout inside I/O");
    }, 0);

    setImmediate(() => {
        console.log("setImmediate inside I/O");
    });
});

// Output: ALWAYS this order:
// setImmediate inside I/O
// setTimeout inside I/O
```

**📝 Why the Difference?**

**Case 1: Top Level (Unpredictable)**
```
When Node.js starts:
1. Parse and execute script (sync)
2. Enter event loop

Event loop timing:
- If event loop enters timers phase quickly:
  → setTimeout(0) executes first

- If system is slow to start event loop:
  → Timer might not be ready
  → Poll phase checks setImmediate
  → setImmediate executes first

Result: Non-deterministic!
```

**Case 2: Inside I/O (Predictable)**
```
When I/O callback executes:
- Currently in POLL phase
- Both setTimeout and setImmediate are queued

Event loop flow:
1. Finish current poll phase
2. Move to CHECK phase
   → Execute setImmediate ✅
3. Move to CLOSE phase
4. Loop back to TIMERS phase
   → Execute setTimeout ✅

Result: setImmediate ALWAYS first!
```

---

**Example 3: nextTick and Promise Queues**

```javascript
// microtasks-demo.js

console.log("Start");

// Queue multiple nextTick
process.nextTick(() => {
    console.log("nextTick 1");

    // Nested nextTick
    process.nextTick(() => {
        console.log("nextTick 1.1");
    });
});

process.nextTick(() => {
    console.log("nextTick 2");
});

// Queue multiple Promises
Promise.resolve().then(() => {
    console.log("Promise 1");

    // Nested Promise
    Promise.resolve().then(() => {
        console.log("Promise 1.1");
    });
});

Promise.resolve().then(() => {
    console.log("Promise 2");
});

// setTimeout
setTimeout(() => {
    console.log("setTimeout");

    process.nextTick(() => {
        console.log("nextTick in setTimeout");
    });

    Promise.resolve().then(() => {
        console.log("Promise in setTimeout");
    });
}, 0);

console.log("End");
```

**Output:**
```
Start
End
nextTick 1
nextTick 2
nextTick 1.1
Promise 1
Promise 2
Promise 1.1
setTimeout
nextTick in setTimeout
Promise in setTimeout
```

**📝 Execution Order Explained:**

```
1. Synchronous Code:
   → Start
   → End

2. Process all nextTick callbacks (queue 1):
   → nextTick 1 (adds nextTick 1.1 to queue)
   → nextTick 2

3. Process all nextTick callbacks (queue 2):
   → nextTick 1.1

4. Process all Promise callbacks (queue 1):
   → Promise 1 (adds Promise 1.1 to queue)
   → Promise 2

5. Process all Promise callbacks (queue 2):
   → Promise 1.1

6. Event Loop - Timers Phase:
   → setTimeout callback executes
   → Queues nextTick and Promise

7. Microtasks (after setTimeout):
   → nextTick in setTimeout
   → Promise in setTimeout
```

**🎯 Important Pattern:**
```
nextTick queue is processed COMPLETELY before Promise queue!

If nextTick keeps adding more nextTicks:
→ Can STARVE the event loop! (infinite recursion)

process.nextTick(function recurse() {
    process.nextTick(recurse);  // ⚠️ DANGEROUS!
});
// Event loop never progresses to other phases!
```

---

**Example 4: Real-World Event Loop Visualization**

```javascript
// real-world-example.js

const fs = require('fs');
const https = require('https');

console.log("🌐 Server starting...");

// Simulate server initialization
process.nextTick(() => {
    console.log("📋 Loading configuration...");
});

Promise.resolve().then(() => {
    console.log("🔗 Connecting to database...");
});

// File operation (I/O)
fs.readFile('./config.json', 'utf8', (err, data) => {
    console.log("📄 Config file loaded");

    // After loading config, do other tasks
    setImmediate(() => {
        console.log("⚙️  Applying configuration...");
    });

    process.nextTick(() => {
        console.log("✅ Config validated");
    });
});

// Network operation (I/O)
https.get('https://api.github.com/users/nodejs', (res) => {
    console.log("🌐 API request completed");

    let data = '';
    res.on('data', chunk => data += chunk);
    res.on('end', () => {
        console.log("📦 API data received");
    });
});

// Timer
setTimeout(() => {
    console.log("⏰ Health check timer");
}, 100);

console.log("🚀 Server initialization complete");
console.log("🔄 Event loop running...\n");

// Keep alive
setInterval(() => {
    // Server keeps running
}, 1000);
```

**Output (approximate):**
```
🌐 Server starting...
🚀 Server initialization complete
🔄 Event loop running...

📋 Loading configuration...
🔗 Connecting to database...
⏰ Health check timer
📄 Config file loaded
✅ Config validated
⚙️  Applying configuration...
🌐 API request completed
📦 API data received
```

**📝 Event Loop Flow:**

```
Iteration 1:
├─ Sync: Server starting, initialization complete
├─ nextTick: Loading configuration
├─ Promise: Connecting to database
└─ Timer (100ms): Health check timer

Iteration 2:
├─ Poll: File read completes
│  ├─ Callback: Config file loaded
│  ├─ nextTick: Config validated (executes immediately)
│  └─ setImmediate: Queued for check phase
├─ Check: Applying configuration
└─ Poll: API response received

Iteration 3+:
└─ setInterval keeps event loop alive forever
```

---

**Example 5: Debugging Event Loop Issues**

```javascript
// debug-event-loop.js

// ❌ PROBLEM: Server exits unexpectedly
function badServer() {
    const http = require('http');

    // Server created but not kept alive
    http.createServer((req, res) => {
        res.end('Hello');
    });

    // No .listen() called
    // No timers or pending operations
    // Event loop exits immediately!
}

// ✅ SOLUTION: Keep server alive
function goodServer() {
    const http = require('http');

    const server = http.createServer((req, res) => {
        res.end('Hello');
    });

    server.listen(3000);  // Keeps event loop alive!
    console.log("Server running on port 3000");
}

// ❌ PROBLEM: Memory leak from unclosed handles
function leakyCode() {
    setInterval(() => {
        // Heavy operation
        const data = new Array(1000000).fill('data');
        console.log('Running...');
    }, 1000);

    // Interval NEVER cleared
    // Event loop NEVER exits
    // Memory keeps growing!
}

// ✅ SOLUTION: Proper cleanup
function cleanCode() {
    const interval = setInterval(() => {
        console.log('Running...');
    }, 1000);

    // Cleanup on exit signal
    process.on('SIGTERM', () => {
        console.log('Cleaning up...');
        clearInterval(interval);
        process.exit(0);
    });
}

// ❌ PROBLEM: Blocking event loop
function blockingCode() {
    const http = require('http');

    http.createServer((req, res) => {
        // Synchronous heavy computation
        let result = 0;
        for (let i = 0; i < 10000000000; i++) {
            result += i;
        }
        res.end(`Result: ${result}`);
    }).listen(3000);

    // While computing:
    // - Other requests wait
    // - Event loop blocked
    // - Server appears frozen
}

// ✅ SOLUTION: Non-blocking approach
function nonBlockingCode() {
    const http = require('http');
    const { Worker } = require('worker_threads');

    http.createServer((req, res) => {
        // Offload to worker thread
        const worker = new Worker('./compute-worker.js');

        worker.on('message', result => {
            res.end(`Result: ${result}`);
        });

        // Event loop remains free!
    }).listen(3000);
}

// Run examples
goodServer();
cleanCode();
nonBlockingCode();
```

---

### **❌ Common Mistakes & Fixes:**

1. **Mistake: Starving Event Loop with process.nextTick()**
   - ❌ Problem: Recursive nextTick blocks event loop
   ```javascript
   // ❌ DANGEROUS - Infinite nextTick recursion
   function recursiveNextTick() {
       console.log("Running...");
       process.nextTick(recursiveNextTick);  // Queues immediately
   }

   recursiveNextTick();

   // What happens:
   // - nextTick queue NEVER empties
   // - Event loop NEVER moves to other phases
   // - Timers, I/O, everything BLOCKED
   // - Process becomes unresponsive!
   ```
   - ✅ Fix: Use setImmediate for recursion
   ```javascript
   // ✅ SAFE - setImmediate allows event loop to progress
   function recursiveImmediate() {
       console.log("Running...");
       setImmediate(recursiveImmediate);  // Queues for next iteration
   }

   recursiveImmediate();

   // What happens:
   // - Each call waits for next event loop iteration
   // - Other phases get their turn
   // - Timers, I/O still execute
   // - Process remains responsive
   ```
   - 💡 **Rule**: Never do infinite recursion with `nextTick`, use `setImmediate` instead!

2. **Mistake: Expecting Exact Timer Delays**
   - ❌ Problem: Thinking setTimeout guarantees exact timing
   ```javascript
   // ❌ WRONG ASSUMPTION
   const start = Date.now();

   setTimeout(() => {
       const elapsed = Date.now() - start;
       console.log(`Elapsed: ${elapsed}ms`);
       // Expected: 100ms
       // Reality: Could be 101ms, 105ms, even 150ms!
   }, 100);

   // Why delay might be longer:
   // - Event loop busy with other operations
   // - Timers phase not reached immediately
   // - System load affects timing
   ```
   - ✅ Fix: Understand timers are minimum delays, not guarantees
   ```javascript
   // ✅ CORRECT UNDERSTANDING
   setTimeout(() => {
       console.log("Executes after AT LEAST 100ms");
       // Might be 100ms, 105ms, 120ms, etc.
       // Never less than 100ms
       // Could be more based on event loop state
   }, 100);

   // For precise timing, use hrtime
   const start = process.hrtime.bigint();
   setTimeout(() => {
       const elapsed = process.hrtime.bigint() - start;
       console.log(`Precise elapsed: ${elapsed}ns`);
   }, 100);
   ```

3. **Mistake: Blocking Poll Phase with Synchronous I/O**
   - ❌ Problem: Using sync methods in production
   ```javascript
   // ❌ BAD - Blocks entire server
   const http = require('http');
   const fs = require('fs');

   http.createServer((req, res) => {
       // Synchronous file read - BLOCKS!
       const data = fs.readFileSync('large-file.txt');
       res.end(data);
   }).listen(3000);

   // When one request is reading file:
   // - Poll phase is blocked
   // - Other requests wait
   // - Timer callbacks delayed
   // - Server appears slow/frozen
   ```
   - ✅ Fix: Always use async methods
   ```javascript
   // ✅ GOOD - Non-blocking
   const http = require('http');
   const fs = require('fs');

   http.createServer((req, res) => {
       // Asynchronous file read
       fs.readFile('large-file.txt', (err, data) => {
           if (err) {
               res.statusCode = 500;
               res.end('Error');
               return;
           }
           res.end(data);
       });

       // Server immediately available for other requests!
   }).listen(3000);

   // Or use streams for large files:
   http.createServer((req, res) => {
       const stream = fs.createReadStream('large-file.txt');
       stream.pipe(res);  // Non-blocking streaming
   }).listen(3000);
   ```

4. **Mistake: Not Understanding setImmediate vs setTimeout(0)**
   - ❌ Problem: Using them interchangeably without knowing the difference
   ```javascript
   // ❌ UNCLEAR - Which should I use?
   setTimeout(() => {
       console.log("setTimeout 0");
   }, 0);

   setImmediate(() => {
       console.log("setImmediate");
   });

   // At top level: Order not guaranteed!
   ```
   - ✅ Fix: Use the right tool for the job
   ```javascript
   // ✅ Use setImmediate for I/O-related callbacks
   fs.readFile('file.txt', (err, data) => {
       setImmediate(() => {
           // Process file data in next iteration
           // Allows I/O in same iteration to complete
           processData(data);
       });
   });

   // ✅ Use setTimeout for actual delays
   setTimeout(() => {
       console.log("Execute after 1 second");
   }, 1000);

   // ✅ Use setTimeout(0) for breaking up long tasks
   function processLargeArray(arr, callback) {
       let index = 0;

       function processChunk() {
           // Process batch
           const batch = arr.slice(index, index + 1000);
           batch.forEach(process);

           index += 1000;

           if (index < arr.length) {
               setTimeout(processChunk, 0);  // Yield to event loop
           } else {
               callback();
           }
       }

       processChunk();
   }
   ```

---

### **❓ Interview Questions:**

1. **Q: Explain the Node.js event loop in detail with all phases.**
   - **A:**

   **The Node.js event loop has 6 phases that execute in order:**

   ```
   ┌─────────────────────────┐
   │      1. TIMERS          │ ← setTimeout, setInterval
   ├─────────────────────────┤
   │  2. PENDING CALLBACKS   │ ← Deferred I/O callbacks
   ├─────────────────────────┤
   │  3. IDLE, PREPARE       │ ← Internal use
   ├─────────────────────────┤
   │      4. POLL            │ ← Most I/O callbacks ⭐
   ├─────────────────────────┤
   │      5. CHECK           │ ← setImmediate
   ├─────────────────────────┤
   │  6. CLOSE CALLBACKS     │ ← socket.close(), etc.
   └─────────────────────────┘
          ↓
    Loop or Exit
   ```

   **Phase Details:**

   **1. TIMERS Phase:**
   - Executes callbacks from `setTimeout()` and `setInterval()`
   - Checks which timers have expired
   - Executes their callbacks in order of expiration
   - Note: Timers specify minimum delay, not exact

   **2. PENDING CALLBACKS Phase:**
   - Executes I/O callbacks deferred from previous iteration
   - Example: TCP errors, some system operations
   - Most I/O callbacks execute in POLL, not here

   **3. IDLE, PREPARE Phase:**
   - Internal libuv operations
   - Not used by application code
   - Can ignore for practical purposes

   **4. POLL Phase (Most Important!):**
   - Retrieves new I/O events
   - Executes I/O-related callbacks:
     - `fs.readFile()` callbacks
     - HTTP request callbacks
     - Database query callbacks
     - Network I/O
   - May block waiting for I/O if:
     - No timers scheduled
     - No setImmediate() scheduled
   - Two main operations:
     1. Calculate how long to wait/block
     2. Process events in poll queue

   **5. CHECK Phase:**
   - Executes callbacks from `setImmediate()`
   - Runs immediately after poll phase
   - Designed specifically for I/O callbacks

   **6. CLOSE CALLBACKS Phase:**
   - Executes close event callbacks
   - Example: `socket.on('close', callback)`
   - Cleanup operations

   **Microtasks (Between EVERY Phase):**
   1. Process ALL `process.nextTick()` callbacks
   2. Process ALL Promise microtasks
   3. Then move to next phase

   **Event Loop Continues While:**
   - Pending timers exist
   - Pending I/O operations exist
   - Pending setImmediate() callbacks exist

   **Event Loop Exits When:**
   - No more pending operations
   - No active handles or requests

2. **Q: What are microtasks and macrotasks? What's the difference?**
   - **A:**

   **Microtasks (High Priority):**
   - Execute BETWEEN event loop phases
   - Process ALL microtasks before moving to next phase
   - Types:
     1. `process.nextTick()` (highest priority)
     2. `Promise.then()`, `Promise.catch()`, `Promise.finally()`
     3. `queueMicrotask()`

   **Macrotasks (Regular Priority):**
   - Execute IN their respective event loop phases
   - One macrotask per phase iteration (system limit applies)
   - Types:
     1. `setTimeout()`, `setInterval()` (Timers phase)
     2. I/O callbacks (Poll phase)
     3. `setImmediate()` (Check phase)
     4. Close callbacks (Close phase)

   **Key Differences:**

   | Aspect | Microtasks | Macrotasks |
   |--------|-----------|------------|
   | **When** | Between phases | In phases |
   | **How many** | ALL in queue | Limited per phase |
   | **Priority** | Higher | Lower |
   | **Examples** | nextTick, Promises | setTimeout, I/O |
   | **Starvation risk** | Yes (nextTick) | No |

   **Execution Order Example:**
   ```javascript
   console.log("1. Sync");

   setTimeout(() => console.log("5. Macro"), 0);  // Macrotask

   Promise.resolve().then(() => console.log("3. Micro"));  // Microtask

   process.nextTick(() => console.log("2. NextTick"));  // Microtask (highest)

   setImmediate(() => console.log("6. Immediate"));  // Macrotask

   Promise.resolve().then(() => console.log("4. Micro 2"));  // Microtask

   console.log("1b. Sync end");

   // Output:
   // 1. Sync
   // 1b. Sync end
   // 2. NextTick       ← Microtask (highest priority)
   // 3. Micro          ← Microtask
   // 4. Micro 2        ← Microtask
   // 5. Macro          ← Macrotask (Timer phase)
   // 6. Immediate      ← Macrotask (Check phase)
   ```

   **Visual Flow:**
   ```
   Sync Code
      ↓
   ALL Microtasks (nextTick → Promises)
      ↓
   Macrotask (Timers phase)
      ↓
   ALL Microtasks
      ↓
   Macrotask (Poll phase)
      ↓
   ALL Microtasks
      ↓
   Macrotask (Check phase)
      ↓
   ALL Microtasks
   ```

   **Practical Implication:**
   ```javascript
   // ⚠️ Microtasks can block macrotasks
   Promise.resolve().then(function recursive() {
       console.log("Micro");
       Promise.resolve().then(recursive);  // Infinite microtasks
   });

   setTimeout(() => {
       console.log("Macro - NEVER EXECUTES!");
   }, 0);

   // Microtasks keep running, timer never executes!
   ```

3. **Q: Why does setImmediate execute before setTimeout(0) in I/O callbacks?**
   - **A:**

   **The Behavior:**
   ```javascript
   // Top level: Order NOT guaranteed
   setTimeout(() => console.log("setTimeout"), 0);
   setImmediate(() => console.log("setImmediate"));
   // Could be either order!

   // Inside I/O: setImmediate ALWAYS first
   fs.readFile(__filename, () => {
       setTimeout(() => console.log("setTimeout"), 0);
       setImmediate(() => console.log("setImmediate"));
       // Always: setImmediate, then setTimeout
   });
   ```

   **Why This Happens:**

   **Top Level (Non-Deterministic):**
   ```
   When script starts:
   1. Node.js initializes
   2. Executes script (sync code)
   3. Enters event loop

   Timing issue:
   - If event loop enters quickly:
     → Timers phase runs first
     → setTimeout(0) has expired
     → setTimeout executes first

   - If system is slow (high load):
     → Timer might not be ready
     → Poll phase checks setImmediate
     → setImmediate executes first

   Result: Race condition, order varies!
   ```

   **Inside I/O Callback (Deterministic):**
   ```
   Event Loop Phases when I/O callback executes:

   1. Currently in POLL phase
      ├─ I/O callback executing
      ├─ Queue setTimeout (goes to Timers phase)
      └─ Queue setImmediate (goes to Check phase)

   2. Finish current Poll phase

   3. Next phase: CHECK phase
      └─ Execute setImmediate ✅

   4. Next phase: Close Callbacks phase

   5. Loop back to TIMERS phase
      └─ Execute setTimeout ✅

   Result: setImmediate ALWAYS first!
   ```

   **Phase Order Guarantees It:**
   ```
   Event Loop Order:
   Timers → ... → Poll → Check → Close → (back to Timers)
                    ↑      ↑
                    │      │
                    │      setImmediate executes here
                    │
                    I/O callback here

   From Poll phase:
   - CHECK phase is next (setImmediate)
   - TIMERS phase is later (setTimeout)
   ```

   **Best Practice:**
   ```javascript
   // ✅ Use setImmediate for I/O-related callbacks
   // It's designed for this and behavior is predictable
   fs.readFile('file.txt', (err, data) => {
       setImmediate(() => {
           processData(data);  // Predictable, runs in next iteration
       });
   });

   // ✅ Use setTimeout for actual delays
   setTimeout(() => {
       console.log("After 1 second");
   }, 1000);
   ```

4. **Q: How can you check if the event loop is blocked? How to prevent it?**
   - **A:**

   **How to Detect Blocked Event Loop:**

   **1. Lag Monitoring:**
   ```javascript
   // Monitor event loop lag
   const start = Date.now();

   setInterval(() => {
       const delay = Date.now() - start - 1000;

       if (delay > 100) {
           console.warn(`Event loop lag: ${delay}ms`);
       }

       start = Date.now();
   }, 1000);

   // If interval is supposed to run every 1s but runs at 1.2s:
   // → Event loop is blocked by 200ms
   ```

   **2. Use Monitoring Libraries:**
   ```javascript
   // Using 'blocked' npm package
   const blocked = require('blocked');

   blocked((ms) => {
       console.log(`Event loop blocked for ${ms}ms`);
   }, { threshold: 100 });  // Alert if blocked > 100ms
   ```

   **3. Production Monitoring:**
   ```javascript
   // Using clinic.js or 0x profiler
   // Run: clinic doctor -- node app.js
   // Analyzes event loop performance

   // Or use APM tools:
   // - New Relic
   // - Datadog
   // - AppDynamics
   ```

   **How to Prevent Blocking:**

   **1. Avoid Synchronous Operations:**
   ```javascript
   // ❌ BAD
   const data = fs.readFileSync('file.txt');
   const hash = crypto.pbkdf2Sync('password', 'salt', 100000, 64, 'sha512');

   // ✅ GOOD
   fs.readFile('file.txt', callback);
   crypto.pbkdf2('password', 'salt', 100000, 64, 'sha512', callback);
   ```

   **2. Break Up Long Computations:**
   ```javascript
   // ❌ BAD - Blocks for seconds
   function processAll(items) {
       items.forEach(item => heavyOperation(item));
   }

   // ✅ GOOD - Process in batches
   function processInBatches(items, batchSize = 100) {
       let index = 0;

       function processBatch() {
           const end = Math.min(index + batchSize, items.length);

           for (let i = index; i < end; i++) {
               heavyOperation(items[i]);
           }

           index = end;

           if (index < items.length) {
               setImmediate(processBatch);  // Yield to event loop
           }
       }

       processBatch();
   }
   ```

   **3. Use Worker Threads for CPU Work:**
   ```javascript
   const { Worker } = require('worker_threads');

   function computeInWorker(data) {
       return new Promise((resolve, reject) => {
           const worker = new Worker('./compute.js', {
               workerData: data
           });

           worker.on('message', resolve);
           worker.on('error', reject);
       });
   }

   // CPU work happens in separate thread
   // Main event loop remains free
   ```

   **4. Use Child Processes:**
   ```javascript
   const { fork } = require('child_process');

   const child = fork('./heavy-work.js');
   child.send({ task: 'process', data: largeDataset });
   child.on('message', result => {
       console.log('Result:', result);
   });
   ```

   **5. Profile and Optimize:**
   ```bash
   # Profile your application
   node --prof app.js

   # Analyze profile
   node --prof-process isolate-*.log

   # Find hot paths and optimize them
   ```

---

### **🌐 Real-World Usage:**

**How Event Loop Knowledge Helps in Production:**

1. **High-Performance API Server**
   ```javascript
   const express = require('express');
   const app = express();

   // ✅ Non-blocking database queries
   app.get('/users', async (req, res) => {
       try {
           // Async DB query - doesn't block event loop
           const users = await db.query('SELECT * FROM users');
           res.json(users);
       } catch (err) {
           res.status(500).json({ error: err.message });
       }
   });

   // ✅ Stream large files (doesn't block)
   app.get('/download/:file', (req, res) => {
       const stream = fs.createReadStream(`files/${req.params.file}`);
       stream.pipe(res);  // Streaming, non-blocking
   });

   // ✅ Offload CPU work
   app.post('/process', async (req, res) => {
       const worker = new Worker('./processor.js');
       worker.postMessage(req.body);

       worker.on('message', result => {
           res.json({ result });
       });
   });
   ```

2. **Real-Time Chat (WebSockets)**
   ```javascript
   const io = require('socket.io')(server);

   io.on('connection', (socket) => {
       // Event loop handles thousands of connections

       socket.on('message', (msg) => {
           // Non-blocking message broadcast
           setImmediate(() => {
               io.emit('message', msg);
           });
       });
   });

   // Event loop efficiently manages all socket events
   ```

3. **Job Queue Processing**
   ```javascript
   const queue = require('bull')('tasks');

   queue.process(async (job) => {
       // Process job in batches to avoid blocking
       const { items } = job.data;

       for (let i = 0; i < items.length; i += 100) {
           const batch = items.slice(i, i + 100);
           await processBatch(batch);

           // Update progress
           job.progress(i / items.length * 100);

           // Yield to event loop
           await new Promise(resolve => setImmediate(resolve));
       }

       return { processed: items.length };
   });
   ```

4. **Microservices Communication**
   ```javascript
   // Service calls are non-blocking
   async function aggregateData(userId) {
       // All fetch in parallel (non-blocking)
       const [user, orders, preferences] = await Promise.all([
           fetch(`http://user-service/users/${userId}`),
           fetch(`http://order-service/orders?user=${userId}`),
           fetch(`http://pref-service/preferences/${userId}`)
       ]);

       // Event loop handled 3 concurrent I/O operations
       return { user, orders, preferences };
   }
   ```

---

### **⚡ Summary & Quick Revision:**

✅ **Event Loop has 6 phases**: Timers → Pending → Idle/Prepare → **Poll** → Check → Close - Poll phase is where most I/O callbacks execute!

✅ **Microtasks execute BETWEEN phases**: process.nextTick() (highest priority) → Promises → then move to next phase - ALL microtasks processed before phase transition!

✅ **Inside I/O callbacks**: setImmediate ALWAYS executes before setTimeout(0) - guaranteed by phase order (Poll → Check → Timers)!

✅ **Never block the event loop**: Use async methods, break up CPU work, use worker threads - event loop must stay responsive! 🚀

---

**🎯 Mental Model:**
```
Event Loop = Traffic Circle
Phases = Exits on the circle
Microtasks = Emergency vehicles (always go first)
Macrotasks = Regular cars (wait for their exit)

Emergency vehicles (nextTick, Promises) → Always prioritized
Regular cars (setTimeout, I/O) → Wait for their designated exit
```

---

**Next Episode Preview:** Episode-10 will explore the **Thread Pool in libuv** - how it manages concurrent operations, when it's used, and how to optimize thread pool size! 🔥

