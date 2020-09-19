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
এই ব্যাকারণ এর সাথে তুলনা করতে পারেন এপিআই স্পেসিফিকেশন কে। যখনি কোন এপিআই নিয়ে কাজ করবেন তখন দেখবেন এপিআই বানানোর সময় আপনাকে 
একটা আর্কিটেকচার স্টাইল ফলো করে বানাতে হচ্ছে। যেমন রেস্ট এপিআই এর জন্য OpenAPI, RAML, API Blueprint। জাগগে এইসব বাদ দিয়ে গ্রাফকিউএল নিয়ে কথা বলা যাক। 

## গ্রাফকিউএল কি?

প্রথমেই গ্রাফকিউএল নাম শুনা মাত্র যেসব ভুল ধারনা মাথায় আসে সেটা বলে নেই। আপনাদের মাথায় আসছে কিনা জানি নাহ আমার মাথায় এইগুলোই আসছে 

- গ্রাফকিউএল কি কোনো গ্রাফ ডেটাবেজ? নাহ 
- গ্রাফকিউএল কি কোনো লাইব্রেরি? নাহ 
- গ্রাফকিউএল কি কোনো গ্রাফ থিওরি? নাহ

তাহলে গ্রাফকিউএল জিনিসটা কি?

> GraphQL is a speciﬁcation for an API query language and a server engine capable of executing such queries

গ্রাফকিউএল আমাদের এপিআই নিয়ে নতুন করে চিন্তাভাবনা শিখায়। যেমন ধরুন রেস্ট এপিআইতে সার্ভার ডিফাইন এন্ডপয়েন্টে গিয়ে ইউজার তার প্রয়োজনীয় ডেটা পায়।
মানি আপনি কিরূপ ডেটা পাবেন এবং কতোটুকু পাবেন তা আগেই সার্ভারে সেটআপ করা থাকে। আপনি ক্লায়েন্ট হিসেবে চাইলেও এটাকে পরিবর্তন করতে পারবেন নাহ। একে বারেই পারবেন নাহ তা নাহ কিছু কুয়েরি প্যারামিটার ব্যবহার করে করতে পারবেন কিন্তু এক্ষেত্রে জিনিসটা খারাপ দেখায় আর হেন্ডেল করাও কষ্টসাধ্য যেমন `GET api/users?include=name&fields[posts]=id,content` এইভাবে পারামিটার দিয়ে কতটুকু ডেটা লাগবে তা ম্যানুপুলেট করা যায়।
সোজা বাংলায় নির্দিষ্ট এন্ড পয়েন্ট থেকে আপনি কেবল নির্দিষ্ট ডেটাই পাবেন। 
অপরদিকে গ্রাফকিউএল (Graph**QL**) হলো একটা কুয়েরি ল্যাঙ্গুয়েজ (**Q**uery **L**anguage) যা কিনা সার্ভার এ কি কি ডেটা আছে আপনাকে বলে দিবে আপনি শুধু ক্লায়েন্ট হিসেবে আপনার কি রকম ডেটা 
লাগবে বলে দিবেন ব্যাস কাজ খতম। 
আসলেই কি এতো সহজ ব্যাপারটা 🤔 দেখা যাক। একটা কথা শুরুতেই বলে রাখি যা আমার অনেক ভালো লাগে:

> An API isn't useful if it isn't predictable 

ব্যাপারটা রিলেটেড করাই দাড়ান। 

## ইউআরএল এন্ডপয়েন্ট এবং গ্রাফকিউএল স্ক্রিমা কি জিনিস?

রেস্ট এপিআইতে হয়কি আপনি কোথায় গেলে কি পাবেন সেটা একটা এন্ড পয়েন্ট লিস্ট আকারে বলে দেওয়া থাকে। উদাহরণ স্বরূপ:

```
GET /users
GET /users/:id
```

`/users` এ গেলে আপনি সব ইউজার এর ডেটা দেখতে পারবেন আর যদি `/users/:id` তে গেলে নির্দিষ্ট ইউজারের ডেটা যার আইডির সাথে উপরের আইডির মিল আছে।
ব্যাপারটা লিনিয়ার ভাবে হচ্ছে তাই নাহ। তাহলে গ্রাফকিউএলে সেম জিনিসটা কিভাবে করবো? মনে আছে তো আমরা শুধু বলে দিবো আমাদের কি রূপ ডাটা দরকার। 

