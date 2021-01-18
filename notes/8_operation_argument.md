# OPERATION ARGUMENT

Allows client to pass data to server.

Must specify in `typeDefs`:

```javascript
const typeDefs = `
  type Query {
    greeting(name: String): String!
  }
`
```

Each resolver function receives 4 args:

```javascript
greeting(parent, args, ctx, info) {
  if (args.name) {
    return `Hello, ${args.name}!`
  } else {
    return 'Hello!'
  }
},
```

`parent` useful when working with relational data. `args` contains args sent by client. `ctx` is context data (e.g., is user logged in). `info` has info about operation.

To query:

```graphql
query {
  greeting(name: "Jess")
}
```
