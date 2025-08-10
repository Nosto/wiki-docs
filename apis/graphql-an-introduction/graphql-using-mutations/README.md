# Using Mutations

The mutation methods allow you to change the session on Nosto's end and request personalized recommendations. Each mutation operation allows you to update the cart and customer information, all while giving you access to recommendations for the sessions.

Any mobile experience built atop Nosto's GraphQL API should use the mutation operation as it feeds data into to the recommender systems while providing personalization data.

\_The given example updates the customer's information and his current shopping cart contents sends an event that the customer is currently viewing product number 400 and requests the personalized recommendations associated with a given product for him aliased as `front_page_1`.

```text
curl -0 -v -X POST https://api.nosto.com/v1/graphql \
-u ":<token>" \
-H 'Content-Type: application/graphql' \
-d @- << EOF
mutation MySession {
  updateSession(id: "ad8f0d0e-1156-4df2-b385-10e03f8f8a44",
    params: {
      customer: {
        firstName: "John"
        lastName: "Doe"
        marketingPermission: true
        customerReference: "319330"
      }
      event: {
        type: VIEWED_PRODUCT
        target: "400"
      }
      cart: {
        items: [
          {
            productId: "100",
            skuId: "100-1",
            name: "#100",
            unitPrice: 199,
            priceCurrencyCode: "EUR",
            quantity: 1
          },
          {
            productId: "200",
            skuId: "200-1",
            name: "#200",
            unitPrice: 299,
            priceCurrencyCode: "EUR",
            quantity: 2
          },
          {
            productId: "300",
            skuId: "300-1",
            name: "#300",
            unitPrice: 399,
            priceCurrencyCode: "EUR",
            quantity: 3
          },
        ]
      }
    }) {
      id,
      recos (preview: false, image: VERSION_8_400_400) {
        front_page_1: related(productIds: ["525834092559"],
          relationship: VIEWED_TOGETHER
          params: {
            minProducts: 3,
            maxProducts: 5
        }) {
        primary {
          productId
        }
      }
    }
  }
}
EOF
```

