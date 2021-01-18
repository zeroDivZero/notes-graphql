# ARRAY

## Return Array from Server

```javascript
const typeDefs = `
  type Query {
    grades: [Int!]!
  }
`

const resolvers = {
  Query: {
    grades(parent, args, ctx, info) {
      return [99, 80, 93]
    }
  }
}
```

Query:

```graphql
query {
  grades
}
```

## Send Array to Server

```javascript
const typeDefs = `
  type Query {
    add(numbers: [Float!]!): Float!
  }
`

// resolver
add(parent, args) {
  if (args.numbers.length == 0) {
    return 0
  }

  return args.numbers.reduce((accumulator, currentValue) => {
    return accumulator + currentValue
  })
}
```

Query:

```graphql
query {
  add(numbers: [1, 5, 10, 2])
}
```

## Custom Type

```javascript
const typeDefs = `
  type Query {
    users: [User!]!
  }

  type User {
    id: ID!
    name: String!
    email: String!
    age: Int
  }
`

// demo user data (typically retrieved from database)
const users = [
  {
    id: '1',
    name: 'Alex',
    email: 'alex@example.com',
    age: 35
  },
  {
    id: '2',
    name: 'Sarah',
    email: 'sarah@example.com'
  },
  {
    id: '3',
    name: 'Zack',
    email: 'zack@example.com'
  }
]

// resolver
users(parent, args, ctx, info) {
  return users
}
```

Query:

```graphql
query {
  users {
    id
    name
    email
    age
  }
}
```

## Filter or Sort

Typically useful to provide options to let user filter or sort array results.

```javascript
const typeDefs = `
  type Query {
    users(query: String): [User!]!
  }
`

// resolver
users(parent, args, ctx, info) {
  if (!args.query) {
    return users
  }

  return users.filter((user) => {
    return user.name.toLowerCase().includes(args.query.toLowerCase())
  })
}
```

Query:

```graphql
query {
  nameWithX: users(query: "x") {
    name
    email
  }
  
  nameWithZ: users(query: "z") {
    name
    email
  }

  nameWithA: users(query: "A") {
    name
    email
  }
}

# result
{
  "data": {
    "nameWithX": [
      {
        "name": "Alex",
        "email": "alex@example.com"
      }
    ],
    "nameWithZ": [
      {
        "name": "Zack",
        "email": "zack@example.com"
      }
    ],
    "nameWithA": [
      {
        "name": "Alex",
        "email": "alex@example.com"
      },
      {
        "name": "Sarah",
        "email": "sarah@example.com"
      },
      {
        "name": "Zack",
        "email": "zack@example.com"
      }
    ]
  }
}
```