```graphql
query{
    users{
        id,
        name
    }
}
```

ওয়েট!!! ওয়েট!! ওয়েট! আমাদের এপিআই কি ভাবে জানবে এইভাবে ইউজারদের দেখতে চাওয়াটা ভ্যালিড? 
এখানেই গ্রাফকিউএল প্রথম বাজিটা মেরে দেয়। গ্রাফকিউএলের আছে নিজস্ব টাইপ সিস্টেম। কি রকমের ডেটা চাওয়া ভ্যালিড আর কি রকমেরটা ভ্যালিড নাহ তা আগে
থেকেই সেট করে দিতে হয় যাতে করে ক্লায়েন্ট হিসেবে আপনি জানতে পারেন আপনি কিভাবে ডেটা পেতে পারেন। যেমন উপরের কুয়েরি অপারেশনটা ঠিক মতো কাজ করবে যদি আপনার কুয়েরি টাইপটা ঠিক মতো লিখা থাকে। এখন আবার আরেক প্রশ্ন আসে। লিখবো কিভাবে? গ্রাফকিউএলের আছে নির্দিষ্ট schema definition language (SDL) যার সিনট্যাক্স গুলো সুন্দর করে নির্ধারণ করা আছে গ্রাফকিউএল স্পেসিফিকেশনে। লিখে দেখাই কি বলুন? 

```graphql
type Query{
    users: [User]
}

type User{
    id:   ID!
    name: String
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

খেয়াল করে দেখুন রেস্ট বলুন আর গ্রাফকিউএল দুই জনই কিন্তু কোথায় গেলে কি পাওয়া যেতে পারে আগে থেকেই আন্দাজ করা যায়। পার্থক্য শুধু কিভাবে যাচ্ছেন আর কিভাবে API টাকে কল করছেন। এই ক্ষেত্রে একটা জিনিস নাহ বললেই নাহ। 

> REST API: "structure endpoints according to clients' data needs" 💡 GraphQL API: "Think in graphs, not endpoints."

## Route Handlers বনাম Resolvers

এই পর্যন্ত থাকলেই চলতো কিন্তু আসলে একটা API কল করলে সার্ভারে অনেক কিছু হয়। হয় কোনো ক্যালকুলেশন করে অথবা কোনো ডেটাবেজ থেকে কাঙ্ক্ষিত ডেটা গুলো ফেচ করে নয়তো অন্য কোনো আরেকটা API কে কল করতে পারে।  ব্যাপারটা বোঝানোর জন্য জাভাস্ক্রিপ্ট এর সাহায্য নিবো কিন্তু রেস্ট এবং গ্রাফকিউএল API অন্য যেকোনো ল্যাঙ্গুয়েজে ইমপ্লিমেন্ট করা সম্ভব। জিনিসগুলো সহজ রাখার জন্য এক্সপ্রেস/গ্রাফকিউএল সার্ভার কিভাবে ডেটাবেজ থেকে ডেটা ফেচ করে তা স্কিপ করা হলো। 

প্রথম উদাহরণ এক্সপ্রেস, একটা জনপ্রিয় লাইব্রেরী নোডজেএস এর উপর ভিত্তি করে তৈরি, দিয়ে দেখাই যা কিনা রেস্ট API সার্ভারে কিভাবে রিকুয়েস্টকে প্রসেস করতে হয় তার একটা ধারনা দিবে। 

```js
app.get('/posts', function (req, res) {
  // data fetching
  ...
  res.send(users)
})
```

সার্ভারটি একটি নির্দিষ্ট পাথ এ একটি নির্দিষ্ট রিকুয়েষ্টকে প্রসেস করে কিছু একটা রিটার্ন করে। এই ক্ষেত্রে ডেটাবেজ থেকে সব ইউজার এর ডেটা ফেচ করে একটা অ্যারে আকারে রির্টান করে। 

আচ্ছা এইবার গ্রাফকিউএলে ব্যাপারটা কিভাবে হয় দেখা যাক। 

```js
const resolvers = {
  Query: {
    hello: () => {
      // data fetching
      return users;
    },
  },
};
```

কি দেখলেন? কোনো একটা স্পেসিফিক ইউআরএল ভিত্তিক ফাংশন না লিখে আমরা গ্রাফকিউএলের একটা রুট টাইপের আন্ডারে ফাংশনটা ফিল্ড আকারে লিখেছি। কেন এমনটা করা? যেহেতু রেস্ট API এর মতো গ্রাফকিউএল API তে স্পেসিফিক ইউআরএল ভিত্তিক কাজ করা লাগে নাহ তাই এমনটা করা। ঠিক এই কারনেই আমরা নেস্টেট কুয়েরি করতে পারি। এমনকি নিজেদের মধ্যেই নিজেকে খুজতে পারি (আহা এই যেন নিজেকে নিজে ভালোবাসার মতো 😌) বিশ্বাস করছেন নাহ? দেখাচ্ছি ওয়েট তার জন্য শুধু স্ক্যালার টাইপ দিয়ে হবে নাহ একটা অবজেক্ট টাইপও সংযুক্ত করতে হবে। ধরুন ইউজার টাইপের মধ্যে একটা অবজেক্ট টাইপ পোস্ট দিয়ে দিলাম। আবার তাহলে গ্রাফকিউএল স্ক্রিমা টা কি হবে দেখাই দাড়ান।

```graphql
type Query{
    users: [User]
}

