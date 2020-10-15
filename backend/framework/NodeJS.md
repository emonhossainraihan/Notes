pajetix208@wwmails.com


When you connect to a traditional server, such as Apache, it will spawn a new thread to handle the request. In a language such as PHP or Ruby, any subsequent I/O operations (for example, interacting with a database) block the execution of your code until the operation has completed. That is, the server has to wait for the database lookup to complete before it can move on to processing the result. If new requests come in while this is happening, the server will **spawn new threads** to deal with them. This is potentially inefficient, as a large number of threads can cause a system to become sluggish—and, in the worst case, for the site to go down. The most common way to support more connections is to add more servers.

Node.js, however, is single-threaded. It’s also event-driven, which means that everything that happens in Node is in reaction to an event. For example, when a new request comes in (one kind of event) the server will start processing it. If it then encounters a blocking I/O operation, instead of waiting for this to complete, it will **register a callback** before continuing to process the next event. When the I/O operation has finished (another kind of event), the server will execute the callback and continue working on the original request. Under the hood, Node uses the **libuv library to implement this asynchronous** (that is, non-blocking) behavior.

## Living in a single-threaded world

JavaScript was conceived as a single-threaded programming language that ran in a browser. Being single-threaded means that only one set of instructions is executed at any time in the same process (the browser, in this case, or just the current tab in modern browsers).

In other words, everything runs in parallel except for our JavaScript code. Synchronous blocks of JavaScript code are always run one at a time.

## Are There Any Downsides?

The fact that Node runs in a single thread does impose some limitations. For example, **blocking I/O** calls should be avoided, **CPU-intensive operations** should be handed off to a worker thread, and **errors should always be handled correctly** for fear of crashing the entire process.


## Node.js multithreading: What are Worker threads, and why do they matter?

## CPU-intensive tasks

What happens if we need to do synchronous-intense stuff, such as doing complex calculations in memory in a large dataset? Then we might have a synchronous block of code that takes a lot of time and will block the rest of the code. JavaScript and Node.js were not meant to be used for CPU-bound tasks. Since JavaScript is single-threaded, this will freeze the UI in the browser and queue any I/O event in NodeJS.

```js
db.findAll('SELECT ...', function(err, results) {
  if (err) return console.error(err)
  
  // Heavy computation and many results
  for (const encrypted of results) {
    const plainText = decrypt(encrypted)
    console.log(plainText)
  }
})
```
We will get the results in the callback once they are available. Then, no other JavaScript code is executed until our callback finishes its execution.
So let’s split this into smaller chunks and use setImmediate(callback):

```js
const crypto = require('crypto')

const arr = new Array(200).fill('something')
function processChunk() {
  if (arr.length === 0) {
    // code that runs after the whole array is executed
  } else {
    console.log('processing chunk');
    // pick 10 items and remove them from the array
    const subarr = arr.splice(0, 10)
    for (const item of subarr) {
      // do heavy stuff for each item on the array
      doHeavyStuff(item)
    }
    // Put the function back in the queue
    setImmediate(processChunk)
  }
}

processChunk()

function doHeavyStuff(item) {
  crypto.createHmac('sha256', 'secret').update(new Array(10000).fill(item).join('.')).digest('hex')
}

// This is just for confirming that we can continue
// doing things
let interval = setInterval(() => {
  console.log('tick!')
  if (arr.length === 0) clearInterval(interval)
}, 0)
```

Now we process 10 items each time and call setImmediate(callback) so that if there’s something else the program needs to do, it will do it in between those chunks of 10 items. I’ve added a setInterval() for demonstrating exactly that.

As you can see, the code gets more complicated. And many times, the algorithm is a lot more complex than this, so it’s hard to know **where to put the setImmediate()** to find a good balance.

## Alert


Since **nextTick** is called at the end of the current operation, calling it recursively can end up blocking the event loop from continuing. **setImmediate** solves this by firing in the check phase of the event loop, allowing event loop to continue normally.

```js
   ┌───────────────────────┐
┌─>│        timers         │
│  └──────────┬────────────┘
│  ┌──────────┴────────────┐
│  │     I/O callbacks     │
│  └──────────┬────────────┘
│  ┌──────────┴────────────┐
│  │     idle, prepare     │
│  └──────────┬────────────┘      ┌───────────────┐
│  ┌──────────┴────────────┐      │   incoming:   │
│  │         poll          │<─────┤  connections, │
│  └──────────┬────────────┘      │   data, etc.  │
│  ┌──────────┴────────────┐      └───────────────┘
│  │        check          │
│  └──────────┬────────────┘
│  ┌──────────┴────────────┐
└──┤    close callbacks    │
   └───────────────────────┘
```

