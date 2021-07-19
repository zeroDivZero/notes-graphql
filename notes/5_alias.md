# ALIAS

Cannot use same operation with different args at same time. Use aliases:

```graphql
{
  graphqlProject: repository(name: "graphql", owner: "facebook") {
    id
    name
    description
    homepageUrl
  }
  reactProject: repository(name: "react", owner: "facebook") {
    id
    description
    createdAt
    homepageUrl
  }
}
```

Result:

```json
{
  "data": {
    "graphqlProject": {
      "id": "MDEwOlJlcG9zaXRvcnkzODM0MjIyMQ==",
      "name": "graphql-spec",
      "description": "GraphQL is a query language and execution engine tied to any backend service.",
      "homepageUrl": "https://spec.graphql.org"
    },
    "reactProject": {
      "id": "MDEwOlJlcG9zaXRvcnkxMDI3MDI1MA==",
      "description": "A declarative, efficient, and flexible JavaScript library for building user interfaces.",
      "createdAt": "2013-05-24T16:15:54Z",
      "homepageUrl": "https://reactjs.org"
    }
  }
}
```
