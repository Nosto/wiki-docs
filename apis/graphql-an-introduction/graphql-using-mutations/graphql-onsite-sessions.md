# GraphQL: Onsite Sessions

## Creating a session

When a new user comes to the app, you can use this method to get a new session. It will return you a customer-id that can save on the device and use for future requests. This would be ideal.

```graphql
mutation {
  newSession(referer: "https://google.com?q=shoes")
}
```

## Updating a session

When a customer logs in, you can update the existing customer with the customer-reference. This would merge the online and mobile sessions. If not needed, I would omit this for now.

## Working with recommendations

### On the Product Page

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

