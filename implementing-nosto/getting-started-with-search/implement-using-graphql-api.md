# Implement using GraphQL API

## Making search API requests <a href="#making-search-api-requests" id="making-search-api-requests"></a>

{% hint style="info" %}
GraphQL search endpoint: `https://search.nosto.com/v1/graphql`
{% endhint %}

The request body accepts `accountId` variable, available in the Nosto dashboard.

Replace `accountId` variable with your account id, retrieved from the Nosto dashboard, in the following example request:

```bash
curl -0 -v -X POST 'https://search.nosto.com/v1/graphql' \
-H 'Content-Type: application/json' \
-d @- << EOF
{
  "query": "
    query {
      search(accountId: "YOUR_ACCOUNT_ID", name: serp, query: "green") {
        products {
          hits {
            productId
            name
          }
          total
          size
          from
          facets {
            ... on SearchStatsFacet {
              id
              field
              type
              name
              min
              max
            }
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
  "
}
EOF
```

API will return a response with a 200 status code, it may include an error with an explanation in the following format:

```json
{
  "data": {
    "search": {
      "products": {
        "hits": [
          {
            "productId": "",
            ...
          }
        ]
      }
    }
  },
  "errors": [
    {
      "message": ""
    }
  ]
}
```

{% hint style="info" %}
Further examples will only include GraphQL queries.
{% endhint %}

## GraphQL playground <a href="#graphql-playground" id="graphql-playground"></a>

