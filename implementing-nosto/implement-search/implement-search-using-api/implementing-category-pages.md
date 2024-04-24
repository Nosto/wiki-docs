# Implement Category pages

Nosto provides functionality to retrieve all products for a specific category. This is useful when you want to implement category merchandising using the same API as for Search.

## API Requests <a href="#autocomplete" id="autocomplete"></a>

### Using category ID

Provide the [categoryId](https://search.nosto.com/v1/graphql?ref=InputSearchProducts) API parameter to fetch all products associated with that category.&#x20;

#### Query

```graphql
query {
  search(
    accountId: "YOUR_ACCOUNT_ID"
    products: {
      categoryId: "123456789"
    }
  ) {
    products {
      hits {
        productId
        name
      }
      total
      size
    }
  }
}
```

### Using category path

Provide the [categoryPath](https://search.nosto.com/v1/graphql?ref=InputSearchProducts) API parameter to fetch all products associated with that category. This parameter is the same as the `categories` product field.

#### Query

```graphql
query {
  search(
    accountId: "YOUR_ACCOUNT_ID"
    products: {
      categoryPath: "Pants"
    }
  ) {
    products {
      hits {
        productId
        name
      }
      total
      size
    }
  }
}
```

#### Child category handling

Depending on your configuration, fetching a parent category will also include products from the child categories. For example, fetching products for the category `Pants` would also include products from the categories `Pants -> Shorts` and `Pants -> Khakis`.

This is an admin-only setting. Please contact your Nosto representative to adjust this setting.

### Using custom filters

In some rare cases [categoryId](https://search.nosto.com/v1/graphql?ref=InputSearchProducts) or [categoryPath](https://search.nosto.com/v1/graphql?ref=InputSearchProducts) is not enough. In these cases [custom filters](https://search.nosto.com/v1/graphql?ref=InputSearchFilter) can be used to build any query for category & landing pages.

#### Query

```graphql
query {
  search(
    accountId: "YOUR_ACCOUNT_ID"
    products: {
      preFilter: [
        {
          field: "productId",
          value: [
            "2276",
            "2274"
          ]
        }
      ],
    }
  ) {
    products {
      hits {
        productId
        name
      }
      total
      size
    }
  }
}
```

### Other features & implementation

The category page shares a lot of similarities with the search page, so please refer to the search page documentation:

{% content-ref url="implementing-search-page.md" %}
[implementing-search-page.md](implementing-search-page.md)
{% endcontent-ref %}

## Analytics

### Nosto Analytics

To analyze user behavior you need to implement tracking. This can be achieved using our [JavaScript library](../search/). You need to implement the following methods with `type = category`:

* [recordSearch](../search/#search-1) to track category page visits
* [recordSearchClick](../search/#search-product-keyword-click) to track clicks on category results

