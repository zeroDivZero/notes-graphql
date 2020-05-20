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

Query self-documenting because GQL API exposes app schema, which describes all operations that can be performed.
