# For Headless

Nosto's GraphQL APIs can be used for simplified implementations for headless frontends. While we recommend using our JS API for use in headless environments, there are scenarios where a GraphQL based implementation might be more fruitful.

* [Using the API](graphql-using-the-api.md)
* [Testing & Debugging](graphql-testing-and-debugging.md)

🚨Implementing Nosto over GraphQL will not allow you to leverage the entire Nosto suite. The following features will not function:

* Facebook Ads: As the pixel events aren't dispatched.
* Content Personalisation: As the GraphQL API only handle the personalization and not onsite experiences.
* Popups: As the GraphQL API only handle the personalization and not onsite experiences.

Each customer who visits a site is uniquely identified with a session identifier. When a new customer comes to the site, a GraphQL session mutation call must be made to initiate a session. The resultant session identifier must be persisted and reused for all consecutive calls.

💡The session-duration is 30 minutes from the last activity.

We recommend using the following flow to creating and resuming sessions.

1. Read the session-identifier from the cookie.
2. If the session-identifier doesn't exist, initiate a new session and store the resultant session identifier in a cookie.
3. Read the session identifier from the cookie, and leverage the mutations for the outlined page types.

#### Starting a new session

In order to start a new session when a session-identifier doesn't exist, you'll need to use the `newSession` mutation

```graphql
mutation {
  newSession(referer: "https://google.com?q=shoes")
}
```

The `newSession` mutation will return a unique session-identifier that you must persist.

⚠️ How you persist the session-identifier is entirely dependant upon your implementation. For example, you can persist it into a cookie or even application storage.

#### Using a session

If you already have a session-identifier, you can pass that using the `updateSession` mutation.

The example below is a generic example of how you update a session and pass the appropriate event to Nosto but in practice, you'll be using one of the page-specific mutations as shown in the section "Implementing on pages".

```graphql
mutation {
  updateSession(by: BY_CID, id: "5b1a481060b221115c4a251e",
    params: {
      event: {
        type: VIEWED_PAGE
        target: "https://example.com"
      }
    }
  ) {
    id
  }
}
```

## Implementing on pages

In order to use Nosto the different pages, you'll need to make the appropriate mutations for the different page types.

Every page-specific mutation requires you to pass the event for the specific page. These events are used to pass signals to Nosto's intelligence engine. Each of the page-specific mutations also allows you to fetch the recommendations for the given page type.

#### Sending the cart

When you mutate a session, it is imperative that you send the full cart contents.

If the current shopping cart is empty, this can be omitted.

```graphql
mutation MySession {
  updateSession(by: BY_CID, id: "ad8f0d0e-1156-4df2-b385-10e03f8f8a44",
    params: {
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
          }
        ]
      }
    }) {
      id
    }
  }
}
```

#### Sending the customer

When you mutate a session, it is imperative that you send the details of the currently logged-in customer. If no customer if currently logged in, this can be omitted.

```graphql
mutation MySession {
  updateSession(by: BY_CID, id: "ad8f0d0e-1156-4df2-b385-10e03f8f8a44",
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
    }) {
      id
    }
  }
}
```

#### Sending attribution parameters

When navigating between pages, if the navigation happens as a result of a click on a recommendation element, you must pass the identifier as part of the route and on the next page load, read the attribution parameter and pass it along with the event as the `ref` parameter.

```graphql
mutation MySession {
  updateSession(by: BY_CID, id: "ad8f0d0e-1156-4df2-b385-10e03f8f8a44",
    params: {
      event: {
        type: VIEWED_PRODUCT
        target: "400"
        ref: "frontpage-bestseller"
      }
    }) {
      id
    }
  }
}
```

⚠️ If you do not pass the attribution parameter, the recommendations statistics will be inaccurate but will not affect the quality of the recommendations.

#### Previewing the recommendations

All default the recommendation results are returned when the recommendations are enabled and the account is a live account. If you would like to preview the recommendations, all the recommendation fields accept a boolean `isPreview` parameter.

