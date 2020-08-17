
>Node.js is an event-based, non-blocking, asynchronous I/O runtime that uses Google’s V8 JavaScript engine and libuv library.

The **V8 engine** is the open-source JavaScript engine that runs in Google Chrome and other Chromium-based web browsers, including Brave, Opera, and Vivaldi. It was designed with performance in mind and is responsible for compiling JavaScript directly to native machine code that your computer can execute. However, when we say that Node is built on the V8 engine, we don’t mean that Node programs are executed in a browser. They aren’t. Rather, the creator of Node (Ryan Dahl) took the V8 engine and enhanced it with various features, such as a file system API, an HTTP library, and a number of operating system–related utility methods. This means that Node.js is a program we can use to execute JavaScript on our computers. In other words, it’s a **JavaScript runtime**.

Node.js, however, is single-threaded. It’s also event-driven, which means that everything that happens in Node is in reaction to an event. For example, when a new request comes in (one kind of event) the server will start processing it. If it then encounters a blocking I/O operation, instead of waiting for this to complete, it will register a callback before continuing to process the next event. When the I/O operation has finished (another kind of event), the server will execute the callback and continue working on the original request. Under the hood, Node uses the libuv library to implement this asynchronous (that is, non-blocking) behavior.

##  What is an Event-Driven Architecture?
**Decoupled systems that run in response to events**

An event-driven architecture uses events to trigger and communicate between decoupled services and is common in modern applications built with microservices. An event is a change in state, or an update, like an item being placed in a shopping cart on an e-commerce website. Events can either carry the state (the item purchased, its price, and a delivery address) or events can be identifiers (a notification that an order was shipped).

Event-driven architectures have three key components: event producers, event routers, and event consumers. A producer publishes an event to the router, which filters and pushes the events to consumers. Producer services and consumer services are decoupled, which allows them to be scaled, updated, and deployed independently.

## Are There Any Downsides?

The fact that Node runs in a single thread does impose some limitations. For example, blocking I/O calls should be avoided, [CPU-intensive operations should be handed off to a worker thread](https://blog.logrocket.com/node-js-multithreading-what-are-worker-threads-and-why-do-they-matter-48ab102f8b10/), and errors should always be handled correctly for fear of crashing the entire process.

Some developers also dislike the callback-based style of coding that JavaScript imposes (so much so that there’s even a [site](http://callbackhell.com/) dedicated to the horrors of writing asynchronous JavaScript). But with the arrival of native Promises, followed closely by async await,[flow control in modern JavaScript](https://www.sitepoint.com/flow-control-callbacks-promises-async-await/) has become easier than it ever was.

## Different Module Formats

As JavaScript originally had no concept of modules, a variety of competing formats have emerged over time. Here’s a list of the main ones to be aware of:

- The Asynchronous Module Definition (AMD) format is used in browsers and uses a define function to define modules.
- The CommonJS (CJS) format is used in Node.js and uses require and module.exports to define dependencies and modules. The npm ecosystem is built upon this format.
- The ES Module (ESM) format. As of ES6 (ES2015), JavaScript supports a native module format. It uses an export keyword to export a module’s public API and an import keyword to import it.
- The System.registerformat was designed to support ES6 modules within ES5.
- The Universal Module Definition (UMD) format can be used both in the browser and in Node.js. It’s useful when a module needs to be imported by a number of different module loaders.

>If you assign anything to `module.exports`, `exports` is not no longer a reference to it, and exports loses all its power.

## The Node Knowledge Challenge



- What is the relationship between Node and V8? Can Node work without V8?

- come when you declare a global variable in any Node file it’s not really global to all modules?

  - Every Node file gets its own IIFE (Immediately Invoked Function Expression) behind the scenes. All variables declared in a Node file are scoped to that IIFE.

- When exporting the API of a Node module, why can we sometimes use exports and other times we have to use module.exports?

- What is the Call Stack? Is it part of V8?

  - The Call Stack is definitely part of V8. It is the data structure that V8 uses to keep track of function invocations. 

- What is the Event Loop? Is it part of V8?

  - The event loop is provided by the libuv library. It is not part of V8. The Event Loop is the entity that handles external events and converts them into callback invocations. It is a loop that picks events from the event queues and pushes their callbacks into the Call Stack. It is also a multi-phase loop. The Event Loop sits in the middle between V8’s Call Stack and the different phases and callback queues and it acts like an organizer. When the V8 Call Stack is empty, the event loop can decide what to execute next.

- What is the difference between setImmediate and process.nextTick?

- What are the major differences between spawn, exec, and fork?

- How does the cluster module work? How is it different than using a load balancer?

- What will Node do when both the call stack and the event loop queue are empty?

  - If there is nothing left to execute it will simply exit. Use a setInterval function because that would create a permanent pending callback in the Event Loop.

- What are V8 object and function templates?

- What is libuv and how does Node use it?

- How can we do one final operation before a Node process exits? Can that operation be done asynchronously?

- Besides V8 and libuv, what other external dependencies does Node have?

  - The following are all separate libraries that a Node process can use: `http-parser`, `c-ares`, `OpenSSL` and `zlib`

- What’s the problem with the process uncaughtException event? How is it different than the exit event?

- What are the 5 major steps that the require function does?

- How can you check for the existence of a local module?

- What are circular modular dependencies in Node and how can they be avoided?

- What are the 3 file extensions that will be automatically tried by the require function?

- When creating an http server and writing a response for a request, why is the end() function required?

- When is it ok to use the file system *Sync methods?

  - However, if you are using synchronous methods inside a handler like an HTTP server on-request callback, that would simply be 100% wrong. Do not do that.

- How can you print only one level of a deeply nested object?

- How come top-level variables are not global?

- The objects exports, require, and module are all globally available in every module but they are different in every module. How?

- If you execute a JavaScript file that has the single line: console.log(arguments); with Node, what exactly will Node print?

- How can a module be both "requirable" by other modules and executable directly using the node command?

- What’s an example of a built-in stream in Node that is both readable and writable?

- What happens when the line cluster.fork() gets executed in a Node script?

- What’s the difference between using event emitters and using simple callback functions to allow for asynchronous handling of code?

- What’s the difference between the Paused and the Flowing modes of readable streams?

- How can you read data from a connected socket?

- The require function always caches the module it requires. What can you do if you need to execute the code in a required module many times?

- When working with streams, when do you use the pipe function and when do you use events? Can those two methods be combined?



