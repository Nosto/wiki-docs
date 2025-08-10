# Querying Identities

You can query identities using the GraphQL Identities endpoint. An "identity" is the personal information associated with an email address.

So long as you are able to specify the email address, you will be able to query the identity, its associated customer affinity and the personalized recommendations for that identity.

```graphql
curl -0 -v -X POST https://api.nosto.com/v1/graphql \
-u ":<token>" \
-H 'Content-Type: application/graphql' \
-d @- << EOF
query {
  identity(email:"john.smith@example.com") {
    email,
    attributes {
      name,
      value
    }
    recos(preview: true, image: VERSION_ORIGINAL) {
      history(params: {
        minProducts: 3,
        maxProducts: 10,
        showRelatedProducts: true,
        skipProductsInCart: true,
        skipBoughtProducts: true
      }) {
        primary {
          url
          imageUrl
        }
      }
    }
  }
}
EOF
```

#### What can identity attributes be used for?

The attributes associated with an identity can be used to segment users. This works similarly to how the attributes can be leveraged [when importing them via a CSV](https://help.nosto.com/en/articles/2884483-how-to-import-customer-data-to-nosto-via-api).

