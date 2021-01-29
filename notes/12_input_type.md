# INPUT TYPE

Argument list may get too long. Can define custom input type to group args. Also allows reusability.

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

All properties must be scalar types (hence cannot directly use custom type like `User`, which may contain other custom type). Naming convention is to have postfix `Input`.

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
