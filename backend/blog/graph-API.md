## Type System

At the core of any GraphQL server is a powerful type system that helps to
express API capabilities. Practically, the type system of a GraphQL engine is
often referred to as the schema. GraphQL APIs rely on GraphQL schemas to expose data. GraphQL provides it owns scalar types 
(`Int`, `Float`, `String`, `Boolean`, and `ID`) and lets you create your own data types using the following keywords.

- `type` to define complex object types used by queries (read data)
- `input` to define input types used as arguments by mutations (create, update, delete data)


## Types & Fields

The fieldName: Type syntax allows us to give a return type to our ﬁelds. 

## Schema Roots

As you can see, a GraphQL schema can be deﬁned using types and ﬁelds to
describe its capabilities. However, how can clients even begin to query this
“Graph” of capabilities? It must have an entry point somewhere. This is where
“Schema Roots” come in. A GraphQL schema must always deﬁne a Query Root,
a type that deﬁnes the entry point to possibilities. 

A query root has to be deﬁned on a GraphQL schema, but two other types of
roots that can be deﬁned: the Mutation, and the Subscription roots. 

This query consists of different parts

- operation type 
- name 
- selection set 

## Variables
While we are on the subject of arguments, it is good to note that GraphQL
queries can also deﬁne variables to be used within a query. This allows clients
to send variables along with a query and have the GraphQL server execute it,
instead of including it directly in the query string itself:

```js
queryFetchProduct($id: ID!, $format: PriceFormat!) {
    product(id: $id) {
        price(format: $format) {
            name
        }
    }
}
```

Notice that we also gave the query an operation name, FetchProduct. A
client would send this query along with its variables, like this:

```
{
    "id": "abc",
        "format": {
        "displayCents": true,
            "currency": "USD"
    }
}
```

## Aliases
While the server dictates the canonical name of fields, clients may want to receive
these ﬁelds under another name. For this, they can use field aliases:

```js
query{
    abcProduct: product(id: "abc") {
        name
        price
    }
}
```
## Fragment
Each fragment contains the name of the fragment, to what type we are applying this fragment and the selection set.

```js
query getUsers {
    admins: users(role: admin) {
        ...userFields
    }
    accountants: users(role: accountant) {
        ...userFields
    }
}
fragment userFields on User {
    id
    firstName
    lastName
    phone
    username
}
```
## Abstract types
In the GraphQL specification we are able to use [two abstract types](https://www.apollographql.com/docs/apollo-server/schema/unions-interfaces/):

- interfaces
- unions

> The interface is a structure that enforces certain properties on the object or class that implements the corresponding interface

when applying interface we are basically adding additional node above our types, so that we are able to access them as one entity.
Let's say that we would like to access all the types from our schema as one single entity called Node.

![](https://atheros.ai/articles/graphql-interfaces-and-unions-how-to-design-graphql-schema/node_interface.webp)

https://atheros.ai/

## Mutations

Two things make mutation slightly different:
- Top-level ﬁelds under the mutation root are allowed to have side eﬀects /
make modiﬁcations.
- Top-level mutation ﬁelds must be executed serially by the server, while
other ﬁelds could be executed in parallel.

## Summary
With a query language for clients to express requirements, a type system for
servers to express possibilities, and an introspection system that allows clients to
discover these possibilities, GraphQL allows API providers to designs schemas
that clients will be able to consume in the way they want. 
