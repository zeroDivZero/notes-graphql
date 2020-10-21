# Playground

Graphical, interactive, in-browser GraphQL IDE.

In development, Apollo Server enables GraphQL Playground on same URL as GraphQL server (e.g. [http://localhost:4000/graphql](http://localhost:4000/graphql)) and automatically serves GUI to web browsers. When `NODE_ENV` is set to production, GraphQL Playground (and introspection) is disabled.

When typing, `Ctrl-Space` to see inline doc.

## Query Schema

Since GQL is introspective, can use query to get info about schema.

```graphql
{
  __schema {
    queryType {
      name
      description
      fields {
        name
        description
        isDeprecated
        deprecationReason
      }
    }
    mutationType {
      name
      description
    }
  }
}
```

## Query Types

Similarly, can query about type info.

```graphql
{
  __type(name: "Repository") {
    kind
    name
    description
  }
}
```
