You can generic recommendations using the GraphQL orders endpoint. The recommendations offered here don't use sessions so they aren't personalized but still offer enough flexibility to support a multitude of use-cases.

### Fetching toplists recommendations

The endpoint can be used to fetch toplist recommendations i.e. best-sellers. Toplists recommendations are either sorted by views or buys.

```shell
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

### Fetching random recommendations

The endpoint can be used to fetch random recommendations i.e. previews. Random recommendations are a totally randomized selection of products and often used for testing purposes.

```shell
curl -0 -v -X POST https://api.nosto.com/v1/graphql \
-u ":<token>" \
-H 'Content-Type: application/graphql' \
-d @- << EOF
query {
  recos (preview: true, image: VERSION_7_200_200) {
    random(params: {
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

### Fetching related recommendations

The endpoint can be used to fetch related recommendations i.e. cross-sellers. Cross-sell recommendations allow you fetch related products to a given set of products.

**Example:** If you were to use this to add recommendations to a product page, the `productIds` parameter would be a single-item array containing the product identifiers of the product that is being viewed.

**Example:** If you were to use this to add recommendations to an order-follow email, the `productIds` parameter would be an array containing the product identifiers of the products that were purchased.

```shell
curl -0 -v -X POST https://api.nosto.com/v1/graphql \
-u ":<token>" \
-H 'Content-Type: application/graphql' \
-d @- << EOF
query {
  recos (preview: true, image: VERSION_7_200_200) {
    related(relationship: VIEWED_TOGETHER, productIds: [
    	"8685560646"  
    ],
    params: {
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

### Fetching search recommendations

The endpoint can be used to fetch recommendations related to a search term.

**Example:** If you were to use this to add recommendations to a search page, the `term` parameter would be the entire search term as queries by the user.

**Note:** This endpoint cannot be used for building auto-complete style integrations.

```shell
curl -0 -v -X POST https://api.nosto.com/v1/graphql \
-u ":<token>" \
-H 'Content-Type: application/graphql' \
-d @- << EOF
query {
  recos (preview: true, image: VERSION_7_200_200) {
    search(term: "shoes",
    params: {
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