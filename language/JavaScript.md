[stanford: JavaScript](https://web.stanford.edu/class/cs98si/)

standalone JavaScript interpreters:

- **Rhino** - Implemented in Java, intended primarily for use as a scripting engine extension for the Java platform. Includes an interactive command-line console, which will be helpful for learning.
- **SpiderMonkey**- Implemented in C/C++, intended for use as the JavaScript engine in Firefox.
- **Node.js** - A standalone, evented, asynchronous JavaScript environment, based on V8 and can be used for building lightweight, real-time applications.

See also:

- **V8**- Implemented in C++, designed primarily as the Chrome browser's JavaScript implementation; well suited for embedding.
- **NarWhal** - a CommonJS platform, and its a framework based on Rhino.
- **Windows Script Host** - includes a JScript interpreter (ECMAScript based, very similar to modern JavaScript).

 জাভাস্ক্রিপ্টে অ্যারের কোন একটি এলিমেন্ট কে অ্যাক্সেস করতে আমরা কেন স্কয়ার ব্রাকেট [] ব্যবহার করি?
আসলে জাভাস্ক্রিপ্টে অ্যারে নামের কোন ডাটা টাইপ নেই, অ্যারে অবজেক্ট কে এক্সটেন্ড করে তার মধ্যে ডাটা সংরক্ষন করে।
জাভাস্ক্রিপ্টে অবজেক্ট এর কোন প্রপার্টি আমরা দুভাবে এক্সেস করতে পারি। যেমন:
let user = {
name: 'John Doe'
};
console.log(user.name); // John Doe
console.log(user['name']); // John Doe
আগেই বলেছি অ্যারে একটি বিশেষ অবজেক্ট যা উর্ধক্রম numeric key: value অনুসারে ডাটা স্টোর করে। যেমন:
let arr = ['John', 'Doe'];
বিহাইন্ড দ্যা সীনে ডাটা এভাবে স্টোর হয়,
let arr = {
0: 'John',
1: 'Doe'
};
অবজেক্টের ক্ষেত্রে numeric কী কে arr.0 এভাবে অ্যাক্সেস করতে পারব না, arr.0 এর পরিবর্তে আমাদের এভাবে লিখতে হবে arr[0]।
যেহেতু অ্যারে numeric key: value এভাবে ডাটা সংরক্ষণ করে, তাই আমরা অ্যারের কোন এলিমেন্ট অ্যাক্সেস করতে স্কয়ার ব্রাকেট [] ব্যবহার করি।
