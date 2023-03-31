# Using the Search API

## What is GraphQL? <a href="#graphql-playground" id="graphql-playground"></a>

GraphQL is a query language for APIs. What does this mean for you? Unlike regular SOAP or REST APIs, GraphQL gives you the ultimate flexibility in being able to specify in your API requests specifically what data you need and get back exactly that.

As a query language, it provides you with a lot of flexibility that most normal APIs will not. Without needing to recreate endpoints, you can provide developers with the same functionality as a bulk endpoint. Your queries will be cleaner and easier to understand by combining multiple queries into one request.

Lear more on [https://graphql.org/learn/](https://graphql.org/learn/).

### What’s the difference between GraphQL API and the regular API?

The regular API is very well structured and specifically defined. The endpoints have their set requests and responses and that’s what you get whether or not that matches your usage pattern. GraphQL lets you control all of this so that the way you consume the data matches exactly what you need.

This is both a pro and a con. If your use case does not require all of the data, GraphQL can speed up your requests as we do less work on the server-side to fulfill those requests. Conversely, if you need all of the data in one request, your requests could slow down as we do more work to fulfill these requests.

### GraphQL playground <a href="#graphql-playground" id="graphql-playground"></a>

[GraphQL playground](https://search.nosto.com/v1/graphql) is an interactive GraphQL query tool where you can create queries interactively and send search requests straight to our search engine. It provides:

1. [GraphQL search request schema](https://search.nosto.com/v1/graphql?ref=Query) - you can see field types and inspect what fields are needed for a search request.
2. [GraphQL search result schema](https://search.nosto.com/v1/graphql?ref=SearchResult) - you can see return field types with descriptions.
3. Autocomplete - while forming a search request you can autocomplete available fields.
4. Send search requests to your shop and preview the response.

## Making search API requests <a href="#making-search-api-requests" id="making-search-api-requests"></a>

### API endpoint

Search GraphQL use different API endpoint than other nosto queries: `https://search.nosto.com/v1/graphq`

### Request example

{% tabs %}
{% tab title="Curl" %}
```bash
curl -X POST 'https://search.nosto.com/v1/graphql' \
-H 'Content-Type: application/json' \
-d @- << EOF
{
  "query": "query (\$query: String) { search (accountId: \"ACCOUNT ID\", query: \$query) { products { hits { name } total } } }",
  "variables": {
    "query": "green"
  }
}
EOF
```

Replace ACCOUNT ID with your account id, retrieved from the Nosto dashboard.
{% endtab %}

{% tab title="Javascript" %}
```javascript
fetch('https://search.nosto.com/v1/graphql', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
  },
  body: JSON.stringify({
    query: `query ($query: String) {
      search (accountId: "ACCOUNT ID", query: $query) {
        products {
          hits {
            name
          }
          total
        }
      }
    }`,
    variables: {
      query: "green"
    },
  }),
})
  .then((res) => res.json())
  .then((result) => console.log(result))
```

Replace ACCOUNT ID with your account id, retrieved from the Nosto dashboard.
{% endtab %}

{% tab title="Python" %}
```python
import requests

r = requests.post(
    "https://search.nosto.com/v1/graphql",
    headers={
        "Content-Type": "application/json"
    },
    json={
        "query": """query ($query: String) {
            search (accountId: "ACCOUNT ID", query: $query) {
                products {
                hits {
                    name
                }
                total
                }
            }
        }""",
        "variables": {
            "query": "green"
        }
    }
)
print(r.json())
```

Replace ACCOUNT ID with your account id, retrieved from the Nosto dashboard.
{% endtab %}

{% tab title="PHP" %}
```php
$ch = curl_init();

curl_setopt($ch, CURLOPT_RETURNTRANSFER, 0);
curl_setopt($ch, CURLOPT_URL, "https://search.nosto.com/v1/graphql");
curl_setopt($ch, CURLOPT_POST, 1);
curl_setopt($ch, CURLOPT_HTTPHEADER, array("Content-Type: application/json"));
curl_setopt($ch, CURLOPT_POSTFIELDS, json_encode(array(
    "query" => <<<EOT
        query (\$query: String) {
            search (accountId: "ACCOUNT ID", query: \$query) {
                products {
                hits {
                    name
                }
                total
                }
            }
        }
    EOT,
    "variables" => array(
        "query" => "green"
    )
)));

$result = curl_exec($ch);
var_dump($result);
```

Replace ACCOUNT ID with your account id, retrieved from the Nosto dashboard.
{% endtab %}
{% endtabs %}

### Response example

API on successful request will return a `200` status code response which includes search data:

```json
{
  "data": {
    "search": {
      "products": {
        "hits": [
          {
            "name": "My product name",
          }
        ],
        "total": 1
      }
    }
  }
}
```

### Error handling

### Error response

API returns list of errors with explanations. These errors should be logged internally and not displayed to end user.

```json
{
  "data": {
    ...
  },
  "errors": [
    {
      "message": ""
    }
  ]
}
```

### Partial response

API may return some errors even when data is returned. That means some part of response may be missing, but you should still display it instead of showing error to our user. These errors should be logged to internal logging.&#x20;

### Production example <a href="#autocomplete" id="autocomplete"></a>

In the search application you should use GraphQL variables instead of hardcoded arguments to pass search data, meaning `filters`, `sort`, `size`, `from` options should be passed in `products` variable (see [InputSearchProducts reference](https://search.nosto.com/v1/graphql?ref=InputSearchProducts) for all available options):

```graphql
query (
  $acountId: String,
  $query: String,
  $segments: [String!],
  $products: InputSearchProducts
) {
  search(
    acountId: "ACCOUNT ID"
    query: $query
    segments: $segments
    products: $products
  ) {
    query
    products {
      hits {
        productId
        url
        name
        imageUrl
        thumbUrl
        description
        brand
        variantId
        availability
        price
        categoryIds
        categories
        categorySplit
        tags1
        tags2
        tags3
        priceCurrencyCode
        attributes
        datePublished
        listPrice
        unitPricingBaseMeasure
        unitPricingUnit
        unitPricingMeasure
        googleCategory
        gtin
        ageGroup
        gender
        condition
        alternateImageUrls
        ratingValue
        reviewCount
        inventoryLevel
        supplierCost
        pid
        isInStock
        isExcluded
        onDiscount
        stats {
          availabilityRatio
        }
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
        ... on SearchStatsFacet {
          id
          field
          type
          name
          min
          max
        }
      }
      collapse
      fuzzy
      categoryId
    }
  }
}
```

GraphQL request variables:

```json
{
  "query": "green",
  "products": {
    "size": 10,
    "from": 10,
    "sort": [
      {
        "field": "price",
        "order": "asc"
      }
    ],
    "filter": [
      {
        "field": "price",
        "range": { "lt": "60", "gt": "50" }
      },
      { 
        "field": "customFields.brandname",
        "value": "Adidas"
      }
    ]
  }
}.
```

