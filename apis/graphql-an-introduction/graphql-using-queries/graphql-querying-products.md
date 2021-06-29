# Querying Products

## List Products

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

