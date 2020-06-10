
>Node.js is an event-based, non-blocking, asynchronous I/O runtime that uses Google’s V8 JavaScript engine and libuv library.

The **V8 engine** is the open-source JavaScript engine that runs in Google Chrome and other Chromium-based web browsers, including Brave, Opera, and Vivaldi. It was designed with performance in mind and is responsible for compiling JavaScript directly to native machine code that your computer can execute. However, when we say that Node is built on the V8 engine, we don’t mean that Node programs are executed in a browser. They aren’t. Rather, the creator of Node (Ryan Dahl) took the V8 engine and enhanced it with various features, such as a file system API, an HTTP library, and a number of operating system–related utility methods. This means that Node.js is a program we can use to execute JavaScript on our computers. In other words, it’s a JavaScript runtime.

Node.js, however, is single-threaded. It’s alsoevent-driven, which means that everything that happens in Node is in reaction to an event. For example, when a new request comes in (one kind of event) the server will start processing it. If it then encounters a blocking I/O operation, instead of waiting for this to complete, it will register a callback before continuing to process the next event. When the I/O operation has finished (another kind of event), the server will execute the callback and continue working on the original request. Under the hood, Node uses thelibuvlibrary to implement this asynchronous (that is, non-blocking) behavior.

##  What is an Event-Driven Architecture?
**Decoupled systems that run in response to events**

An event-driven architecture uses events to trigger and communicate between decoupled services and is common in modern applications built with microservices. An event is a change in state, or an update, like an item being placed in a shopping cart on an e-commerce website. Events can either carry the state (the item purchased, its price, and a delivery address) or events can be identifiers (a notification that an order was shipped).

Event-driven architectures have three key components: event producers, event routers, and event consumers. A producer publishes an event to the router, which filters and pushes the events to consumers. Producer services and consumer services are decoupled, which allows them to be scaled, updated, and deployed independently.

## Are There Any Downsides?

The fact that Node runs in a single thread does impose some limitations. For example, blocking I/O calls should be avoided, [CPU-intensive operations should be handed off to a worker thread](https://blog.logrocket.com/node-js-multithreading-what-are-worker-threads-and-why-do-they-matter-48ab102f8b10/), and errors should always be handled correctly for fear of crashing the entire process.

Some developers also dislike the callback-based style of coding that JavaScript imposes (so much so that there’s evena [site](http://callbackhell.com/) dedicated to the horrors of writing asynchronous JavaScript). But with the arrival of native Promises, followed closely by async await,[flow control in modern JavaScript](https://www.sitepoint.com/flow-control-callbacks-promises-async-await/) has become easier than it ever was.
