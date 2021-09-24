# Querying Products

## List Products

Querying products gives access to Nosto's product catalogues current state. It can be useful for backend integration and verification purposes. It is not meant for online use purposes as it doesn't include any concepts related to user sessions or attribution, for those refer to [GraphQL For Headless](../graphql-for-headless.md)

```graphql
curl -0 -v -X POST https://api.nosto.com/v1/graphql \
-u ":<token>" \
-H 'Content-Type: application/graphql' \
-d @- << EOF
query {
  products(
    limit: 5
    offset: 0
    sort: {field: PRICE, reverse:true},
    filter: {categories: "shoes"}
  ) {
    products {
      productId
      url
      price
      categories
    }
  }
}
EOF
```

{% hint style="info" %}
The maximum number of products that can be paged over is capped at 10000. If you need to get around this limitation, we recommend adding more restrictive filters to narrow down the result set.
{% endhint %}

## Query by Product ID

```graphql
curl -0 -v -X POST https://api.nosto.com/v1/graphql \
-u ":<token>" \
-H 'Content-Type: application/graphql' \
-d @- << EOF
query {
  product(id: "5358") {
    productId
    name
    url
    price
    listPrice
    imageUrl
    attributes {
      key
      value
    }
    skus {
      name
      price
      listPrice
      availability
      imageUrl
      url
    }
  }
}
EOF
```

