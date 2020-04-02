# Sending Offline Orders

Mutations can be used to send order information to Nosto. The `createOrders` mutation allows you to create one or more orders at a time.

Any validation errors in the order data are accessible in the response. The entire order object is accessible in the response too. In the event that an order validation error led to the order to not be updated, the response would contain the errors as well as the invalid order data.

**Note:** This mutation is currently in closed beta.

The given example creates a single order \#74 and requests the details of the created order and any associated errors.

```text
curl -0 -v -X POST https://api.nosto.com/v1/graphql \
-u ":<token>" \
-H 'Content-Type: application/graphql' \
-d @- << EOF
mutation {
  createOrders(orders: [
    {
      orderNumber: "74",
      externalOrderRef: "100004994",
      paymentProvider: "mastercard_mxodo",
      orderStatusCode: "PENDING",
      orderStatusDescription: "Pending confirmation",
      purchasedItems: [{
        productId: "123",
        skuId: "sku-123",
        quantity: 1,
        name: "Shoes",
        unitPrice: 54.90,
        priceCurrencyCode: "EUR"
      }],
      createdAt: "2007-11-13T09:00Z",
      customer: {
        firstName: "Joe",
        lastName: "Smith",
        email: "j.smith@example.com"
      },
      customerId: "5c897d600000000000000000",
      meta: {
        pos_id: "1",
        pos_description: "Helsinki city center outlet"
      }
    }
  ]) {
    result {
      errors {
        field
        message
      }
      data {
        orderNumber
      }
    }
  }
}
EOF
```

