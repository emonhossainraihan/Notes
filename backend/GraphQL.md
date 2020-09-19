- API documentation is essentially the reference manual for an API – It tells API consumers how to use the API. API documentation is meant for humans, usually developers, to read and understand. Providing documentation that is well-designed, comprehensive, and easy to follow is crucial when it comes to ensuring developers have a great experience with the API.

- An API specification explains how the API behaves and what to expect from the API.

- An API definition is similar to an API specification in that it provides an understanding of how an API is organized and how the API functions. But the API definition is aimed at machine consumption instead of human consumption of APIs.

The documentation tells developers and other API consumers how to use the API. After all, how can your API be successful if developers don’t know how to use it?

An API specification provides a broad understanding of the functionality of the API and the expected results. The specification is largely about the design of the API or your design philosophy. API design and functionality are key factors when choosing to integrate an API with an application.

And finally, the API definition is about machine consumption of an API and providing a machine-readable format for use by automated tools like automatic API documentation and SDK generators.



স্পেসিফিকেশন মানি কি? 

## API কী? খায় না মাথায় দেয়?

API এর ফুল ফর্ম হলো Application Programming Interface. 

> An API(application programming interface) simplifies programming by abstracting the underlying implementation and only exposing objects or actions the developer needs

সোজা বাংলায় যার মাধ্যমে একটা প্রোগ্রাম অন্য প্রোগ্রামের সাথে কথা বলে। সার্ভার ক্লায়েন্ট ব্যাপারটার সাথে মিল পাওয়া যাচ্ছ কি?  
কিন্তু যেকোনো ভাষার জন্য রয়েছে ব্যাকারণ যা দিয়ে আমরা ভাষাটা ঠিক ভাবে বলা শিখি যদিও ব্যাকারণ  
এর আগেই ভাষা নিজের মত করে তৈরি হয় আর পরিবর্তন হতে থাকে। তাই নতুনদের কাছে যেন ব্যাপারটা পরিষ্কার থাকে তার জন্যই তো ব্যাকারণ। 
এই ব্যাকারণ এর সাথে তুলনা করতে পারেন এপিআই স্পেসিফিকেশন কে| যখনি কোন এপিআই নিয়ে কাজ করবেন তখন দেখবেন এপিআই বানানোর সময় আপনাকে 
একটা আর্কিটেকচার স্টাইল ফলো করে বানাতে হচ্ছে। যেমন রেস্ট এপিআই এর জন্য OpenAPI, RAML, API Blueprint। জাগগে এইসব বাদ দিয়ে গ্রাফকিউএল নিয়ে কথা বলা যাক। 

## গ্রাফকিউএল কি?

> GraphQL is a speciﬁcation for an API query language and a server engine capable of executing such queries

গ্রাফকিউএল আমাদের এপিআই নিয়ে নতুন করে চিন্তাভাবনা শিখায়। যেমন ধরুন রেস্ট এপিআইতে সার্ভার ডিফাইন এন্ডপয়েন্টে গিয়ে ইউজার তার প্রয়োজনীয় ডেটা পায়।
মানি আপনি কিরূপ ডেটা পাবেন এবং কতোটুকু পাবেন তা আগেই সার্ভারে সেটআপ করা থাকে। আপনি ক্লায়েন্ট হিসেবে চাইলেও এটাকে পরিবর্তন করতে পারবেন নাহ। 
সোজা বাংলায় নির্দিষ্ট এন্ড পয়েন্ট থেকে আপনি কেবল নির্দিষ্ট ডেটাই পাবেন। 
অপরদিকে গ্রাফকিউএল (Graph**QL**) হলো একটা কুয়েরি ল্যাঙ্গুয়েজ (**Q**uery **L**anguage) যা কিনা সার্ভার এ কি কি ডেটা আছে আপনাকে বলে দিবে আপনি শুধু ক্লায়েন্ট হিসেবে আপনার কি রকম ডেটা 
লাগবে বলে দিবেন ব্যাস কাজ খতম। 
আসলেই কি এতো সহজ ব্যাপারটা 🤔 দেখা যাক। একটা কথা শুরুতেই বলে রাখি যা আমার অনেক ভালো লেগেছে

> An API isn't useful if it isn't predictable 

ব্যাপারটা রিলেটেড করাই দাড়ান। 

## URL Routes vs GraphQL Schema

রেস্ট এপিআইতে হয়কি আপনি কোথায় গেলে কি পাবেন সেটা একটা এন্ড পয়েন্ট লিস্ট আকারে বলে দেয়। উদাহরণ স্বরূপ

```
GET /users
GET /users/:id
```

/users এ গেলে আপনি সব ইউজার এর ডেটা দেখতে পারবেন আর যদি /users/:id তে গেলে নির্দিষ্ট ইউজারের ডেটা যার আইডির সাথে উপরের আইডির মিল আছে।
ব্যাপারটা লিনিয়ার ভাবে হচ্ছে তাই নাহ। তাহলে গ্রাফকিউএলে সেম জিনিসটা কিভাবে করবো? মনে আছে তো আমরা শুধু বলে দিবো আমাদের কি রূপ ডাটা দরকার। 

```graphql
query{
    users{
        id,
        name
    }
}
```

ওয়েট ওয়েট ওয়েট। আমাদের এপিআই কি ভাবে জানবে এইভাবে ইউজারদের দেখতে চাওয়াটা ভেলিড? 
এখানেই গ্রাফকিউএল প্রথম বাজিটা মেরে দেয়। গ্রাফকিউএলের আছে নিজস্ব টাইপ সিস্টেম। কি রকমের ডেটা চাওয়া ভেলিড আর কি রকমেরটা ভেলিড নাহ তা আগে
থেকেই সেট করে দিতে হয় যাতে করে ক্লায়েন্ট হিসেবে আপনি জানতে পারেন আপনি কিভাবে ডেটা পেতে পারেন। যেমন উপরের কুয়েরি অপারেশনটা ঠিক মতো কাজ করবে যদি আপনার কুয়েরি টাইপটা ঠিক মতো লিখা থাকে। এখন আবার আরেক প্রশ্ন আসে। লিখবো কিভাবে? গ্রাফকিউএলের আছে নির্দিষ্ট schema definition language (SDL) যার সিনট্যাক্স গুলো সুন্দর করে নির্ধারণ করা আছে গ্রাফকিউএল স্পেসিফিকেশনে। লিখে দেখাই কি বলুন? 

```graphql
type Query{
    users: [User]
}

type User{
    id:   ID!
    name: String
    ...
}
```

খেয়াল করে দেখবেন আমরা শুধু কুয়েরি অপারেশনটাই ডিফাইন করি নি সাথে যেই ডেটা আসবে তার ধরনও বলে দিয়েছি। ঠিক এই কারনেই আপনি ইচ্ছা মতো আপনার যে রকম ডেটা দরকার তা বলে দিতে পারেন। শুধু তাই নয় ফিরতি ডেটার চেহারাটা ঠিক আপনি যেভাবে চাইবেন তার মতো। 

```json
{
  "data": {
    "users": [
      {
        "id": "1",
        "name": "user1"
      },
      {
        "id": "2",
        "name": "user2"
      }
      
    ]
  }
}
```
