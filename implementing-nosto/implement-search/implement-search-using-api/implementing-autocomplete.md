# Implementing Autocomplete

### Autocomplete <a href="#autocomplete" id="autocomplete"></a>

Autocomplete is an element shown under search input used to display products for a partial query. The same GraphQL API should be used with a smaller `size` argument:

#### Query

```graphql
query {
  search(
    accountId: "YOUR_ACCOUNT_ID"
    query: "green"
    products: { size: 4 }
  ) {
    products {
      hits {
        productId
        name
      }
      total
      size
    }
    query
  }
}
```

[GraphQL playground example](https://search.nosto.com/v1/graphql?query=%7B%0A%20%20search\(accountId:%20%22YOUR\_ACCOUNT\_ID%22,%20query:%20%22green%22,%20products:%20%7Bsize:%204%7D\)%20%7B%0A%20%20%20%20products%20%7B%0A%20%20%20%20%20%20hits%20%7B%0A%20%20%20%20%20%20%20%20productId%0A%20%20%20%20%20%20%20%20name%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20total%0A%20%20%20%20%20%20size%0A%20%20%20%20%7D%0A%20%20%20%20query%0A%20%20%7D%0A%7D)

Response

```json
{
  "data": {
    "search": {
      "query": "green",
      "products": {
        "hits": [
          {
            ...
          }
        ]
        "size": 4,
        "total": 300
      }
    }
  }
}
```