# PROJECT STRUCTURE

## Schema

Instead of single const `typeDefs` multi-line string defining types, have it in separate file, like `schema.graphql`.

```graphql
type Query {
  users(query: String): [User!]!
  posts(query: String): [Post!]!
  comments: [Comment!]!
}

type Mutation {
  createUser(input: CreateUserInput!): User!
  deleteUser(id: ID!): User!
  createPost(input: CreatePostInput!): Post!
  deletePost(id: ID!): Post!
  createComment(input: CreateCommentInput): Comment!
  deleteComment(id: ID!): Comment!
}

input CreateUserInput {
  name: String!
  email: String!
  age: Int
}
# and so on
```

To load:

```js
const server = new GraphQLServer({
  typeDefs: './src/schema.graphql',
  resolvers
})
```

`nodemon` by default does not monitor changes in files of extension `graphql`. In `package.json`, change files `nodemon` monitors by adding option `--ext js,graphql`.

## Data Context

Create `db.js` to contain data. Useful whether using hardcoded test data or loading database.

```js
// ...
// define `users`, `posts`, `comments`
// ...

const db = {
  users,
  posts,
  comments
}

export default db
```

Instead of importing to use directly, set it on context because every resolver, which could be split to separate its own file, automatically gets it.

```js
import db from './db'

const server = new GraphQLServer({
  typeDefs: './src/schema.graphql',
  resolvers,
  context: {
    db
  }
})
```

Example usage in resolver:

```js
users(parent, args, { db }, info) {
  if (!args.query) {
    return db.users
  }

  return db.users.filter((user) => {
    return user.name.toLowerCase().includes(args.query.toLowerCase())
  })
}
```

## Resolvers

Create each of resolvers' root props in its own file, resulting in `Query.js`, `Mutation.js`, etc. Naming convention is to capitalize. E.g., have `Comment` resolvers in `Comment.js`:

```js
const Comment = {
  author(parent, args, { db }, info) {
    return db.users.find((user) => {
      return user.id === parent.author
    })
  },

  post(parent, args, { db }, info) {
    return db.posts.find((post) => {
      return post.id === parent.post
    })
  }
}

export default Comment
```

To import:

```js
import Query from './resolvers/Query'
import Mutation from './resolvers/Mutation'
import User from './resolvers/User'
import Post from './resolvers/Post'
import Comment from './resolvers/Comment'

const resolvers = {
  Query,
  Mutation,
  User,
  Post,
  Comment
}
```

User same pattern to break up resolver functions into their own files as necessary.
