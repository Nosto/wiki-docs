# Implementing Category page

Nosto provides functionality to get all products from a specific category. This is useful when you want to implement category merchandising through the same search GraphQL API.

An empty search query should be provided and `categoryId`:

### Query

```graphql
query {
  search(
    accountId: "YOUR_ACCOUNT_ID"
    products: { categoryId: "123456789" }
  ) {
    products {
      hits {
        productId
        price
        categoryIds
        name
      }
      total
      size
      categoryId
    }
  }
}
```

[GraphQL playground example](https://search.nosto.com/v1/graphql?query=%7B%0A%20%20search\(accountId:%20%22YOUR\_ACCOUNT\_ID%22,%20products:%20%7BcategoryId:%20%22123456789%22%7D\)%20%7B%0A%20%20%20%20products%20%7B%0A%20%20%20%20%20%20hits%20%7B%0A%20%20%20%20%20%20%20%20productId%0A%20%20%20%20%20%20%20%20price%0A%20%20%20%20%20%20%20%20categoryIds%0A%20%20%20%20%20%20%20%20name%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20total%0A%20%20%20%20%20%20size%0A%20%20%20%20%20%20categoryId%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D)

### Response

```json
{
  "data": {
    "search": {
      "products": {
        "hits": [
          {
            "productId": "",
            "price": 12.99,
            "categoryIds": ["123456789"],
            "name": "Blue T-shirt"
          },
          ...
        ],
        "total": 370,
        "size": 5,
        "categoryId": "123456789"
      }
    }
  }
}
```

You can also query by full category name using `categoryPath`:

### Query

```graphql
query {
  search(
    accountId: "YOUR_ACCOUNT_ID"
    products: { categoryPath: "T-Shirts" }
  ) {
    products {
      hits {
        productId
        price
        categories
        name
      }
      total
      size
    }
  }
}
```

[GraphQL playground example](https://search.nosto.com/v1/graphql?query=%7B%0A%20%20search\(accountId:%20%22YOUR\_ACCOUNT\_ID%22,%20products:%20%7BcategoryPath:%20%22T-Shirts%22%7D\)%20%7B%0A%20%20%20%20products%20%7B%0A%20%20%20%20%20%20hits%20%7B%0A%20%20%20%20%20%20%20%20productId%0A%20%20%20%20%20%20%20%20price%0A%20%20%20%20%20%20%20%20categories%0A%20%20%20%20%20%20%20%20name%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20total%0A%20%20%20%20%20%20size%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D)

### Response

```json
{
  "data": {
    "search": {
      "products": {
        "hits": [
          {
            "productId": "",
            "price": 12.99,
            "categories": ["T-Shirts"],
            "name": "Blue T-shirt"
          }
        ],
        "total": 370,
        "size": 5
      }
    }
  }
}
```

An empty query should be sent when using category merchandising. Also, you must use the same category values as you provide in products data export.
