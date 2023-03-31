# Implementing Search page

## Searching <a href="#selecting-fields" id="selecting-fields"></a>

For a basic search, it’s enough to provide `accountId`, `query,` and select fields that should be returned. You can control which product attributes to select through `products.hits` field. See [all available fields](https://search.nosto.com/v1/graphql?ref=SearchProduct).

### Query

For example, if you want to return only `productId` and `name`, the query would be:

```graphql
query {
  search(accountId: "YOUR_ACCOUNT_ID", query: "green") {
    products {
      hits {
        productId
        name
      }
      total
      size
      from
    }
  }
}
```

[GraphQL playground example](https://search.nosto.com/v1/graphql?query=%7B%0A%20%20search\(accountId:%20%22YOUR\_ACCOUNT\_ID%22,%20query:%20%22green%22\)%20%7B%0A%20%20%20%20products%20%7B%0A%20%20%20%20%20%20hits%20%7B%0A%20%20%20%20%20%20%20%20productId%0A%20%20%20%20%20%20%20%20name%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20total%0A%20%20%20%20%20%20size%0A%20%20%20%20%20%20from%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D)

#### Query parameters

* `accountId` - Nosto account id.
* `query` - search input.

#### Query select fields

* `products.hits` - used to specify which product fields should be returned.
* `products.total` - how many products are found for query.
* `products.size` - the count of products returned per single request.
* `products.from` - what is the offset of returned products.

## Pagination and size <a href="#pagination-and-size" id="pagination-and-size"></a>

Products offset parameter `from` is used for pagination functionality.

The **default** count of documents returned per page is `size = 5`, you can change it with `products.size`, and offset of products is controlled with `products.from` field:

### Query

```graphql
query {
  search(
    accountId: "YOUR_ACCOUNT_ID"
    query: "green"
    products: { size: 10, from: 10 }
  ) {
    products {
      hits {
        name
      }
      total
      size
      from
    }
  }
}
```

[GraphQL playground example](https://search.nosto.com/v1/graphql?query=%7B%0A%20%20search\(%0A%20%20%20%20accountId:%20%22YOUR\_ACCOUNT\_ID%22%0A%20%20%20%20query:%20%22green%22%0A%20%20%20%20products:%20%7Bsize:%2010,%20from:%2010%7D%0A%20%20\)%20%7B%0A%20%20%20%20products%20%7B%0A%20%20%20%20%20%20hits%20%7B%0A%20%20%20%20%20%20%20%20productId%0A%20%20%20%20%20%20%20%20name%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20total%0A%20%20%20%20%20%20size%0A%20%20%20%20%20%20from%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D)

### Response

```json
{
  "data": {
    "search": {
      "products": {
        "hits": [
          {
            "name": "My Product"
          }
        ],
        "total": 300,
        "size": 10,
        "from": 10
      }
    }
  }
}
```

## Sorting <a href="#sorting" id="sorting"></a>

By default results are sorted by products relevance score. &#x20;

To change the sorting, use the sort parameter, where you would specify the field which should be sorted by, and order: `asc` for ascending and **** `desc` **** for descending.

By default, you should always sort by relevance. Only if the user selects a different sort method, a sorting rule should be used.

### Query

```graphql
query {
    search(
      accountId: "YOUR_ACCOUNT_ID"
      query: "green"
      products: {
        sort: [
          {
            field: "price"
            order: asc
          }
        ]
      }   
  ) {
      products {
        hits {
          productId
          name
          price
        }
      }
    }
  }
```

[GraphQL playground example](https://search.nosto.com/v1/graphql?query=%7B%0A%20%20search\(%0A%20%20%20%20accountId:%20%22YOUR\_ACCOUNT\_ID%22%0A%20%20%20%20query:%20%22green%22%0A%20%20%20%20products:%20%7B%0A%20%20%20%20%20%20sort:%20%5B%7Bfield:%20%22price%22,%20order:%20asc%7D%5D%0A%20%20%20%20%7D%0A%20%20\)%20%7B%0A%20%20%20%20products%20%7B%0A%20%20%20%20%20%20hits%20%7B%0A%20%20%20%20%20%20%20%20productId%0A%20%20%20%20%20%20%20%20name%0A%20%20%20%20%20%20%20%20price%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D)

## Faceting <a href="#faceting" id="faceting"></a>

Facets help the user to find products more easily. Faceted navigation is normally found in the sidebar of a website and contains filters only relevant to the current search query. Facets are configured in the Nosto dashboard.

{% hint style="info" %}
To use facet for a specific field you need to [configure it in the Nosto dashboard](https://help.nosto.com/en/articles/7169091-setting-up-facets) first.
{% endhint %}

### **Querying facet**

One of the facet types is `type = terms`. Assume that we have configured facets for `customFields.brandname` and `categories`:

```graphql
query {
  search(
    accountId: "YOUR_ACCOUNT_ID"
    query: "green"
  ) {
    products {
      hits {
        productId
        name
      }
      facets {
        ... on SearchTermsFacet {
          id
          field
          type
          name
          data {
            value
            count
            selected
          }
        }
      }
    }
  }
}
```

[GraphQL playground example](https://search.nosto.com/v1/graphql?query=%7B%0A%20search\(accountId:%20%22YOUR\_ACCOUNT\_ID%22%20query:%20%22green%22\)%20%7B%0A%20%20products%20%7B%0A%20%20%20hits%20%7B%20productId%20name%20%7D%0A%20%20%20%20facets%20%7B%0A%20%20%20%20%20...%20on%20SearchTermsFacet%20%7B%0A%20%20%20%20%20%20id%0A%20%20%20%20%20%20field%0A%20%20%20%20%20%20type%0A%20%20%20%20%20%20name%0A%20%20%20%20%20%20data%20%7B%20value%20count%20selected%20%7D%0A%20%20%20%20%7D%0A%20%20%20%7D%0A%20%20%7D%0A%20%7D%0A%7D)

{% hint style="info" %}
To use a facet for the specific field you need to configure it first in the Nosto dashboard.
{% endhint %}

### Response

```json
{
  "data": {
    "search": {
      "products": {
        "hits": [
          {
            "productId": "",
            "name": "",
            "price": 1.0
          }
        ],
        "facets": [
          {
            "id": "345678901abc",
            "field": "categories",
            "type": "terms",
            "name": "Categories",
            "data": [
              {
                "value": "/Shoes",
                "count": 30,
                "selected": false
              },
              {
                "value": "/Shoes/Sportswear",
                "count": 6,
                "selected": false
              }
            ]
          }
        ]
      }
    }
  }
}

```

#### Facet parameters

* `value` - original facet value, it should be displayed in the user interface. If the full category path is provided then the last category should be displayed.
* `count` - shows how many products will be returned if you select this facet, it should be displayed in the user interface.
* `selected` - indicates if the filter is selected ( used in `filter` GraphQL parameter).

Another facet type is `stats`, which returns a numeric facet:

#### Query

```graphql
query {
    search(
      accountId: "YOUR_ACCOUNT_ID"
      query: "green"
  ) {
      products {
        hits {
          productId
          name
          price
        }
        facets {
          ... on SearchStatsFacet {
            id
            field
            type
            name
            min
            max
          }
        }
      }
    }
  }
```

[GraphQL playground example](https://search.nosto.com/v1/graphql?query=%7B%0A%09search\(accountId:%20%22YOUR\_ACCOUNT\_ID%22%20query:%20%22green%22\)%20%7B%0A%09%09products%20%7B%0A%09%09%09hits%20%7B%20productId%20name%20price%20%7D%0A%09%09%09facets%20%7B%0A%09%09%09%09...%20on%20SearchStatsFacet%20%7B%0A%09%09%09%09%09id%0A%09%09%09%09%09field%0A%09%09%09%09%09type%0A%09%09%09%09%09name%0A%09%09%09%09%09min%0A%09%09%09%09%09max%0A%09%09%09%09%7D%0A%09%09%09%7D%0A%09%09%7D%0A%09%7D%0A%7D)

#### **Response**

```json
{
  "data": {
    "search": {
      "products": {
        "hits": [
          {
            "productId": "",
            "name": "",
            "price": 1.00
          }
        ],
        "facets": [
            {
                "id": "123456789abc",
                "field": "price",
                "type": "stats",
                "name": "Price",
                "min": 0.60,
                "max": 70.99
            }
        ],
      }
    }
  }
}
```

Facets of type `stats` are displayed as slider filter.

#### Stats facet fields

* `id` - a facet identification string.
* `field` - field for which facet is returned.
* `type` - facet type (`stats` or `terms`).
* `name` - facet name to render.
* `min` - minimum value for products that match provided query.
* `max` - maximum value for products that match provided query.

#### Displaying facets

Select group filter is `type = terms` facet. Its parts should be rendered from the search response's `facets` field where `type = terms`:

1. The header should be rendered using `name` field.
2. List items should be rendered using `data` array field.
3. Each list item should be rendered using `data[].value`.
4. Item's select state is rendered using `data[].selected`.
5. It's good practice to render `count` of each item.&#x20;

&#x20;

<figure><img src="../../../.gitbook/assets/image (8).png" alt=""><figcaption></figcaption></figure>

Range filter is `type = stats` facet. Its parts should be rendered from the search response's `facets` field where `type = stats`:

1. The header should be rendered using `name` field.
2. Min. and max. ranges of the filter should be rendered from `min` and `max` fields.
3. The slider position should be rendered from the current query `filter` values.

<figure><img src="../../../.gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>

## Filter <a href="#filter" id="filter"></a>

Filtering by `terms` facet, for example by _Adidas_ brand:

```graphql
query {
  search(
    accountId: "YOUR_ACCOUNT_ID"
    query: "green"
    products: {
      size: 10
      filter: [{ field: "customFields.brandname", value: "Adidas" }]
    }
  ) {
    products {
      hits {
        productId
        name
      }
      facets {
        ... on SearchTermsFacet {
          id
          field
          type
          name
          data {
            value
            count
            selected
          }
        }
      }
    }
  }
}
```

[GraphQL playground example](https://search.nosto.com/v1/graphql?query=%7B%0A%20search\(%0A%20%20accountId:%20%22YOUR\_ACCOUNT\_ID%22%20query:%20%22green%22%0A%20%20products:%20%7B%20filter:%20%5B%7B%20field:%20%22customFields.brandname%22,%20value:%20%22Adidas%22%20%7D%5D%20%7D%0A\)%20%7B%0A%20%20products%20%7B%0A%20%20%20hits%20%7B%20productId%20name%20%7D%0A%20%20%20facets%20%7B%0A%20%20%20%20...%20on%20SearchTermsFacet%20%7B%20field%20name%20data%20%7B%20value%20count%20selected%20%7D%20%7D%0A%20%20%20%7D%0A%20%20%7D%0A%20%7D%0A%7D)

When filtering by multiple same field items filters will be joined with OR operator and different fields with AND.

If you wish to have more facets, you should configure it in the Nosto dashboard first.

Filtering by `stats` field, for example by price:

```graphql
query {
   search(
    accountId: "YOUR_ACCOUNT_ID"
    query: "green"
    products: {
      filter: [
        {
          field: "price",
          range: {lt: "60", gt: "50"}
        }
      ]
    }
  ) {
    products {
      hits {
        productId
        name
      }
      facets {
        ... on SearchStatsFacet {
          id
          field
          type
          name
          min
          max
        }
      }
    }
  }
}
```

[GraphQL playground example](https://search.nosto.com/v1/graphql?query=%7B%0A%20search\(%0A%20%20accountId:%20%22YOUR\_ACCOUNT\_ID%22%20query:%20%22green%22%0A%20%20products:%20%7Bfilter:%20%5B%7Bfield:%20%22price%22,%20range:%20%7Blt:%20%2260%22,%20gt:%20%2250%22%7D%7D%5D%7D%0A\)%20%7B%0A%20%20products%20%7B%0A%20%20%20hits%20%7B%20productId%20name%20%7D%0A%20%20%20facets%20%7B%0A%20%20%20%20...%20on%20SearchStatsFacet%20%7B%20field%20name%20min%20max%20%7D%0A%20%20%20%7D%0A%20%20%7D%0A%20%7D%0A%7D)

You can sort using these arguments: `lt` (less than), `gt` (greater than), `lte` (less than or equal to), `gte` (greater than or equal to).

## Search engine configuration <a href="#selecting-fields" id="selecting-fields"></a>

Nosto Search engine is relevant out of the box and search API can be used without any initial setup. Nosto Dashboard can be used to further tune search engine configuration:

* [Searchable Fields](https://help.nosto.com/en/articles/7161528-search-engine-s-logic-and-searchable-fields) - manage which fields are used for search and their priorities,
* [Facets](https://help.nosto.com/en/articles/7169091-setting-up-facets) - create facets (filtering options) for search results page,
* [Ranking and Personalization](https://help.nosto.com/en/articles/7168969-merchandising-search-personalization-guide) Ranking and Personalization - manage how results are ranked,
* Synonyms, Redirects, and other search features are also managed through Nosto Dashboard (my.nosto.com).
