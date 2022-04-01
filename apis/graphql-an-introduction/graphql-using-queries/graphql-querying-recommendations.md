# Querying Recommendations

You can generic recommendations using the GraphQL orders endpoint. The recommendations offered here don't use sessions so they aren't personalized but still offer enough flexibility to support a multitude of use-cases.

## Fetching toplists recommendations

The endpoint can be used to fetch toplist recommendations i.e. best-sellers. Toplists recommendations are either sorted by views or buys.

```
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

## Fetching random recommendations

The endpoint can be used to fetch random recommendations i.e. previews. Random recommendations are a totally randomized selection of products and often used for testing purposes.

```
curl -0 -v -X POST https://api.nosto.com/v1/graphql \
-u ":<token>" \
-H 'Content-Type: application/graphql' \
-d @- << EOF
query {
  recos (preview: false, image: VERSION_7_200_200) {
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

## Fetching related recommendations

The endpoint can be used to fetch related recommendations i.e. cross-sellers. Cross-sell recommendations allow you fetch related products to a given set of products.

**Example:** If you were to use this to add recommendations to a product page, the `productIds` parameter would be a single-item array containing the product identifiers of the product that is being viewed.

**Example:** If you were to use this to add recommendations to an order-follow email, the `productIds` parameter would be an array containing the product identifiers of the products that were purchased.

```
curl -0 -v -X POST https://api.nosto.com/v1/graphql \
-u ":<token>" \
-H 'Content-Type: application/graphql' \
-d @- << EOF
query {
  recos (preview: false, image: VERSION_7_200_200) {
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

## Fetching search recommendations

The endpoint can be used to fetch recommendations related to a search term.

**Example:** If you were to use this to add recommendations to a search page, the `term` parameter would be the entire search term as queries by the user.

**Note:** This endpoint cannot be used for building auto-complete style integrations.

```
curl -0 -v -X POST https://api.nosto.com/v1/graphql \
-u ":<token>" \
-H 'Content-Type: application/graphql' \
-d @- << EOF
query {
  recos (preview: false, image: VERSION_7_200_200) {
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

## Add inclusive or exclusive filters

In case some cases it is desired to filter products by their properties. `include` and `exclude` fields of `primary` should be used to achieve this. The former will filter only the products that match the specified attribute, the later will filter out the products that match the attribute.

`include` field expects a type of `InputIncludeParams`, whose attributes are

```
brands: [String]
The list of brand used for inclusion or exclusion

categories: [String]
The list of categories used for inclusion or exclusion

customFields: [InputAttributeParams]
The map of product attributes used for inclusion or exclusion

discounted: Boolean
A boolean indicating whether discounted products should be excluded

fresh: Boolean
A boolean value denoting the inclusion of new products

price: InputPriceParams
The price range used for inclusion

productIds: [String]
The list of product identifiers used for inclusion or exclusion

rating: Float
The minimum rating score for the inclusion

reviews: Int
The minimum amount of reviews for the inclusion

search: String
Search all products containing this text

stock: InputStockParams
The stock level range used for inclusion

tags1: [String]
The first set of tags used for inclusion or exclusion

tags2: [String]
The second set of tags used for inclusion or exclusion

tags3: [String]
The third set of tags used for inclusion or exclusion
```

`exclude` field expect type `InputFilterParams` whose attributes are:

```
brands: [String]
The list of brand used for inclusion or exclusion

categories: [String]
The list of categories used for inclusion or exclusion

customFields: [InputAttributeParams]
The map of product attributes used for inclusion or exclusion

discounted: Boolean
A boolean indicating whether discounted products should be excluded

productIds: [String]
The list of product identifiers used for inclusion or exclusion

search: String
Search all products containing this text

tags1: [String]
The first set of tags used for inclusion or exclusion

tags2: [String]
The second set of tags used for inclusion or exclusion

tags3: [String]
The third set of tags used for inclusion or exclusion
```

Here is an example of a query including filters

```bash
curl -0 -v -X POST https://api.nosto.com/v1/graphql \
-u ":<token>" \
-H 'Content-Type: application/graphql' \
-d @- << EOF
query {
  recos (preview: false, image: VERSION_7_200_200) {
    toplist(hours: 168, sort: BUYS, params: {
      minProducts: 1
      maxProducts: 10
      include: {
        customFields: [
          {
            "attribute": "pattern"
            "values": "Solid"
          }
        ]
        discounted: true
      }
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
