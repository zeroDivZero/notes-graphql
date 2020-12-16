# CUSTOM TYPE

```javascript
const typeDefs = `
  type Query {
    post: Post!
  }

  type Post {
    id: ID!
    title: String!
    body: String!
    published: Boolean!
  }
`

const resolvers = {
  Query: {
    post() {
      return {
        id: '789abc',
        title: 'Favorite Anime',
        body: 'Demon Slayer is my favorite anime right now.',
        published: true
      }
    }
  }
}
```
