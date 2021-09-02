# GraphQL: Placing Orders

When a user places an order onsite or offsite, you must send the conversion tracking information to Nosto.

```graphql
mutation {
  placeOrder(by:BY_REF, id: "514421fce84abcb61bd45241", params: {
    customer: {
      firstName: "Mridang"
      lastName: "Agarwalla"
      email: "mridang@nosto.com"
      marketingPermission: false
    }
    order: {
      number: "25435"
      orderStatus: "paid"
      paymentProvider: "klarna"
      ref: "0010"
      purchasedItems: [
        {
          name: "Shoe"
          productId: "1"
          skuId: "11"
          priceCurrencyCode: "EUR"
          unitPrice: 22.43
          quantity: 1
        }
      ]
    }
  }) {
    id
  }
}
```

Orders can be associated with a customer either by [customer reference](../../../../implementing-nosto/implement-on-your-website/manual-implementation/adding-the-customer-information.md) or by customer id. The customer id matches the Nosto cookie \(this cookie is typically called `2c.cId`\).

Tracking orders by customer id looks like the following:

```graphql
mutation {
  placeOrder(by:BY_ID, id: "5d3ef53010b4f8a24c2acf9a", params: {
    customer: {
      firstName: "Mridang"
      lastName: "Agarwalla"
      email: "mridang@nosto.com"
      marketingPermission: false
    }
    order: {
      number: "25435"
      orderStatus: "paid"
      paymentProvider: "klarna"
      ref: "0010"
      purchasedItems: [
        {
          name: "Shoe"
          productId: "1"
          skuId: "11"
          priceCurrencyCode: "EUR"
          unitPrice: 22.43
          quantity: 1
        }
      ]
    }
  }) {
    id
  }
}
```

## Working with recommendations

### On the Order-Confirmation Page

To fetch the recommendations for the order-confirmation page, simply use the GraphQL field called `forOrderPage` to fetch all the recommendations for the order-confirmation page.

```graphql
mutation {
  placeOrder(by:BY_REF, id: "514421fce84abcb61bd45241", params: {
    customer: {
      firstName: "Mridang"
      lastName: "Agarwalla"
      email: "mridang@nosto.com"
      marketingPermission: false
    }
    order: {
      number: "25435"
      orderStatus: "paid"
      paymentProvider: "klarna"
      ref: "0010"
      purchasedItems: [
        {
          name: "Shoe"
          productId: "1"
          skuId: "11"
          priceCurrencyCode: "EUR"
          unitPrice: 22.43
          quantity: 1
        }
      ]
    }
  }) {
    id
    pages {
      forOrderPage(value: "25435", params: {
        imageVersion: VERSION_3_90_70
        isPreview: true
      }) {
    divId
        resultId
        primary {
          productId
        }
      }
    }
  }
}
```

