# FRAGMENT

Reusable set of fields. To avoid duplication.

```graphql
{
  graphqlProject: repository(name: "graphql", owner: "facebook") {
    ...repoFields
  }
  reactProject: repository(name: "react", owner: "facebook") {
    ...repoFields
  }
}

fragment repoFields on Repository {
  id
  description
  homepageUrl
}
```
