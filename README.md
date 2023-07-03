# wave-connector

[Wave](https://en.wikipedia.org/wiki/Wave_Financial) is a webapp that provides financial services for small businesses. It helps business owners take control of their business financial data and streamline their financial operations.

### GraphQL
Wave's APIs use [GraphQL](https://graphql.org). That cuts down the number of API requests and provides flexibility to request only the required data.

There are two common types of operations in GraphQL, a `query` to request some data, and a `mutation` to submit or change data.

### Query

In the following example we query all businesses with [pagination](https://developer.waveapps.com/hc/en-us/articles/360018856791-Pagination):

```graphql
query ($page: Int!, $pageSize: Int!) {
  businesses(page: $page, pageSize: $pageSize) {
    pageInfo {
      currentPage
      totalPages
      totalCount
    }
    edges {
      node {
        id
        name
      }
    }
  }
}
```

The `page` and `pageSize` [variables](https://developer.waveapps.com/hc/en-us/articles/360024906591) are defined separately. In Wave playground they defined under `Query variables`, and in the `Systems` file they defined in the payload object:

```graphql
# Wave playground

{
  "page": 1,
  "pageSize": 20
}
```

```json
// system.json
{
    "customer-lookup": {
        "method": "POST",
        "payload": {
        "query": "query($businessId: ID!, $customerId: ID!) { business(id: $businessId) { id customer(id: $customerId) { id } } }",
        "variables": {
          "businessId": "{{@ account_id @}}",
          "customerId": "{{ entity.id }}"
        }
      },
      "payload_property": "data.business.customer",
      "url": ""
    }
}
```

### Mutation

In the following example we `delete` (_archive_ in that case) an `account`. The operation will return `didSucceed` to indicate if the operation succeeded or failed, and `inputErrors` object in case of an error:

```graphql
mutation($input: AccountArchiveInput!) {
  accountArchive(input: $input) {
    didSucceed
    inputErrors {
      code
      message
      path
    }
  }
}

```
```graphql
# playground

{
	"input": {
    "id": "QWNjb3VudDoxNzYwODI0MDk0NzkxNjkzMDk1O0J1c2luZXNzOjJiY2QxZGRiLTg1ODktNDIwNi05N2E0LWUzMjRlMTM5Yzk4Nw=="
	}
}
```

### Useful links
#### GraphQL
- [GraphQL](https://graphql.org)
- [HowToGraphQL](https://www.howtographql.com/basics/0-introduction)

#### Wave API and documentation
- [Dev portal documentation](https://developer.waveapps.com/hc/en-us/categories/360001114072-Documentation)
- [GraphQL API playground](https://gql.waveapps.com/help-center)
- [API reference](https://developer.waveapps.com/hc/en-us/articles/360019968212-API-Reference)

#### Tools
https://twin.sh/utilities/graphql-minifier