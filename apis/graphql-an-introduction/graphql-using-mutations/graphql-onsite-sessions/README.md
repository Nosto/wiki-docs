# GraphQL: Onsite Sessions

{% hint style="success" %}
[Nosto MCP Server Beta](nosto-mcp-server-beta.md) is now available to help out with documentation and implementation of GraphQL onsite sessions
{% endhint %}

## Creating a session

When a new user comes to the app, you can use this method to get a new session. It will return you a customer-id that can save on the device and use for future requests. This would be ideal.

```graphql
mutation {
  newSession(referer: "https://google.com?q=shoes")
}
```

## Updating a session

During a user session two things will happen that are likely not tied to viewing a page. Use the following mutations to keep the user session up to date.

* If you use `by: BY_CID`, pass a Nosto session ID (see above). 
* If you use `by: BY_REF`, pass an external [customer-reference](./../../../../implementing-nosto/implement-on-your-website/manual-implementation/adding-the-customer-information.md)

You will likely want to set `skipEvents` to `true` to prevent incrementing the page views. A common use case is to update the session after the "add to cart"-button on a product card has been clicked.

You also have the option to include the `cart` and `customer` information in the mutations per page type. You can explore details in the [GraphQL playground](./../../graphql-the-playground.md), the general structure of the `updateSession` mutation is:

```graphql
mutation {
  updateSession(
    by: ...,
    id: "...",
    params: {
      cart: { ... },
      customer: { ... },
      event: { ... },
      skipEvents: ...
    }
  ) {
    id
    cart: { ... }
    customer: { ... }
    events: { ... }
    pages: { ... }
    segments: { ... }
  }
}
```

### Set the cart

The cart content *must* be updated whenever the cart contents change. The cart contents are the 1:1 representation of the user's cart.

```graphql
mutation {
  updateSession(by: BY_CID, id: "5b1a481060b221115c4a251e",
    params: {
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
      },
      skipEvents: true,
      event: {
        // Same data as below from the different page types, will get ignored if skipEvents: true
      }
    }
  ) {
    id
  }
}
```

### Set the customer

When a customer logs in, you can update the existing customer with the their data and potentially an external [customer-reference](./../../../../implementing-nosto/implement-on-your-website/manual-implementation/adding-the-customer-information.md). This would merge the online and mobile sessions. If not needed, you can omit this.

```graphql
mutation {
  updateSession(by: BY_CID, id: "5b1a481060b221115c4a251e",
    params: {
      customer: {
        firstName: "Mridang"
        lastName: "Agarwalla"
        email: "mridang@nosto.com"
        customerReference: "b369f1235cf4f08153c560.82515936"
        marketingPermission: false
        doNotTrack: false
      }
      skipEvents: true,
      event: {
        // Same data as below from the different page types, will get ignored if skipEvents: true
      }
    }
  ) {
    id
  }
}
```


## Working with recommendations

### On the Product Page

In order to use the GraphQL session mutation to fetch recommendations for your product page, the event, in this case, must be `VIEWED_PRODUCT` and you should specify the product-identifier of the current product being viewed.

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

### Tracking Product Variant Views

This optional event that can be sent to signal that a specific product variant (SKU in Nosto terms) is being viewed.

* Typical use case for sending this event would be from product detail page when the user selects a product variant, such as some specific color and/or size.
* The recommendations can then be configured in the Nosto admin UI to update and give preference for products that have similar variants available. For example "Other products also available in the same size".
  
Product variant views are added with `targetFragment=skuId` in the `event` the `updateSession.params`.

Example for a product page after the SKU ID "589053" was selected by a user:

```graphql
mutation {
  updateSession(by: BY_CID, id: "5b1a481060b221115c4a251e",
    params: {
      event: {
        type: VIEWED_PRODUCT
        target: "11923861519",
        targetFragment: "589053",
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

### On the Category Page

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

### On the Search Page

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

### On the Cart Page

In order to use the GraphQL session mutation to fetch recommendations for your cart or checkout page, the event, in this case, must be `VIEWED_PAGE` and you should specify a fully qualified URL of the page as the target.

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

### On the Front Page

In order to use the GraphQL session mutation to fetch recommendations for your home or front page, the event, in this case, must be `VIEWED_PAGE` and you should specify a fully qualified URL of the page as the target

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

### On additional page types

Please review the `PageRequestEntity` in the [GraphQL Playground](../../graphql-the-playground.md) to ensure all page types are tracking the user behavior.

You can apply the same concept as in the examples above with e.g. `forNotFoundPage()` and `forOtherPage()`.


### Attribution of Recommendation Results

Recommendation results can be attributed to events by setting a session event's `ref` to the recommendation result's `resultId`.

Here is an example of some recommendations for a front page. You can see recommendation result's `resultId` is `"frontpage-nosto-1"`.

```graphql
{
  "data": {
    "updateSession": {
      "pages": {
        "forFrontPage": [
          {
            "resultId": "frontpage-nosto-1",
            "primary": [
              {
                "productId": "9497180547",
                "name": "Cool Kicks",
                "url": "https://example.com/products/cool-kicks"
              },
              {
                "productId": "9497180163",
                "name": "Awesome Sneakers",
                "url": "https://example.com/products/awesome-sneakers"
              },
              {
                "productId": "4676165861430",
                "name": "Free gift",
                "url": "https://example.com/products/free-gift"
              },
              {
                "productId": "2188051218486",
                "name": "Furry Dice",
                "url": "https://example.com/products/furry-dice"
              },
              {
                "productId": "9497180611",
                "name": "Gnarly Shoes",
                "url": "https://example.com/products/gnarly-shoes-1"
              }
            ]
          }
        ]
      }
    }
  }
}
```

If a customer selects to view the Cool Kicks product, you can generate the following request. Note that the event's `ref` is set to `"frontpage-nosto-1"`.

```graphql
mutation {
  updateSession(by: BY_CID, id: "5b1a481060b221115c4a251e",
    params: {
      event: {
        type: VIEWED_PRODUCT
        target: "9497180547"
        ref: "frontpage-nosto-1"
      }
    }
  ) {
    pages {
      forProductPage(params: {
        isPreview: false, imageVersion:  VERSION_8_400_400
      }, product: "9497180547") {
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

## Customer Group Pricing and Multi Currency

If your site uses Nosto variants for [customer group pricing](../../../../implementing-nosto/implement-on-your-website/advanced-implementation/adding-support-for-customer-group-pricing.md) or [multi currency](../../../../implementing-nosto/implement-on-your-website/advanced-implementation/adding-support-for-multi-currency.md), you must pass the `variantId` (e.g. `USD`, `EUR` or `GENERAL`, `GUEST`, `WHOLESALE`) to the `params` inside of `pages` and the respective `forXXPage()` field to retrieve the correct price and availability for the products.

Example for a product page to retrieve `WHOLESALE` prices and availability:

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
        isPreview: false,
        imageVersion:  VERSION_8_400_400,
        variantId: "WHOLESALE"
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


## GraphQL from mobile applications

When making GraphQL queries from mobile applications, it's essential to define the user agent string in your HTTP headers. Ideally, the user agent should represent the mobile environment, including details such as the platform, device type, and application version. Avoid using terms like "bot" in the user agent string, as this might lead to unintended behavior or rejection of the query/session. Sending an empty user agent will also lead to be catch by the bot detection mechanism.
