# Implement Search results page

## API Requests <a href="#autocomplete" id="autocomplete"></a>

### Searching <a href="#selecting-fields" id="selecting-fields"></a>

For a basic search, it’s enough to provide `accountId`, `query,` and select fields that should be returned. You can control which product attributes to select through `products.hits` field. See [all available fields](https://search.nosto.com/v1/graphql?ref=SearchProduct).

#### Query

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

[Playground example](https://search.nosto.com/v1/graphql?query=%7B%0A%20%20search\(accountId:%20%22YOUR_ACCOUNT_ID%22,%20query:%20%22green%22\)%20%7B%0A%20%20%20%20products%20%7B%0A%20%20%20%20%20%20hits%20%7B%0A%20%20%20%20%20%20%20%20productId%0A%20%20%20%20%20%20%20%20name%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20total%0A%20%20%20%20%20%20size%0A%20%20%20%20%20%20from%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D)

#### Query parameters:

<table data-header-hidden><thead><tr><th width="177">name</th><th>description</th><th data-hidden></th></tr></thead><tbody><tr><td><strong>accountId</strong></td><td>Nosto account ID</td><td></td></tr><tr><td><strong>query</strong></td><td>search query text</td><td></td></tr></tbody></table>

See all [query parameters](https://search.nosto.com/v1/graphql?ref=InputSearchQuery).

### Pagination and size <a href="#pagination-and-size" id="pagination-and-size"></a>

Products offset parameter `from` is used for pagination functionality.

The **default** count of documents returned per page is `size = 5`, you can change it with `products.size`, and offset of products is controlled with `products.from` field:

{% hint style="info" %}
Up to 250 products can be retrieved in a single page, corresponding to `size = 250`.
{% endhint %}

The `total` value in the response is useful for pagination as well:

* `total / products.size` is the number of available pages with the current page size.
* `products.from + products.size >= total` is `true` when the last page has been reached. This is particularly useful for infinite scrolling/load more solutions.

#### Query

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

[Playground example](https://search.nosto.com/v1/graphql?query=%7B%0A%20%20search\(%0A%20%20%20%20accountId:%20%22YOUR_ACCOUNT_ID%22%0A%20%20%20%20query:%20%22green%22%0A%20%20%20%20products:%20%7Bsize:%2010,%20from:%2010%7D%0A%20%20\)%20%7B%0A%20%20%20%20products%20%7B%0A%20%20%20%20%20%20hits%20%7B%0A%20%20%20%20%20%20%20%20productId%0A%20%20%20%20%20%20%20%20name%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20total%0A%20%20%20%20%20%20size%0A%20%20%20%20%20%20from%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D)

### Sorting <a href="#sorting" id="sorting"></a>

By default results are sorted by products relevance score.

To change the sorting, use the sort parameter, where you would specify any indexed field which should be sorted by, and order: `asc` for ascending and `desc` for descending.
Top-level product fields can be used for sorting using [documented field names](https://search.nosto.com/v1/graphql?ref=SearchProduct).
To sort by a custom field, prefix the custom field name with `customFields.`.
Likewise, extracted fields need to be prefixed with `extra.` to use them for sorting.

By default, you should always sort by relevance and merchandising rules, which is achieved by not specifying any sort parameter. Only if the user selects a different sort method, a sorting rule should be used.

{% hint style="info" %}
When sorting by one or more fields, only the field(s) dictate the order of products. Merchandising rules have no effect.
{% endhint %}

#### Query

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

[Playground example](https://search.nosto.com/v1/graphql?query=%7B%0A%20%20search\(%0A%20%20%20%20accountId:%20%22YOUR_ACCOUNT_ID%22%0A%20%20%20%20query:%20%22green%22%0A%20%20%20%20products:%20%7B%0A%20%20%20%20%20%20sort:%20%5B%7Bfield:%20%22price%22,%20order:%20asc%7D%5D%0A%20%20%20%20%7D%0A%20%20\)%20%7B%0A%20%20%20%20products%20%7B%0A%20%20%20%20%20%20hits%20%7B%0A%20%20%20%20%20%20%20%20productId%0A%20%20%20%20%20%20%20%20name%0A%20%20%20%20%20%20%20%20price%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D)

{% hint style="info" %}
Some search implementations are more straightforward if sorting is defined in all search queries, even ones that use default sorting (by relevance and rules).
Sorting by field `_score` with order `desc` is equivalent to omitting the sort parameter entirely.
{% endhint %}

### Faceting <a href="#faceting" id="faceting"></a>

Facets help the user to find products more easily. Faceted navigation is normally found in the sidebar of a website and contains filters only relevant to the current search query. Facets are configured in the Nosto dashboard.

<div><figure><img src="../../../.gitbook/assets/image (8).png" alt=""><figcaption><p>Terms facet</p></figcaption></figure> <figure><img src="../../../.gitbook/assets/image (6).png" alt=""><figcaption><p>Stats facet</p></figcaption></figure></div>

{% hint style="info" %}
To use facet for a specific field you need to [configure it in the Nosto dashboard](https://help.nosto.com/en/articles/7169091-setting-up-facets) first.
{% endhint %}

#### **Terms facet**

One of the facet types is `type = terms`. It returns list if common terms from found documents.

#### **Query**

Assume that we have configured facets for `customFields.brandname` and `categories:`

```graphql
query {
  search(
    accountId: "YOUR_ACCOUNT_ID"
    query: "green"
  ) {
    products {
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

[Playground example](https://search.nosto.com/v1/graphql?query=%7B%0A%20search\(accountId:%20%22YOUR_ACCOUNT_ID%22%20query:%20%22green%22\)%20%7B%0A%20%20products%20%7B%0A%20%20%20hits%20%7B%20productId%20name%20%7D%0A%20%20%20%20facets%20%7B%0A%20%20%20%20%20...%20on%20SearchTermsFacet%20%7B%0A%20%20%20%20%20%20id%0A%20%20%20%20%20%20field%0A%20%20%20%20%20%20type%0A%20%20%20%20%20%20name%0A%20%20%20%20%20%20data%20%7B%20value%20count%20selected%20%7D%0A%20%20%20%20%7D%0A%20%20%20%7D%0A%20%20%7D%0A%20%7D%0A%7D)

#### Response

```json
{
  "data": {
    "search": {
      "products": {
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

#### Response parameters:

<table data-header-hidden><thead><tr><th width="161">name</th><th>description</th></tr></thead><tbody><tr><td><strong>id</strong></td><td>internal facet ID, used to select <a href="https://search.nosto.com/v1/graphql?ref=InputSearchProducts.facets">specific facets in query</a></td></tr><tr><td><strong>field</strong></td><td>facet field, should be used for <a href="https://search.nosto.com/v1/graphql?ref=InputSearchTopLevelFilter">filtering</a></td></tr><tr><td><strong>type</strong></td><td>facet type, in this case <code>terms</code></td></tr><tr><td><strong>name</strong></td><td>user friendly facet name configured in the <a href="https://help.nosto.com/en/articles/7169091-setting-up-facets">dashboard</a></td></tr><tr><td><strong>data.value</strong></td><td>original facet value, it should be displayed in the user interface</td></tr><tr><td><strong>data.count</strong></td><td>shows how many products will be returned if you select this facet, it should be displayed in the user interface</td></tr><tr><td><strong>data.selected</strong></td><td>indicates if there is an active filter on this value</td></tr></tbody></table>

#### Stats facet

Stats facet returns minimum and maximum number field value from found documents. The most common usage is to render slider filter (e.g. price)/

#### Query

```graphql
query {
    search(
      accountId: "YOUR_ACCOUNT_ID"
      query: "green"
  ) {
      products {
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

[GraphQL playground example](https://search.nosto.com/v1/graphql?query=%7B%0A%09search\(accountId:%20%22YOUR_ACCOUNT_ID%22%20query:%20%22green%22\)%20%7B%0A%09%09products%20%7B%0A%09%09%09hits%20%7B%20productId%20name%20price%20%7D%0A%09%09%09facets%20%7B%0A%09%09%09%09...%20on%20SearchStatsFacet%20%7B%0A%09%09%09%09%09id%0A%09%09%09%09%09field%0A%09%09%09%09%09type%0A%09%09%09%09%09name%0A%09%09%09%09%09min%0A%09%09%09%09%09max%0A%09%09%09%09%7D%0A%09%09%09%7D%0A%09%09%7D%0A%09%7D%0A%7D)

#### **Response**

```json
{
  "data": {
    "search": {
      "products": {
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

#### Response parameters:

<table data-header-hidden><thead><tr><th width="112">name</th><th>description</th></tr></thead><tbody><tr><td><strong>name</strong></td><td>user friendly facet name configured in the <a href="https://help.nosto.com/en/articles/7169091-setting-up-facets">dashboard</a></td></tr><tr><td><strong>terms</strong></td><td>facet type, in this case <code>stats</code></td></tr><tr><td><strong>field</strong></td><td>facet field, should be used for <a href="https://search.nosto.com/v1/graphql?ref=InputSearchTopLevelFilter">filtering</a></td></tr><tr><td><strong>id</strong></td><td>internal facet ID, used to select <a href="https://search.nosto.com/v1/graphql?ref=InputSearchProducts.facets">specific facets in query</a></td></tr><tr><td><strong>min</strong></td><td>minimum field value for documents that match provided query</td></tr><tr><td><strong>max</strong></td><td>maximum field value for documents that match provided query</td></tr></tbody></table>

### Filter <a href="#filter" id="filter"></a>

Filtering by `terms` facet, for example by _Adidas, Converse_ brands:

### Query

```graphql
query {
  search(
    accountId: "YOUR_ACCOUNT_ID"
    query: "green"
    products: {
      size: 10
      filter: [{ field: "brand", value: ["Adidas", "Converse"] }]
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

[GraphQL playground example](https://search.nosto.com/v1/graphql?query=%7B%0A%20search\(%0A%20%20accountId:%20%22YOUR_ACCOUNT_ID%22%20query:%20%22green%22%0A%20%20products:%20%7B%20filter:%20%5B%7B%20field:%20%22customFields.brandname%22,%20value:%20%22Adidas%22%20%7D%5D%20%7D%0A\)%20%7B%0A%20%20products%20%7B%0A%20%20%20hits%20%7B%20productId%20name%20%7D%0A%20%20%20facets%20%7B%0A%20%20%20%20...%20on%20SearchTermsFacet%20%7B%20field%20name%20data%20%7B%20value%20count%20selected%20%7D%20%7D%0A%20%20%20%7D%0A%20%20%7D%0A%20%7D%0A%7D)

When filtering by multiple same field items, filters will be joined with OR operator and different fields with AND.

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

[GraphQL playground example](https://search.nosto.com/v1/graphql?query=%7B%0A%20search\(%0A%20%20accountId:%20%22YOUR_ACCOUNT_ID%22%20query:%20%22green%22%0A%20%20products:%20%7Bfilter:%20%5B%7Bfield:%20%22price%22,%20range:%20%7Blt:%20%2260%22,%20gt:%20%2250%22%7D%7D%5D%7D%0A\)%20%7B%0A%20%20products%20%7B%0A%20%20%20hits%20%7B%20productId%20name%20%7D%0A%20%20%20facets%20%7B%0A%20%20%20%20...%20on%20SearchStatsFacet%20%7B%20field%20name%20min%20max%20%7D%0A%20%20%20%7D%0A%20%20%7D%0A%20%7D%0A%7D)

You can sort using these arguments: `lt` (less than), `gt` (greater than), `lte` (less than or equal to), `gte` (greater than or equal to).

{% hint style="info" %}
Filters in requests take precedence over merchandising rules. Filtered products can't be brought back using pinning.
{% endhint %}

### Redirects

Redirects can be used to forward users to special pages depending on their search keywords. For example, users searching for `shipping` could be forwarded to https://example.com/shipping.html.

{% hint style="warning" %}
For API integrations GraphQL can only return the target URL. The actual browser redirect must be implemented by the merchant.
{% endhint %}

#### Query

```graphql
query {
  search(
    accountId: "YOUR_MERCHANT_ID",
    query: "shipping"
  ) {
    redirect
    products {
      hits {
        name
      }
    }
    keywords {
      hits {
        keyword
      }
    }
  }
}
```

[GraphQL playground example](https://search.nosto.com/v1/graphql?query=query%20%7B%0A%20%20search%28%0A%20%20%20%20accountId%3A%20%22YOUR_MERCHANT_ID%22%2C%0A%20%20%20%20query%3A%20%22shipping%22%0A%20%20%29%20%7B%0A%20%20%20%20redirect%0A%20%20%20%20products%20%7B%0A%20%20%20%20%20%20hits%20%7B%0A%20%20%20%20%20%20%20%20name%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%20%20%20%20keywords%20%7B%0A%20%20%20%20%20%20hits%20%7B%0A%20%20%20%20%20%20%20%20keyword%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D)

#### Response

```json
{
  "data": {
    "search": {
      "redirect": "https://example.com/shipping.html",
      "products": {
        "hits": []
      },
      "keywords": null
    }
  }
}
```

### Price Formatting and Currency Display <a href="#price-formatting" id="price-formatting"></a>

You can request specific currency formatting settings for prices returned in the search results. This is done by specifying the `currencyFormat` parameter within the `products` input. The actual formatting details (like currency symbol, placement, decimal places) are then returned in the `priceFormat` field within the `products` object of the response.

#### Query

To select which _pre-configured_ currency settings to retrieve, include the `currencyFormat` parameter within the `products` input. Additionally, ensure you request the `priceFormat` field in your query to receive these details.

```graphql
query (
  $accountId: String,
  $products: InputSearchProducts,
) {
   search(
    accountId: $accountId
    products: $products
  ) {
     products {
      # This field will contain the details of the selected currency format
      priceFormat {
        currencySymbol
        placement
        decimalPlaces
        decimalSeparator
        thousandSeparator
      }
    }
  }
}
```

**Variables Example:**

```json
{
  "accountId": "shopify-55872454679-538837015-fi",
  "products": {
    "currencyFormat": "EUR"
  }
}
```

#### Behavior and Error Handling:

* If `currencyFormat` is not provided in the `products` input, the default currency format configured for the account will be used for the `priceFormat` field.
* If `currencyFormat` is provided but corresponds to a currency for which no settings are configured, an error will be returned.
* If `currencyFormat` is not provided and no default currency format exists for the account, an error will be returned.

#### Response Example:

```json
{
  "data": {
    "search": {
      "products": {
        "priceFormat": {
          "currencySymbol": "€",
          "placement": "after",
          "decimalPlaces": 2,
          "decimalSeparator": ",",
          "thousandSeparator": " "
        }
      }
    }
  }
}
```

#### `priceFormat` Response Parameters:

These parameters describe how the prices should be formatted on the frontend based on the selected `currencyFormat`.

| Name                  | Description                                                                                |
| --------------------- | ------------------------------------------------------------------------------------------ |
| **currencySymbol**    | The symbol for the currency (e.g., "$", "€").                                              |
| **placement**         | Indicates where the currency symbol is placed relative to the price ("before" or "after"). |
| **decimalPlaces**     | The number of decimal places to display for the price.                                     |
| **decimalSeparator**  | The character used to separate the decimal part of the price (e.g., ".", ",").             |
| **thousandSeparator** | The character used to separate thousands in the price (e.g., ",", " ").                    |

## Session params <a href="#session-params" id="session-params"></a>

For features like personalized results and user segments to function effectively, the search function needs access to the user's session information. Session information can be [queried from the session API](analytics-personalization-ab-testing.md#query-session).

Alternatively, it's possible to get search session data using the [JS API](../search/#session-parameters):

```javascript
nostojs(api => {
    api.getSearchSessionParams().then(response => {
        console.log(response);
    });
});
```

The results of this function should be passed to search query [sessionParams](https://search.nosto.com/v1/graphql?ref=InputSearchQuery) parameter. In case search is called from backend, it should pass this data to backend (e.g. using [form data](https://developer.mozilla.org/en-US/docs/Learn/Forms/Sending_and_retrieving_form_data)).

## Analytics

### Nosto Analytics

{% include "../../../.gitbook/includes/analytics-hint.md" %}

## Search engine configuration <a href="#selecting-fields" id="selecting-fields"></a>

Nosto Search engine is relevant out of the box and search API can be used without any initial setup. Nosto Dashboard can be used to further tune search engine configuration:

* [Searchable Fields](https://help.nosto.com/en/articles/7161528-search-engine-s-logic-and-searchable-fields) - manage which fields are used for search and their priorities,
* [Facets](https://help.nosto.com/en/articles/7169091-setting-up-facets) - create facets (filtering options) for search results page,
* [Ranking and Personalization](https://help.nosto.com/en/articles/7168969-merchandising-search-personalization-guide) Ranking and Personalization - manage how results are ranked,
* Synonyms, Redirects, and other search features are also managed through Nosto Dashboard ([my.nosto.com](https://my.nosto.com/)).
