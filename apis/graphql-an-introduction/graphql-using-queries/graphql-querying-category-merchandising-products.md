# Querying Category Merchandising Products (CM 1.0)

_This documentation is only for the implementation of CM 1.0 and most likely this doc shouldn't be used anymore. If you'd like to implement CM 2.0, please check this_ [_documentation_](../../../implementing-nosto/implement-search/)_. If you're not sure which version you'd like to implement, please contact your technical solutions manager or our support._

Nosto's category merchandising APIs are meant to be used for resorting your existing product listing pages. It has support for listing the products in the order defined within Nosto for different categories, including paging and filtering support. It doesn't support taking over the product listing pages fully as it doesn't include support for creating category specific filters and calculating their facet counts. The expected implementation leveraging these APIs would keep on using their current solution for building most of the product list pages, but swap the shown products with the information retrieved from this APIs results.

You can query [category merchandising products](https://help.nosto.com/en/articles/3648242-get-started-with-category-merchandising) using the query below:

```
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

## Attribution

### Automatic Attribution

Setting the `addAttributionParameters` parameter to `true`, causes attribution parameters to be automatically rendered in product URLs. For example, the url `https://example.com/product` becomes `https://example.com/product?nosto_source=cmp&amp;nosto=5e5e09f060b232790cbbccbf`. These parameters allow our client script to track the performance of Category Merchandising results.

### Manual Attribution

If you wish to handle attribution parameters manually, the `addAttributionParameters` parameter can either be set to `false` or can be omitted.

Product views resulting from clicking products of a category listing need to contain the required attribution parameters:

* `type`: `VIEWED_PRODUCT`
* `target`: (product id)
* `ref`: (`resultId` from the category merchandising response)
* `refType`: `CATEGORY_MERCHANDISING`

For example, given the following category merchandising response:

```json
{
  "data": {
    "updateSession": {
      "recos": {
        "category": {
          "resultId": "623473861055d80027efe482",
          "primary": [{
            "productId": "9497180547",
            "name": "Cool Kicks",
            "url": "https://example.com/products/cool-kicks"
          }]
        }
      }
    }
  }
}
```

If a customer clicks on Cool Kicks, the following GraphQL query should be sent to fetch the product's details and to attribute the product view to the category merchandising result:

```graphql
mutation($sessionId: String!) {
  updateSession(by: BY_CID, id: $sessionId, params: {
    event: {
      type: VIEWED_PRODUCT
      target: "9497180547"
      ref: "623473861055d80027efe482"
      refType: CATEGORY_MERCHANDISING
    }
  }) {
    primary {
      productId
      url
      name
      description
      priceText
    }
  }
}
```

## Event handling

Graphql calls using Category Merchandising methods are treated as a category view by default. This behavior can be changed by including `skipVCEvent: true` into the graphql request. All product URLs on a category page must be appended with `#nosto\_cmp` fragment. An example of such a product URL would be `www.test-store.com/product1#nosto\_cmp` where `#nosto\_cmp` is the added fragment.

## Pagination

The `batchToken` can be used the next batch of results. This is useful if you only wish to fetch the first 10 products when there may be thousands of results. To fetch the next batch, use the `batchToken` in the next query like in the example below:

```
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
