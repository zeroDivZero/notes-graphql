# SCALAR TYPES

1. `String`
2. `Boolean`
3. `Int`
4. `Float`
5. `ID` - like string, but with differences, specifically for identifier

```javascript
const typeDefs = `
  type Query {
    id: ID!
    name: String!
    age: Int!
    employed: Boolean!
    gpa: Float
  }
`

const resolvers = {
  Query: {
    id() {
      return 'abc123' // could be 12, which would be converted to '12'
    },
    name() {
      return 'John Smith'
    },
    age() {
      return 35
    },
    employed() {
      return true
    },
    gpa() {
      return null
    }
  }
}
```
