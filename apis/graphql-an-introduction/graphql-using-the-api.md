# Using the API

In order to use the GraphQL endpoints, you'll need to [authenticate yourself](https://developer.nosto.com/#authentication). You will need a **Apps** [token](https://help.nosto.com/settings-and-troubleshooting-faq/settings-authentication-tokens) to access this endpoint.

**Note:** Nosto does not rate-limit the API usage but follows a fair-use policy. Nosto reserves the right to revoke API access for any abusive API usage patterns.

### Sending JSON GraphQL queries

You can send your GraphQL requests as JSON to our API and have it correctly interpolate variables passed into it. To do so, set the `Content-Type` header to `application/json`.

| Authentication | Token | Method | Endpoint |
| :--- | :--- | :--- | :--- |
| Basic | API\_APPS | POST | `https://api.nosto.com/v1/graphql` |

```text
curl -0 -v -X POST https://api.nosto.com/v1/graphql \
-u ":<token>" \
-H 'Content-Type: application/json' \
-d @- << EOF
{
  "query": "query { recos (preview: false, image: VERSION_7_200_200) { toplist(hours: 168, sort: BUYS, params: { minProducts: 1 maxProducts: 10 } ) { primary { name productId } } } }"
}
EOF
```

### Sending raw GraphQL queries

If you want to send raw GraphQL queries to the API, you can still do so but you must set the `Content-Type` header to `application/graphql`.

**Note:** If you do not set the correct Content-Type header, the request will be interpreted as JSON and will fail.

| Authentication | Token | Method | Endpoint |
| :--- | :--- | :--- | :--- |
| Basic | API\_APPS | POST | `https://api.nosto.com/v1/graphql` |

```text
curl -0 -v -X POST https://api.nosto.com/v1/graphql \
-u ":<token>" \
-H 'Content-Type: application/graphql' \
-d @- << EOF
query {
  recos (preview: false, image: VERSION_7_200_200) {
    toplist(hours: 168, sort: BUYS, params: {
      minProducts: 1
      maxProducts: 10
    }) {
      primary {
        name
        productId
      }
    }
  }
}
EOF
```

## Using Fetch

You can use the browser's fetch API to request data from GraqhQL. You will need to authenticate yourself and set the appropriate content-type headers.

### Sending JSON GraphQL queries

You can send your GraphQL requests as JSON to our API and have it correctly interpolate variables passed into it. To do so, set the `Content-Type` header to `application/json`.

```javascript
const body = JSON.stringify({
  query: `
  query {
    order(id: "5192009") {
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
`});

fetch('https://api.nosto.com/v1/graphql', {
  method: 'POST',
  headers: new Headers({
    'Content-Type': 'application/json',
    'Authorization': 'Basic ' + btoa(":" + "<token>")
  }),
  mode: 'cors',
  body
})
  .then((response) => response.json())
  .then((json) => {
    console.log(json);
  });
```

### Sending raw GraphQL queries

If you want to send raw GraphQL queries to the API, you can still do so but you must set the `Content-Type` header to `application/graphql`.

 **NOTE:**If you do not set the correct Content-Type header, the request will be interpreted as JSON and will fail.

```javascript
const body = `
query {
  order(id: "5192009") {
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
`;

fetch('https://api.nosto.com/v1/graphql', {
  method: 'POST',
  headers: new Headers({
    'Content-Type': 'application/graphql',
    'Authorization': 'Basic ' + btoa(":" + "<token>")
  }),
  mode: 'cors',
  body
})
  .then((response) => response.json())
  .then((json) => {
    console.log(json);
  });
```