[GraphQL playground](https://search.nosto.com/v1/graphql) is an interactive GraphQL query tool where you can create queries interactively and send search requests straight to our search engine. It provides:

1. GraphQL search request schema - you can see field types and inspect what fields are needed for a search request.
2. Autocomplete - while forming a search request you can autocomplete necessary fields.
3. Send search requests to your shop and preview the response.

## Search documents <a href="#search-documents" id="search-documents"></a>

## Selecting fields <a href="#selecting-fields" id="selecting-fields"></a>

For a basic search, it’s enough to provide `accountId`, `name`, `query,` and select fields that GraphQL should return. You can control which product attributes to select through `products.hits` field. For example, if you want to return only `productId` and `name`, the query would be:

```graphql
query {
  search(accountId: "YOUR_ACCOUNT_ID", name: serp, query: "green") {
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

#### Query parameters

* `accountId` - Nosto account id.
* `name` - query name (`serp` or `autocomplete`).
* `query` - search input.

{% hint style="info" %}
SERP stands for Search Engine Results Page
{% endhint %}

#### Query select fields

* `products.hits` - used to specify which product fields should be returned.
* `products.total` - how many products are found for query.
* `products.size` - the count of products returned per single request.
* `products.from` - what is the offset of returned products.

Other fields can be autocompleted once the GraphQL schema is fetched, or you can check the schema at `https://search.nosto.com/v1/graphql` playground.

### Pagination and size <a href="#pagination-and-size" id="pagination-and-size"></a>

Products offset parameter `from` is used for pagination functionality.

The **default** count of documents returned per page is `size = 5`, you can change it with `products.size`, and offset of products is controlled with `products.from` field:

#### Query

```graphql
query {
  search(
    accountId: "YOUR_ACCOUNT_ID"
    name: serp
    query: "green"
    products: { size: 10, from: 10 }
  ) {
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

#### Response

```json
{
  "data": {
    "search": {
      "products": {
        "hits": [
          {
            "productId": "",
            ...
          },
          ...
        ],
        "total": 300,
        "size": 10,
        "from": 10,
      }
    }
  }
}
```

### Sorting <a href="#sorting" id="sorting"></a>

By default results are sorted by products relevance score. To change the sorting, use the sort parameter, where you would specify the field which should be sorted by, and order: `asc` for ascending and **** `desc` **** for descending.

By default, you should always sort by relevance. Only if the user selects a different sort method, a sorting rule should be used.

### Query

```graphql
query {
    search(
      accountId: "YOUR_ACCOUNT_ID"
      name: serp
      query: "green"
      sort: [
        {
          field: "price"
          order: asc
        }
      ]
  ) {
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

### Faceting <a href="#faceting" id="faceting"></a>

Facets help the user to find products more easily. Faceted navigation is normally found in the sidebar of a website and contains filters only relevant to the current search query. Facets are configured in the Nosto dashboard

#### **Configuring facet**

You can configure new facet in Nosto dashboard Facet Manager. Before creating a facet you need to ensure that attribute is indexed. You can do this in [Nosto Dashboard](https://my.nosto.com/) → Search & Categories → Settings → Indexed Fields.

When attribute is marked as indexed you can create facet of that field:

Go to [Nosto Dashboard](https://my.nosto.com/) → Search & Categories → Settings → Facet Manager

> Before creating a facet you need to ensure that attribute is indexed.

In Facet Manager page:

1. Specify attribute upon which facet should be created.
2. Create a name for facet which will be rendered in Search Page.
3. Depending on field type a facet type will be selected:
   * Select group - when String type field will be selected.
   * Range - when Number type field will be selected.
4. Select the sorting type that facet will be ordered by.

**Querying facet**

One of the facet type is `type = terms` . Assume that we have configured facets for `customFields.brandname` and `categories`

```graphql
query {
  search(
    accountId: "YOUR_ACCOUNT_ID"
    name: serp
    query: "green"
  ) {
    products {
      hits {
        productId
        name
        price
      }
      total
      size
      from
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

#### Response

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
        "total": 300,
        "size": 10,
        "from": 10,
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
      name: serp
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
        total
        size
        from
      }
    }
  }
```

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
        "total": 300,
        "size": 5,
        "from": 0,
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

### Filter <a href="#filter" id="filter"></a>

Filtering by `terms` facet, for example by _Adidas_ brand:

```graphql
query {
  search(
    accountId: "YOUR_ACCOUNT_ID"
    name: serp
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
      total
      size
      from
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

When filtering by multiple same field items filters will be joined with OR operator and different fields with AND.

If you wish to have more facets, you should configure it in the Nosto dashboard first.

Filtering by `stats` field, for example by price:

```graphql
query {
    search(
      accountId: "YOUR_ACCOUNT_ID"
      name: serp
      query: "green"
      products: {
        filter: [{
          field: "price"
          range: {
            lt: "60"
            gt: "50"
          }
        }]
      }
  ) {
      products {
        hits {
          productId
          name
        },
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
        total
        size
        from
      }
    }
  }
```

You can sort using these arguments: `lt` (less than), `gt` (greater than), `lte` (less than or equal to), `gte` (greater than or equal to).

#### Segments <a href="#segments" id="segments"></a>

You may want to provide a segment to the request to match specific ranking rules that target specific users:

```graphql
query {
    search(
      accountId: "YOUR_ACCOUNT_ID"
      name: serp
      query: "green"
      segments: ["segment-nikebuyers"]
  ) {
      products {
        hits {
          productId
          name
          price
        }
        total
        size
        from
      }
    }
  }
```

### Autocomplete <a href="#autocomplete" id="autocomplete"></a>

Autocomplete is an element shown under search input used to display products for a partial query. The same GraphQL API should be used with a smaller `size` argument:

#### Query

```graphql
query {
  search(
    accountId: "YOUR_ACCOUNT_ID"
    name: autocomplete
    query: "green"
    products: { size: 4 }
  ) {
    products {
      hits {
        productId
        name
      }
      total
      size
    }
    query
  }
}
```

#### Response

```json
{
  "data": {
    "search": {
      "query": "green",
      "products": {
        "hits": [
          {
            ...
          }
        ]
        "size": 4,
        "total": 300
      }
    }
  }
}
```

{% hint style="info" %}
Differently than in the search results page, argument `name = autocomplete` is passed in autocomplete requests.
{% endhint %}

## Category <a href="#category" id="category"></a>

Nosto provides functionality to get all products from specific category. This is useful when you want to implement category merchandising through the same search GraphQL API.

An empty search query should be provided and `categoryId`:

### Query

```graphql
query {
  search(
    accountId: "YOUR_ACCOUNT_ID"
    name: serp
    products: { categoryId: "123456789" }
  ) {
    products {
      hits {
        productId
        price
        categoryIds
        name
      }
      total
      size
      categoryId
    }
  }
}
```

### Response

```json
{
  "data": {
    "search": {
      "products": {
        "hits": [
          {
            "productId": "",
            "price": 12.99,
            "categoryIds": ["123456789"],
            "name": "Blue T-shirt"
          },
          ...
        ],
        "total": 370,
        "size": 5,
        "categoryId": "123456789"
      }
    }
  }
}
```

You can also query by full category name using `categoryPath`:

#### Query

```graphql
query {
  search(
    accountId: "YOUR_ACCOUNT_ID"
    name: serp
    products: { categoryPath: "T-Shirts" }
  ) {
    products {
      hits {
        productId
        price
        categories
        name
      }
      total
      size
    }
  }
}
```

#### Response

```json
{
  "data": {
    "search": {
      "products": {
        "hits": [
          {
            "productId": "",
            "price": 12.99,
            "categories": ["T-Shirts"],
            "name": "Blue T-shirt"
          }
        ],
        "total": 370,
        "size": 5
      }
    }
  }
}
```

An empty query should be sent when using category merchandising. Also, you must use the same category values as you provide in products data export.
