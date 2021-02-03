# INPUT TYPE

Arg list may be long. Define custom input type to group args. Also allows reusability.

```javascript
const typeDefs = `
  type Query {
  type Mutation {
    createUser(input: CreateUserInput!): User!
  }

  input CreateUserInput {
    name: String!
    email: String!
    age: Int
  }
```

All props must be scalars (no custom type like `User`, which may contain other custom type). Type naming convention: postfix `Input`.

Resolver:

```javascript
createUser(parent, args, ctx, info) {
  const emailTaken = users.some((user) => user.email === args.input.email)
  if (emailTaken) {
    throw new Error('Email taken.')
  }

  const user = {
    id: uuidv4(),
    ...args.input // using spread operator
  }

  users.push(user)

  return user
}
```

Mutation:

```graphql
mutation {
  createUser(input: {
    name: "Jess"
    email: "jess@test.com"
    age: 26
  }) {
    id
    name
    email
    age
  }
}
```