type User{
    id: ID
    name: String!
    posts: [Post]
}

type Post{
    id: ID
    content: String
    user: User
    userId: ID
}
```

একজন ইউজার কিভাবে পোস্ট তৈরি করে সেই বর্ননায় নাহ যাই শুধু কি রকম ডেটা আমরা রিকুয়েস্ট করতে পারবো সেটাই দেখি। কেউ আবার বইলেন নাহ এদের জন্য রিসলভার গুলো দেখান সেটা নিয়ে অন্য আরেকটা পোস্ট লিখার চেষ্টা করবো নে। কারন তখন রিসলভারের আর্গুমেন্ট ৪টা নিয়ে কথা বলতে হবে। যারা অনেক আগ্রহী এমনকি নাহ দেখানোর জন্য আমাকে গালি দিচ্ছেন তাদের জন্য অন্য আরেকটা আর্টিকেল ধরিয়ে দেই। এই [লিংকে গিয়ে](https://www.prisma.io/blog/graphql-server-basics-demystifying-the-info-argument-in-graphql-resolvers-6f26249f613a) দেখতে পারেন। বাচা গেল বাবা 😓। জাগগে কুয়েরিটা আর রিটার্ন ডেটাটা দেখে নেয়া যাক আগে।


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

```json
{
  "data": {
    "users": [
      {
        "id": "1",
        "name": "user1",
        "posts": [
          {
            "id": "1",
            "content": "Lorem ipsum dolor sit amet",
            "user": {
              "id": "1",
              "name": "user1"
            }
          },
          {
            "id": "2",
            "content": "Sed quis condimentum leo. Nulla non pretium lacus",
            "user": {
              "id": "1",
              "name": "user1"
            }
          }
        ]
      }
    ]
  }
}
```

বাবারে কি অদ্ভুত। আপনি চাইলে আরো নেস্টেট করতে পারেন। যেহেতু কুয়েরি গুলো স্পেসিফিক ইউআরএল ভিত্তিক নাহ তাই এমনটা করা অস্বাভাবিক নাহ। রিসলভার ব্যাপার গুলো ঠিকঠাক করে দেয়। 😁

আজ আর না। যাওয়ার আগে বলে দেই রেস্ট বলুন আর গ্রাফকিউএল দুটোই কিন্তু নিজস্ব কিছু ভালো খারাপ দিক রাখে। তাই কোনটা ভালো কোনটা খারাপ সেই কথা নাহ বলে দুটোই ভালো পেক্ষাপট অনুসারে এইটা বলা মনে হয় সঠিক হবে 😊। আসা করি গ্রাফকিউএল এপিআই নিয়ে আপনাদের সাধারন ধারনা হয়েছে। ইনশাআল্লাহ সামনে আরো বিস্তারিত লিখবো।

আসসালামু আলাইকুম এবং ধন্যবাদ সবাইকে আমার বাংলিশ টার্ম গুলো সহ্য করে শেষ পর্যন্ত পড়ার জন্য 😊





