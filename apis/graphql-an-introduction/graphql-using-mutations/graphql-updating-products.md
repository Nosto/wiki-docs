# Updating Products

Mutations can be used to update the product catalog in Nosto. The `updateProducts` mutation allows you to update one or more products at a go.

Any validation errors in the product data are accessible in the response. The entire product object is accessible in the response too. In the event that a product validation error led to the product to not be updated, the response would contain the errors as well as the invalid product data.

The given example updates the product #101 and requests the details of the updated products and any associated errors.

```text
curl -0 -v -X POST https://api.nosto.com/v1/graphql \
-u ":<token>" \
-H 'Content-Type: application/graphql' \
-d @- << EOF
mutation {
  updateProducts(products: [
    {
      id: "101"
      productId: "101"
      url: "http://mridang.dev.nos.to:8890/product.htm"
      imageUrl: "https://example.com/product/sku-1.jpg"
      priceCurrencyCode: "EUR"
      price: 10
      skus: [
        {
          id: "sku-1"
          name: "One"
          availability: "InStock"
          price: 100
          listPrice: 111
          imageUrl: "https://example.com/product/sku-1.jpg"
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

The given example updates the price of #101 and requests the details of the updated products and any associated errors.

```text
curl -0 -v -X POST https://api.nosto.com/v1/graphql \
-u ":<token>" \
-H 'Content-Type: application/graphql' \
-d @- << EOF
mutation {
  updateProducts(products: [
    {
      id: "101"
      productId: "101"
      url: "http://mridang.dev.nos.to:8890/product.htm"
      price: 10
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
