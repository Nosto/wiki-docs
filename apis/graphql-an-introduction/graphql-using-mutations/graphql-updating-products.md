# Updating Products

Mutations can be used to update the product catalog in Nosto. The `updateProducts` mutation allows you to update one or more products at a go.

Any validation errors in the product data are accessible in the response. The entire product object is accessible in the response too. In the event that a product validation error led to the product to not be updated, the response would contain the errors as well as the invalid product data.

**Note:** This mutation is currently in beta and may not support mutating all product fields. At the time of writing, it is only possible to update the UGC images for a product.

The given example updates the UGC image for product \#74 and requests the details of the updated products and any associated errors.

```text
curl -0 -v -X POST https://api.nosto.com/v1/graphql \
-u ":<token>" \
-H 'Content-Type: application/graphql' \
-d @- << EOF
mutation {
  updateProducts(products: [
    {
      id: "74"
      url: "https://store.mybigcommerce.com/sample-french-connection-straw-bag/"
      ugcImages: [
        {
          url: "https://instagram.com/photo.jpg"
        }
      ]
    }
  ]) {
    result {
      errors {
        field
        message
      }
      data {
        productId
      }
    }
  }
}
EOF
```

