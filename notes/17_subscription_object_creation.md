# SUBSCRIPTION - OBJECT CREATION

## Schema

Declare subscription operation:

```graphql
type Subscription {
  comment(postId: ID!): Comment!
  post: Post!
}
```

## Resolver

Define operation to set up publishing channel:

```js
const Subscription = {
  comment: {
    subscribe(parent, { postId }, { db, pubsub }, info) {
      const post = db.posts.find((post) => post.id === postId && post.published)

      if (!post) {
        throw new Error('Post not found')
      }

      return pubsub.asyncIterator(`comment ${postId}`)
    }
  },

  post: {
    subscribe(parent, args, { pubsub }, info) {
      return pubsub.asyncIterator('post')
    }
  }
}

export default Subscription

```

## Publish

Publish new value when ready:

```js
// in Mutation.js createComment(), after comment created:
pubsub.publish(`comment ${args.input.post}`, { comment })

// ...

// in Mutation.js createPost(), after post created:
if (post.published) {
  pubsub.publish('post', { post })
}
```

## Subscription

Subscribe to receive published values:

```graphql
subscription {
  comment(postId: "101") {
    text
    post {
      id
      title
    }
    author {
      id
      name
    }
  }
}
```

```graphql
subscription {
  post {
    id
    published
    title
    body
    author {
      id
      name
    }
  }
}
```
