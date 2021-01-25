# RELATIONAL DATA

To set up relationship between two types, add field in one type to point to other.

E.g., each `Post` has 1 author (`User`). In `typeDefs`:

```graphql
  type Post {
    id: ID!
    title: String!
    body: String!
    published: Boolean!
    author: User!
  }
```

In data, `author` field points to `User.id`:

```javascript
  {
    id: '101',
    title: 'Romeo & Juliet',
    body: 'It is a tragic love story.',
    published: true,
    author: '1'
  },
```

Since one of `Post`'s fields is not scalar type, resolver needs new property at root (parallel to `Query`) to handle it:

```javascript
const resolvers = {
  Query: {
    // omitted
  },
  Post: {
    author(parent, args, ctx, info) {
      // parent is Post object
      return users.find((user) => {
        return user.id === parent.author
      })
    }
  }
}
```

## Array

`User` can have multiple `Post`s:

```graphql
  type User {
    id: ID!
    name: String!
    email: String!
    age: Int
    posts: [Post!]!
  }
```

Note there's no need to add field to data. Only need to update resolver:

```javascript
const resolvers = {
  Query: {
    // omitted
  },
  Post: {
    // omitted
  },
  User: {
    posts(parent, args, ctx, info) {
      return posts.filter((post) => {
        return post.author === parent.id
      })
    }
  }
}
```
