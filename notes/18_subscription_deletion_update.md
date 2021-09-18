# SUBSCRIPTION - DELETION AND UPDATE

To distinguish mutations (creation, deletion, update), usually need to publish additional data. Convention to add suffix `SubscriptionPayload` to name of return type.

## Schema

```graphql
type Subscription {
  comment(postId: ID!): CommentSubscriptionPayload!
  post: PostSubscriptionPayload!
}

type PostSubscriptionPayload {
  mutation: String!
  data: Post!
}

type CommentSubscriptionPayload {
  mutation: String!
  data: Comment!
}
```

`mutation` can have value: `'CREATED'`, `'DELETED'`, `'UPDATED'`.

No change to resolver, which only sets up channel.

## Publish

```js
deleteComment(parent, args, { db, pubsub }, info) {
  const commentIndex = db.comments.findIndex((comment) => comment.id === args.id)

  if (commentIndex === -1) {
    throw new Error(`Comment ${args.id} not found!`)
  }

  const [comment] = db.comments.splice(commentIndex, 1)  // deconstruct

  pubsub.publish(`comment ${comment.post}`, {
    comment: {
      mutation: 'DELETED',
      data: comment
    }
  })

  return comment
}
```
