# Implementing Category pages

Nosto provides functionality to retrieve all products from specific category. This is useful when you want to implement category merchandising using the same search API.

## Using category ID

By providing [categoryId](https://search.nosto.com/v1/graphql?ref=InputSearchProducts) parameter API will return all products associated with that category.&#x20;

### Query

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

## Using category path

By providing [categoryPath](https://search.nosto.com/v1/graphql?ref=InputSearchProducts) parameter API will return all products associated with that category. This parameter is the same as `categories` product field.

### Query

```graphql
query {
  search(
    accountId: "YOUR_ACCOUNT_ID"
    products: {
      categoryPath: "T-Shirts"
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

## Using custom filters

In rare cases [categoryId](https://search.nosto.com/v1/graphql?ref=InputSearchProducts) or [categoryPath](https://search.nosto.com/v1/graphql?ref=InputSearchProducts) is not enough. In these cases [custom filters](https://search.nosto.com/v1/graphql?ref=InputSearchFilter) can be used to build any query for category & landing pages.

{% hint style="info" %}
[Authentication](https://docs.nosto.com/techdocs/implementing-nosto/implement-search/implement-search-using-api/using-the-search-api#authentication) is required when using custom filters for category pages
{% endhint %}

### Query

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

### Tracking

Category page results and subsequent filtering, sorting, paginating events should be tracked with [JS API library helper](../../../apis/js-apis/search.md#search) where `type = category`. Click events should be tracked accordingly using [click helper](../../../apis/js-apis/search.md#search-product-click) where `type = category`.

