- API documentation is essentially the reference manual for an API – It tells API consumers how to use the API. API documentation is meant for humans, usually developers, to read and understand. Providing documentation that is well-designed, comprehensive, and easy to follow is crucial when it comes to ensuring developers have a great experience with the API.

- An API specification explains how the API behaves and what to expect from the API.

- An API definition is similar to an API specification in that it provides an understanding of how an API is organized and how the API functions. But the API definition is aimed at machine consumption instead of human consumption of APIs.

The documentation tells developers and other API consumers how to use the API. After all, how can your API be successful if developers don’t know how to use it?

An API specification provides a broad understanding of the functionality of the API and the expected results. The specification is largely about the design of the API or your design philosophy. API design and functionality are key factors when choosing to integrate an API with an application.

And finally, the API definition is about machine consumption of an API and providing a machine-readable format for use by automated tools like automatic API documentation and SDK generators.

গ্রাফকিউএল কি? 
> GraphQL is a speciﬁcation for an
API query language and a server engine capable of executing such
queries

স্পেসিফিকেশন মানি কি? 

API এর ফুল ফর্ম হলো Application Programming Interface. 

> An API(application programming interface) simplifies programming by abstracting the underlying implementation and only exposing objects or actions the developer needs

সোজা বাংলায় যার মাধ্যমে একটা প্রোগ্রাম অন্য প্রোগ্রামের সাথে কথা বলে। সার্ভার ক্লায়েন্ট ব্যাপারটার সাথে মিল পাওয়া যাচ্ছ কি?  
কিন্তু যেকোনো ভাষার জন্য রয়েছে ব্যাকারণ যা দিয়ে আমরা ভাষাটা ঠিক ভাবে বলা শিখি যদিও ব্যাকারণ  
এর আগেই ভাষা নিজের মত করে তৈরি হয় আর পরিবর্তন হতে থাকে। তাই নতুনদের কাছে যেন ব্যাপারটা পরিষ্কার থাকে তার জন্যই তো ব্যাকারণ। 
এই ব্যাকারণ এর সাথে তুলনা করতে পারেন এপিআই স্পেসিফিকেশন কে| যখনি কোন এপিআই নিয়ে কাজ করবেন তখন দেখবেন এপিআই বানানোর সময় আপনাকে 
একটা আর্কিটেকচার স্টাইল ফলো করে বানাতে হচ্ছে। যেমন রেস্ট এপিআই এর জন্য OpenAPI, RAML, API Blueprint। জাগগে এইসব বাদ দিয়ে গ্রাফকিউএল নিয়ে কথা বলা যাক। 
গ্রাফকিউএল আমাদের এপিআই নিয়ে নতুন করে চিন্তাভাবনা শিখায়। যেমন ধরুন রেস্ট এপিআইতে সার্ভার ডিফাইন এন্ডপয়েন্টে গিয়ে ইউজার তার প্রয়োজনীয় ডাটা পায়।
মানি আপনি কিরূপ ডাটা পাবেন এবং কতোটুকু পাবেন তা আগেই সার্ভারে সেটআপ করা থাকে। আপনি ক্লায়েন্ট হিসেবে চাইলেও এটাকে পরিবর্তন করতে পারবেন নাহ। 
সোজা বাংলায় নির্দিষ্ট এন্ড পয়েন্ট থেকে আপনি কেবল নির্দিষ্ট ডাটাই পাবেন। 
অপরদিকে গ্রাফকিউএল হলো একটা কুয়েরি ল্যাঙ্গুয়েজ যা কিনা সার্ভার এ কি কি ডাটা আছে আপনাকে বলে দিবে আপনি শুধু ক্লায়েন্ট হিসেবে আপনার কি রকম ডাটা 
লাগবে বলে দিবেন ব্যাস কাজ খতম। 
আসলেই কি এতো সহজ ব্যাপারটা 😂 দেখা যাক। একটা কথা আমার অনেক ভাল্লাগে তা হল 

> An API isn't useful if it isn't predictable 

ব্যাপারটা রিলেটেড করাই দাড়ান। রেস্ট এপিআইতে হয়কি আপনি কোথায় গেলে কি পাবেন সেটা একটা এন্ড পয়েন্ট লিস্ট আকারে বলে দেয়। উদাহরণ স্বরূপ

```
GET /users
GET /users/:id
POST /user/:id/comments
GET /users/:id/comments
```

ব্যাপারটা লিনিয়ার ভাবে হচ্ছে তাই নাহ। তাহলে গ্রাফকিউএলে সেম জিনিসটা কিভাবে করবো? মনে আছে তো আমরা শুধু বলে দিবো আমাদের কি রূপ ডাটা দরকার। 

```graphql
query{

}
```

ওয়েট ওয়েট ওয়েট। আমাদের এপিআই কি ভাবে জানবে ইউজার এর সাথে কমেন্ট ডাটা দেখতে চাওয়াটা ভেলিড? 
এখানেই গ্রাফকিউএল প্রথম বাজিটা মেরে দেয়। গ্রাফকিউএলের আছে নিজস্ব টাইপ সিস্টেম। কি রকমের ডেটা চাওয়া ভেলিড আর কি রকমেরটা ভেলিড নাহ তা আগে
থেকেই সেট করে দিতে হয় যাতে করে ক্লায়েন্ট হিসেবে আপনি জানতে পারেন আপনি কিভাবে ডেটা পেতে পারেন।
