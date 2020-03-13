You can query orders using the GraphQL orders endpoint. So long as you are able to specify the order number or an external order reference, you will be able to query the order.

```graphql
curl -0 -v -X POST https://api.nosto.com/v1/graphql \
-u ":<token>" \
-H 'Content-Type: application/graphql' \
-d @- << EOF
query {
  order(id: "M2_2") {
    number
    reference
    items {
      productId
      skuId
      unitPrice
      priceCurrencyCode
      quantity
    }
    recos(preview: true, image: VERSION_1_170_170) {
      related(params: {
        minProducts: 1,
        maxProducts: 5
      }) {
        primary {
          productId
          name
          price
        }
      }
    }
  }
}
EOF
```