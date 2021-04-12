# SUBSCRIPTION

Last of 3 GQL operations. Like query, but instead of client *pulling* data from server on-demand, uses web socket to establish open connection between client and server to *push* data to client when there is update.

Schema:

```graphql
type Subscription {
  count: Int!
}
```

If using `graphql-yoga`, need `PubSub` (publisher-subscriber):

```js
// ...
import { GraphQLServer, PubSub } from 'graphql-yoga'
// ...
import Subscription from './resolvers/Subscription'
// ...

const pubsub = new PubSub()  // passed to context to be used by resolvers

const server = new GraphQLServer({
  typeDefs: './src/schema.graphql',
  resolvers: {
    Query,
    Mutation,
    Subscription,
    User,
    Post,
    Comment
  },
  context: {
    db,
    pubsub
  }
})
server.start(() => console.log('Server is running on localhost:4000'))
```

Resolver `Subscription.js`:

```js
const Subscription = {
  count: {
    subscribe(parent, args, { pubsub }, info) {
      let count = 0

      setInterval(() => {
        count++
        pubsub.publish('count', { count })  // publish new value
      }, 1000)

      return pubsub.asyncIterator('count')  // set up channel
    }
  }
}

export default Subscription
```

Notice type of `count` is not simply `Int` but object with `subscribe()` function. Likewise, `publish()` takes object that matches schema to publish.
