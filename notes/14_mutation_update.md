# MUTATION UPDATE

## Schema

Add to `Mutation`:

```graphql
updateUser(id: ID!, data: UpdateUserInput!): User!
```

Define input. Decide if fields should be optional to allow updating fewer fields at same time:

```graphql
input UpdateUserInput {
  name: String
  email: String
  age: Int
}
```

## Resolver

```js
updateUser(parent, args, { db }, info) {
  const { id, data } = args
  const user = db.users.find((user) => user.id === id)

  if (!user) {
    throw new Error(`User ${id} not found!`)
  }

  if (typeof data.email === 'string') {
    const emailTaken = db.users.some((user) => user.email === data.email)
    if (emailTaken) {
      throw new Error(`Email ${data.email} taken!`)
    }

    user.email = data.email
  }

  if (typeof data.name === 'string') {
    user.name = data.name
  }

  // can be null
  if (typeof data.age !== 'undefined') {
    user.age = data.age
  }

  return user
}
```

## Mutation

```graphql
mutation {
  updateUser(id: "1", data: {email: "zeke@example.com", age: null}) {
    id
    name
    email
    age
  }
}
```
