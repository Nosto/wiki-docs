# Using the Search API

## Playground and API reference <a href="#graphql-playground" id="graphql-playground"></a>

Use [Search API Playground](https://search.nosto.com/v1/graphql) to try out search queries and browse API reference.

It provides:

1. [Search request schema](https://search.nosto.com/v1/graphql?ref=Query) - you can see field types and inspect what fields are needed for a search request.
2. [Search result schema](https://search.nosto.com/v1/graphql?ref=SearchResult) - you can see return field types with descriptions.
3. Send requests to the search engine and preview the response.

## Authentication

In the majority of cases, **authentication is not a requirement** for using search APIs. However in rare case may need to:

* **Access sensitive data**- all sensitive data is restricted for public access (e.g. sorting by & returning sales).
* **Return all documents** - public access require to specify search query or category ID to avoid returning all documents.

**Note**: Keep your API key secret and do not expose it to the frontend!

| HTTP Header   | Value               |
| ------------- | ------------------- |
| Authorization | `Bearer SEARCH_KEY` |

{% hint style="info" %}
[Authentication Token](https://help.nosto.com/en/articles/613616-settings-authentication-tokens) with `API_SEARCH` Role is available on dashboard settings page
{% endhint %}

## Making search API requests <a href="#making-search-api-requests" id="making-search-api-requests"></a>

### API endpoint

Search use different API endpoint than other Nosto queries: `https://search.nosto.com/v1/graphql`

### Account ID

All request requires account ID that you can find on top-right corner in Admin dashboard under shop name.

<figure><img src="../../../.gitbook/assets/Screenshot 2023-03-31 at 16.04.11.png" alt=""><figcaption><p><a href="https://my.nosto.com/">https://my.nosto.com</a></p></figcaption></figure>

### Request example

{% tabs %}
{% tab title="Curl" %}
```bash
curl -X POST 'https://search.nosto.com/v1/graphql' \
-H 'Content-Type: application/json' \
-d @- << EOF
{
  "query": "query (\$query: String) { search (accountId: \"YOUR_ACCOUNT_ID\", query: \$query) { products { hits { name } total } } }",
  "variables": {
    "query": "green"
  }
}
EOF
```

Replace `YOUR_ACCOUNT_ID` with your account id retrieved from the Nosto dashboard.
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
      search (accountId: "YOUR_ACCOUNT_ID", query: $query) {
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

Replace `YOUR_ACCOUNT_ID` with your account id retrieved from the Nosto dashboard.

JS API includes full-featured [search function](https://docs.nosto.com/techdocs/apis/js-apis/search) with tracking support.
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
            search (accountId: "YOUR_ACCOUNT_ID", query: \$query) {
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

Replace `YOUR_ACCOUNT_ID` with your account id retrieved from the Nosto dashboard.
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
            search (accountId: "YOUR_ACCOUNT_ID", query: $query) {
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

Replace `YOUR_ACCOUNT_ID` with your account id retrieved from the Nosto dashboard.
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
            "name": "My Product",
          }
        ],
        "total": 1
      }
    }
  }
}
```

## Error handling

### Error response

API returns list of errors with explanations. These errors should be logged internally and not displayed to end user.

```json
{
  "data": {
    ...
  },
  "errors": [
    {
      "message": "Error explanation"
    }
  ]
}
```

### Partial response

API may return some errors even when data is returned. That means some part of response may be missing, but you should still display it instead of showing error to our user. These errors should be logged to internal logging.&#x20;

## Production example <a href="#autocomplete" id="autocomplete"></a>

In the search application you should use variables instead of hardcoded arguments to pass search data, meaning `filters`, `sort`, `size`, `from` options should be passed in `products` variable (see [InputSearchProducts reference](https://search.nosto.com/v1/graphql?ref=InputSearchProducts) for all available options)

### Request query

```graphql
query (
  $accountId: String,
  $query: String,
  $segments: [String!],
  $products: InputSearchProducts
) {
  search(
    accountId: "ACCOUNT ID"
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

### Request variables

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

## Analytics

To review search queries that have been made on your site, and to see how those queries are performing in terms of click-through rate, conversion rate, you want to implement analytics tracking along with Search implementation.

[Analytics page](../../../apis/js-apis/search.md#analytics) describes how can you implement tracking utilising JS API library helpers.

Tracking results can be reviewed in the Nosto Dashboard.
