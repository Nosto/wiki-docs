# Querying Identities

You can query identities using the GraphQL Identities endpoint. So long as you are able to specify the email address, you will be able to query the identity, it's associated segment information and the personalized recommendations for that identity.

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