```graphql
mutation {
  updateSession(by: BY_CID, id: "5b1a481060b221115c4a251e",
    params: {
      event: {
        type: VIEWED_PAGE
        target: "https://example.com"
      }
    }
  ) {
    pages {
      forFrontPage(params: {
        isPreview: false, imageVersion:  VERSION_8_400_400
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

⚠️ Any recommendation requests when the preview mode is enabled do not accrue towards the statistics or skew the product relations.

### On your home page

#### Sending the event

In order to use the GraphQL session mutation to fetch recommendations for your home or front page, the event, in this case, must be `VIEWED_PAGE` and you should specify a fully qualified URL of the home page as the target.

```graphql
mutation {
  updateSession(by: BY_CID, id: "5b1a481060b221115c4a251e",
    params: {
      event: {
        type: VIEWED_PAGE
        target: "https://example.com"
      }
    }
  ) {
    pages {
      forFrontPage(params: {
        isPreview: false, imageVersion:  VERSION_8_400_400
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

#### Fetching Recos

The `forFrontPage` field will return the result of all the recommendations that are configured for the front page.

### On your Category pages

#### Sending the event

In order to use the GraphQL session mutation to fetch recommendations for your category page, the event, in this case, must be `VIEWED_CATEGORY` and you should specify a fully qualified category path of the current category. For example, if you have a category called "Dresses" under the category "Women", the FQCN would be "/Women/Dresses".

```graphql
mutation {
  updateSession(by: BY_CID, id: "5b1a481060b221115c4a251e",
    params: {
      event: {
        type: VIEWED_CATEGORY
        target: "/Shorts"
      }
    }
  ) {
    pages {
      forCategoryPage(params: {
        isPreview: false, imageVersion:  VERSION_8_400_400
      }, category: "Shorts") {
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

#### Fetching Recos

The `forCategoryPage` field will return the result of all the recommendations that are configured for the front page.

### On your Product pages

#### Sending the event

In order to use the GraphQL session mutation to fetch recommendations for your search page, the event, in this case, must be `VIEWED_PRODUCT` and you should specify the product-identifier of the current product being viewed.

```graphql
mutation {
  updateSession(by: BY_CID, id: "5b1a481060b221115c4a251e",
    params: {
      event: {
        type: VIEWED_PRODUCT
        target: "11923861519"
        ref: "front-page-slot-1"
      }
    }
  ) {
    pages {
      forProductPage(params: {
        isPreview: false, imageVersion:  VERSION_8_400_400
      }, product: "11923861519") {
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

#### Fetching Recos

The `forProductPage` field will return the result of all the recommendations that are configured for the product page.

### On your search page

#### Sending the event

In order to use the GraphQL session mutation to fetch recommendations for your search page, the event, in this case, must be `SEARCHED_FOR` and you should specify the search term of the query.

```graphql
mutation {
  updateSession(by: BY_CID, id: "5b1a481060b221115c4a251e",
    params: {
      event: {
        type: SEARCHED_FOR
        target: "black shoes"
      }
    }
  ) {
    pages {
      forSearchPage(params: {
        isPreview: false, imageVersion:  VERSION_8_400_400
      }, term: "black shoes") {
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

#### Fetching Recos

The `forSearchPage` field will return the result of all the recommendations that are configured for the search page.

### On your cart/checkout page

#### Sending the event

In order to use the GraphQL session mutation to fetch recommendations for your cart or checkout page, the event, in this case, must be `VIEWED_PAGE` and you should specify a fully qualified URL of the cart page as the target.

```graphql
mutation {
  updateSession(by: BY_CID, id: "5b1a481060b221115c4a251e",
    params: {
      event: {
        type: VIEWED_PAGE
        target: "https://example.com/cart"
      }
    }
  ) {
    pages {
      forCartPage(params: {
        isPreview: false, imageVersion:  VERSION_8_400_400
      }, value: 100) {
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

#### Fetching Recos

The `forCartPage` field will return the result of all the recommendations that are configured for the front page.

### On the order page

#### Sending the event

In order to use the GraphQL session mutation to fetch recommendations for your cart or checkout page, you must use a different mutation as compared to the rest of the pages.

⚠️ The customer, in this case, is the details of the customer making the purchase.

```graphql
mutation {
  placeOrder(by:BY_CID, id: "514421fce84abcb61bd45241", params: {
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

#### Fetching Recos

The `forOrderPage` field will return the result of all the recommendations that are configured for the order-confirmation page.

