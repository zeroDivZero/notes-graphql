# SETUP

[website](https://github.com/prisma-labs/graphql-yoga)

GraphQL server. NPM package. Easy setup, powerful enough for production.

```javascript
import { GraphQLServer } from 'graphql-yoga'

// type definitions (app schema)
const typeDefs = `
  type Query {
    appName: String!
    hello(name: String): String!
  }
`

// resolvers
const resolvers = {
  Query: {
    appName() {
      return 'This is my first GQL app!'
    },
    hello: (_, { name }) => `Welcome ${name || 'to GQL'}`
  }
}

const server = new GraphQLServer({ typeDefs, resolvers })
server.start(() => console.log('Server is running on localhost:4000'))
```

`typeDefs` defines operations and data structures. `resolvers` are implementations of operations.
