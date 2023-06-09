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

[Playground example](https://search.nosto.com/v1/graphql?query=%7B%0A%20%20search\(accountId:%20%22YOUR\_ACCOUNT\_ID%22,%20query:%20%22green%22\)%20%7B%0A%20%20%20%20products%20%7B%0A%20%20%20%20%20%20hits%20%7B%0A%20%20%20%20%20%20%20%20productId%0A%20%20%20%20%20%20%20%20name%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20total%0A%20%20%20%20%20%20size%0A%20%20%20%20%20%20from%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D)

#### Query parameters:

<table data-header-hidden><thead><tr><th width="177">name</th><th>description</th><th data-hidden></th></tr></thead><tbody><tr><td><strong>accountId</strong></td><td>Nosto account ID</td><td></td></tr><tr><td><strong>query</strong></td><td>search query text</td><td></td></tr></tbody></table>

See all [query parameters](https://search.nosto.com/v1/graphql?ref=InputSearchQuery).

### Tracking

Query above is considered as search event and it should be tracked using [JS API library helper](../../../apis/js-apis/search.md#search). Additionally, query submit in autocomplete should tracked with [submit helper](../../../apis/js-apis/search.md#search-form-submit).

Product clicks in SERP should be tracked with [click helper](../../../apis/js-apis/search.md#search-product-click) where `type = serp`.

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

[Playground example](https://search.nosto.com/v1/graphql?query=%7B%0A%20%20search\(%0A%20%20%20%20accountId:%20%22YOUR\_ACCOUNT\_ID%22%0A%20%20%20%20query:%20%22green%22%0A%20%20%20%20products:%20%7Bsize:%2010,%20from:%2010%7D%0A%20%20\)%20%7B%0A%20%20%20%20products%20%7B%0A%20%20%20%20%20%20hits%20%7B%0A%20%20%20%20%20%20%20%20productId%0A%20%20%20%20%20%20%20%20name%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%20%20total%0A%20%20%20%20%20%20size%0A%20%20%20%20%20%20from%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D)

## Sorting <a href="#sorting" id="sorting"></a>

By default results are sorted by products relevance score. &#x20;

To change the sorting, use the sort parameter, where you would specify the field which should be sorted by, and order: `asc` for ascending and `desc` for descending.

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

[Playground example](https://search.nosto.com/v1/graphql?query=%7B%0A%20%20search\(%0A%20%20%20%20accountId:%20%22YOUR\_ACCOUNT\_ID%22%0A%20%20%20%20query:%20%22green%22%0A%20%20%20%20products:%20%7B%0A%20%20%20%20%20%20sort:%20%5B%7Bfield:%20%22price%22,%20order:%20asc%7D%5D%0A%20%20%20%20%7D%0A%20%20\)%20%7B%0A%20%20%20%20products%20%7B%0A%20%20%20%20%20%20hits%20%7B%0A%20%20%20%20%20%20%20%20productId%0A%20%20%20%20%20%20%20%20name%0A%20%20%20%20%20%20%20%20price%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D)

## Faceting <a href="#faceting" id="faceting"></a>

Facets help the user to find products more easily. Faceted navigation is normally found in the sidebar of a website and contains filters only relevant to the current search query. Facets are configured in the Nosto dashboard.

<div>

<figure><img src="../../../.gitbook/assets/image (8).png" alt=""><figcaption><p>Terms facet</p></figcaption></figure>

 

<figure><img src="../../../.gitbook/assets/image (6).png" alt=""><figcaption><p>Stats facet</p></figcaption></figure>

</div>

{% hint style="info" %}
To use facet for a specific field you need to [configure it in the Nosto dashboard](https://help.nosto.com/en/articles/7169091-setting-up-facets) first.
{% endhint %}

### **Terms facet**

One of the facet types is `type = terms`.  It returns list if common terms from found documents.

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

[Playground example](https://search.nosto.com/v1/graphql?query=%7B%0A%20search\(accountId:%20%22YOUR\_ACCOUNT\_ID%22%20query:%20%22green%22\)%20%7B%0A%20%20products%20%7B%0A%20%20%20hits%20%7B%20productId%20name%20%7D%0A%20%20%20%20facets%20%7B%0A%20%20%20%20%20...%20on%20SearchTermsFacet%20%7B%0A%20%20%20%20%20%20id%0A%20%20%20%20%20%20field%0A%20%20%20%20%20%20type%0A%20%20%20%20%20%20name%0A%20%20%20%20%20%20data%20%7B%20value%20count%20selected%20%7D%0A%20%20%20%20%7D%0A%20%20%20%7D%0A%20%20%7D%0A%20%7D%0A%7D)

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

### Stats facet

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

[GraphQL playground example](https://search.nosto.com/v1/graphql?query=%7B%0A%09search\(accountId:%20%22YOUR\_ACCOUNT\_ID%22%20query:%20%22green%22\)%20%7B%0A%09%09products%20%7B%0A%09%09%09hits%20%7B%20productId%20name%20price%20%7D%0A%09%09%09facets%20%7B%0A%09%09%09%09...%20on%20SearchStatsFacet%20%7B%0A%09%09%09%09%09id%0A%09%09%09%09%09field%0A%09%09%09%09%09type%0A%09%09%09%09%09name%0A%09%09%09%09%09min%0A%09%09%09%09%09max%0A%09%09%09%09%7D%0A%09%09%09%7D%0A%09%09%7D%0A%09%7D%0A%7D)

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

<table data-header-hidden><thead><tr><th width="112">name</th><th>description</th></tr></thead><tbody><tr><td><strong>name</strong></td><td>user friendly facet name configured in the <a href="https://help.nosto.com/en/articles/7169091-setting-up-facets">dashboard</a></td></tr><tr><td><strong>terms</strong></td><td>facet type, in this case <code>stats</code></td></tr><tr><td><strong>field</strong></td><td>facet field, should be used for <a href="https://search.nosto.com/v1/graphql?ref=InputSearchTopLevelFilter">filtering</a></td></tr><tr><td><strong>id</strong></td><td>internal facet ID, used to select <a href="https://search.nosto.com/v1/graphql?ref=InputSearchProducts.facets">specific facets in query</a></td></tr><tr><td><strong>min</strong></td><td>minimum field value for documents that match provided query</td></tr><tr><td><strong>max</strong></td><td>maximum field  value for documents that match provided query</td></tr></tbody></table>

### Tracking

Sorting, paginating, faceting are considered as search events and it should be tracked using [JS API library helper](../../../apis/js-apis/search.md#search).

## Filter <a href="#filter" id="filter"></a>

Filtering by `terms` facet, for example by _Adidas_ brand:

### Query

```graphql
query {
  search(
    accountId: "YOUR_ACCOUNT_ID"
    query: "green"
    products: {
      size: 10
      filter: [{ field: "brand", value: "Adidas" }]
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

### Tracking

Filtering is considered as search event and it should be tracked using [JS API library helper](../../../apis/js-apis/search.md#search).

## Session params <a href="#selecting-fields" id="selecting-fields"></a>

For some of the search features to work properly, such as personalised results and segments, the search function needs to be able to access information about the user's session from the front-end.

It's possible to get search session data using the [JS API](https://docs.nosto.com/techdocs/apis/js-apis/search):

```javascript
nostojs(function(api) {
    api.getSearchSessionParams().then(function(response) {
        console.log(response);
    });
});
```

The results of this function should be passed to search query [sessionParams](https://search.nosto.com/v1/graphql?ref=InputSearchQuery) parameter. In case search is called from backend, it should pass this data to backend (e.g. using [form data](https://developer.mozilla.org/en-US/docs/Learn/Forms/Sending\_and\_retrieving\_form\_data)).

## Search engine configuration <a href="#selecting-fields" id="selecting-fields"></a>

Nosto Search engine is relevant out of the box and search API can be used without any initial setup. Nosto Dashboard can be used to further tune search engine configuration:

* [Searchable Fields](https://help.nosto.com/en/articles/7161528-search-engine-s-logic-and-searchable-fields) - manage which fields are used for search and their priorities,
* [Facets](https://help.nosto.com/en/articles/7169091-setting-up-facets) - create facets (filtering options) for search results page,
* [Ranking and Personalization](https://help.nosto.com/en/articles/7168969-merchandising-search-personalization-guide) Ranking and Personalization - manage how results are ranked,
* Synonyms, Redirects, and other search features are also managed through Nosto Dashboard (my.nosto.com).