Notice that the check phase is immediately after the poll phase. This is because the poll phase and I/O callbacks are the most likely places your calls to setImmediate are going to run. So ideally most of those calls will actually be pretty immediate, just not as immediate as nextTick which is checked after every operation and technically exists outside of the event loop.

```js
function step(iteration) {
  if (iteration === 10) return;
  setImmediate(() => {
    console.log(`setImmediate iteration: ${iteration}`);
    step(iteration + 1); // Recursive call from setImmediate handler.
  });
  process.nextTick(() => {
    console.log(`nextTick iteration: ${iteration}`);
  });
}
step(0);
// output
//nextTick iteration: 0
//setImmediate iteration: 0
//nextTick iteration: 1
//setImmediate iteration: 1
```

nextTick can become blocking when used recursively whereas setImmediate will fire in the next event loop and setting another setImmediate handler from within one won't interrupt the current event loop at all, allowing it to continue executing phases of the event loop as normal.

## Solution?

Threads are still very lightweight in terms of resources compared to forked processes. And this is the reason why Worker threads were born!

>  fork the process: require('child_process') > using Worker threads: require('worker_threads')

Threads share the same memory space, so context switch is faster (basically, just the register contents are saved per thread). However, synchronizing between them and ensuring mutual exclusion (global data integrity) is under your own responsibility (as a programmer). Processes run in different memory spaces, so mutual exclusion is guaranteed by the OS, but context switch is slower of course.
Keeping average person in mind,

> On your computer, open Microsoft Word and web browser. We call these two processes.

## Understand Module System

> CommonJS, modules are loaded synchronously and processed in the order they occur

```js
console.log(module)
//result
Module {
  id: '.',
  exports: {},
  parent: null,
  filename: '/home/emon/Desktop/index.js',
  loaded: false,
  children: [],
  paths:
   [ '/home/jim/Desktop/node_modules',
     '/home/jim/node_modules',
     '/home/node_modules',
     '/node_modules' ] }

```
Assigning properties to `exports` also adds them to `module.exports`

Do you like surprises?

You have no choice - exports is not a reference to modules.exports all the time! If you assign anything to module.exports, exports is not no longer a reference to it, and exports loses all its power.

## Requiring Modules

Node uses two core modules for managing module dependencies. You can think of the require module as the command and the module module as the organizer of all required modules.

```js
function require(path) {
      return mod.require(path);
    }
```
When Node invokes that require() function with a local file path as the function’s only argument, Node goes through the following sequence of steps:

- Resolving: To find the absolute path of the file

- Loading: To determine the type of the file content

- Wrapping: To give the file its private scope. This is what makes both the require and module objects local to every file we require

- Evaluating: This is what the VM eventually does with the loaded code

- Caching: So that when we require this file again, we don’t go over all the steps another time

We require a module by loading the content of a file into memory. The module.exports object in every module is what the require function returns when we require that module. module fully loaded if we print its module object on the next cycle of the event loop using a setImmediate call. The whole process of requiring/loading a module is synchronous. That’s why we were able to see the modules fully loaded after one cycle of the event loop. This also means that we cannot change the exports object asynchronously. **What happens when module 1 requires module 2, and module 2 requires module 1?**

## Requiring a Folder

Modules don’t have to be files. An index.js file will be used by default when we require a folder, but we can control what file name to start with under the folder using the main property in package.json.  

## Resolve Only

If you want to only resolve the module and not execute it, you can use the require.resolve function. 

## JSON and C/C++ Addons

If a file extension was not specified, the first thing Node will try to resolve is a .js file. If it can’t find a .js file, it will try a .json file and it will parse the .json file if found as a JSON text file. After that, it will try to find a binary .node file. 

```bash
$ node
Welcome to Node.js v12.9.1.
Type ".help" for more information.
> require.extensions
[Object: null prototype] {
  '.js': [Function],
  '.json': [Function],
  '.node': [Function],
  '.mjs': [Function]
}
```

## Module Wrapping

Before compiling a module, Node wraps the module code in a function, which we can inspect using the wrapper property of the `module` module. This wrapper function has 5 arguments: **exports, require, module, filename, and dirname**. This is what makes them appear to look global when in fact they are specific to each module.

## The “require” Object

There is nothing special about require. It’s an object that acts mainly as a function that takes a module name or path and returns the module.exports object. We need a way to determine if the file is being run as a stand-alone script or if it is being required by other scripts. 

```js
if (require.main === module) {
  // The file is being executed directly (not with require)
}
```

## Module Caching

The cache registry is simply an object that has a property for every required module. Those properties values are the module objects used for each module.

## Node’s Streams

https://nodesource.com/blog/understanding-streams-in-nodejs/

## Node’s Child Processes

## Scaling Node.js Applications

Source: jscomplete

## [A deep dive into queues in Node.js](https://blog.logrocket.com/a-deep-dive-into-queues-in-node-js/) 

