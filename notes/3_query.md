# QUERY

1 of 3 major operations. To fetch data. Queries written in GQL query language.

Query:

```graphql
query {
  hello
  course
}
```

Returned data (in JSON):

```json
{
  "data": {
    "hello": "Hello world!",
    "course": "GraphQL"
  }
}
```

If invalid query:

```json
{
  "error": "Response not successful: Received status code 400"
}
```

## Nested

Query:

```graphql
query {
  hello
  course
  courseInstructor
  me {
    id
    name
    email
  }
}
```

Result:

```json
{
  "data": {
    "hello": "Hello world!",
    "course": "GraphQL",
    "courseInstructor": "John Smith",
    "me": {
      "id": "dc44bec2-3299-4542-a5fa-77c676fdfaf3",
      "name": "John",
      "email": "john@example.com"
    }
  }
}
```

### Array

Assume `users: [User!]!`:

Query:

```graphql
query {
  users {
    name
    email
  }
}
```

Result:

```json
{
  "data": {
    "users": [
      {
        "name": "John",
        "email": "john@example.com"
      },
      {
        "name": "Sarah",
        "email": "sarah@example.com"
      },
      {
        "name": "Michael",
        "email": "michael@example.com"
      }
    ]
  }
}
```

## Arguments

```graphql
{
  repository(name: "graphql", owner: "facebook") {
    id
    description
    homepageUrl
  }
}
```

Result:

```json
{
  "data": {
    "repository": {
      "id": "MDEwOlJlcG9zaXRvcnkzODM0MjIyMQ==",
      "description": "GraphQL is a query language and execution engine tied to any backend service.",
      "homepageUrl": "https://spec.graphql.org"
    }
  }
}
```
