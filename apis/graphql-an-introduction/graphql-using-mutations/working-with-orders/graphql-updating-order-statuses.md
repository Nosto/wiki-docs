# GraphQL: Updating Order Statuses

If you would like to update the order-status for a given order, you can do so using the following request.

```graphql
mutation {
  updateStatus(number: "ORD102-33", params: {
    orderStatus: "fraud"
    paymentProvider: "klarna"
    statusDate: "2011-12-03T10:17:30"
  }) {
    number
    statuses {
      date
      orderStatus
      paymentProvider
    }
  }
}
```

