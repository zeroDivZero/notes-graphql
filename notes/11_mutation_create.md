# MUTATION CREATE

```javascript
const typeDefs = `
  type Mutation {
    createUser(name: String!, email: String!, age: Int): User!
  }
`
```

Resolver:

```javascript
import { v4 as uuidv4 } from 'uuid'
const resolvers = {
  Mutation: {
    createUser(parent, args, ctx, info) {
      const emailTaken = users.some((user) => user.email === args.email)
      if (emailTaken) {
        throw new Error('Email taken.') // passed back to client
      }

      const user = {
        id: uuidv4(), // using package uuid
        name: args.name,
        email: args.email,
        age: args.age
      }

      users.push(user)

      return user
    }
  }
}
```

Mutation:

```graphql
mutation {
  createUser(name: "John", email: "test@example.com") {
    id
    name
    email
    age
  }
}
```

Response:

```json
{
  "data": {
    "createUser": {
      "id": "abf260ef-0bc6-48b3-ab64-37614e816076",
      "name": "John",
      "email": "test@example.com",
      "age": null
    }
  }
}
```

If attempting to run again:

```json
{
  "data": null,
  "errors": [
    {
      "message": "Email taken.",
      "locations": [
        {
          "line": 2,
          "column": 3
        }
      ],
      "path": [
        "createUser"
      ]
    }
  ]
}
```
