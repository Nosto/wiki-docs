# Querying Category Merchandising Products

You can query [category merchandising products](https://help.nosto.com/en/articles/3648242-get-started-with-category-merchandising) using the query below:

```text
curl -0 -v -X POST https://api.nosto.com/v1/graphql \
-u ":<token>" \
-H 'Content-Type: application/graphql' \
-d @- << EOF
query {
  session(by: BY_CID, id: $customerId) {
    id
    recos(
      preview: false,
      image: VERSION_ORIGINAL,
      addAttributionParameters: true) {
      category(
        category: $categoryName,
        minProducts: 1,
        maxProducts: 10
      ) {
        primary {
          productId
          url
        }
        batchToken
        totalPrimaryCount
        resultId
      }
    }
  }
}
EOF
```

## Session Lookup

In order to be able to provide personalized results, we will need to look up a session either by id or by reference. You read more about managing sessions on our [onsite sessions](../graphql-using-mutations/graphql-onsite-sessions.md) wiki page.

## Attribution Parameters

Setting the `addAttributionParameters` parameter to `true`, causes attribution parameters to be automatically rendered in product URLs. For example, the url `https://example.com/product` becomes `https://example.com/product?nosto_source=cmp&amp;nosto=5e5e09f060b232790cbbccbf`. These parameters allow our client script to track the performance of Category Merchandising results.

If you wish to handle attribution parameters manually, the `addAttributionParameters` parameter can either be set to `false` or can be omitted. In order to build the attribution parameters yourself, you will need to set `nosto_source` to `cmp` and the `nosto` parameter to the `resultId` from the GraphQL response.

## Event handling

Graphql calls using CMP methods are treated as a category view by default. This behavior can be changed by including skipVCEvent: true into the graphql request. All product URLs on a category page must be appended with #nosto_cmp fragment. An example of such a product URL would be www.test-store.com/product1#nosto_cmp where #nosto_cmp is the added fragment.

## Pagination

The `batchToken` can be used the next batch of results. This is useful if you only wish to fetch the first 10 products when there may be thousands of results. To fetch the next batch, use the `batchToken` in the next query like in the example below:

```text
curl -0 -v -X POST https://api.nosto.com/v1/graphql \
-u ":<token>" \
-H 'Content-Type: application/graphql' \
-d @- << EOF
query {
  session(by: BY_CID, id: $customerId) {
    id
    recos(
      preview: false,
      image: VERSION_ORIGINAL,
      addAttributionParameters: true) {
      category(
        category: $categoryName,
        minProducts: 1,
        maxProducts: 10,
        batchToken: "n200MjA2NDc0OTUyNzg1+z/wAAAAAAAA/w=="
      ) {
        primary {
          productId
          url
        }
        batchToken
        totalPrimaryCount
        resultId
      }
    }
  }
}
EOF
```

If you wish to skip the first number of pages, you can use the `skipPages` parameter instead of the `batchToken`. A page size is calculated from the `maxProducts` parameter.

## Filtering Results

Results can be filtered by specifying the include and exclude parameters. You can explore more parameters in the [GraphQL Playground](https://github.com/Nosto/techdocs/wiki/GraphQL:-The-Playground)

```bash
curl -0 -v -X POST https://api.nosto.com/v1/graphql \
-u ":<token>" \
-H 'Content-Type: application/graphql' \
-d @- << EOF
query {
  session(by: BY_CID, id: $customerId) {
    id
    recos(
      preview: false,
      image: VERSION_ORIGINAL,
      addAttributionParameters: true) {
      category(
        category: $categoryName,
        minProducts: 1,
        maxProducts: 10,
        include: {
          customFields: [{
            attribute: "color",
            values: ["white"]
          }]
        }
        exclude: {
          discounted: true
        }
      ) {
        primary {
          productId
          url
        }
        batchToken
        totalPrimaryCount
        resultId
      }
    }
  }
}
EOF
```

