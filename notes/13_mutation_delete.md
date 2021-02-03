# MUTATION DELETE

Consideration: Need to cascade.

```javascript
const typeDefs = `
  type Mutation {
    deleteUser(id: ID!): User!
  }
`
```

Resolver (ugly implementation of cascading):

```javascript
deleteUser(parent, args, ctx, info) {
  const userIndex = users.findIndex((user) => user.id === args.id)

  if (userIndex === -1) {
    throw new Error(`User ${args.id} not found!`)
  }

  const deletedUsers = users.splice(userIndex, 1)

  // remove posts by user
  posts = posts.filter((post) => {
    const match = post.author === args.id

    // remove comments of deleted post
    if (match) {
      comments = comments.filter((comment) => comment.post !== post.id)
    }

    return !match
  })

  // remove comments by user
  comments = comments.filter((comment) => comment.author !== args.id)

  return deletedUsers[0]
}
```

Mutation:

```graphql
mutation {
  deleteUser(id: "1") {
    id
    name
  }
}
```
