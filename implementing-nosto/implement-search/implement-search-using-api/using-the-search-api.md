# Use the Search & Categories API

{% hint style="info" %}
Note that both Search & Categories are built on the same technical foundation and use the same API and product data. For simplicity, we will just refer to _Search API_ in the following documentation.
{% endhint %}

## Playground and API reference <a href="#graphql-playground" id="graphql-playground"></a>

{% hint style="info" %}
Use [Search API Playground](https://search.nosto.com/v1/graphql) to try out search queries and browse API reference.
{% endhint %}

It provides:

1. [Search request schema](https://search.nosto.com/v1/graphql?ref=Query) - you can see field types and inspect what fields are needed for a search request.
2. [Search result schema](https://search.nosto.com/v1/graphql?ref=SearchResult) - you can see return field types with descriptions.
3. Send requests to the search engine and preview the response.

## Authentication

In the majority of cases, **authentication is not a requirement** for using search APIs. However in rare case may need to:

* **Access sensitive data**- all sensitive data is restricted for public access (e.g. sorting by & returning sales).
* **Return all documents** - public access require to specify search query, category ID or category path to avoid returning all documents.

**Note**: Keep your API key secret and do not expose it to the frontend!

| HTTP Header   | Value               |
| ------------- | ------------------- |
| Authorization | `Bearer SEARCH_KEY` |

{% hint style="info" %}
[Authentication Token](https://help.nosto.com/en/articles/613616-settings-authentication-tokens) with `API_SEARCH` Role is available on dashboard settings page
{% endhint %}

## Making requests <a href="#making-requests" id="making-requests"></a>

{% hint style="info" %}
When integrating Search you have the option to directly access the API, or you can use our existing [Search JavaScript library](../search/) that provides most of the required functionality out of the box.
{% endhint %}

### API endpoint

Search use different API endpoint than other Nosto queries: `https://search.nosto.com/v1/graphql`

### Account ID

All requests require an account ID, which can be found in the top-right corner of the Admin dashboard, under the shop name.

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

Upon a successful request, the API will return a 200 status code response, which includes the search data:

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

API returns a list of errors with explanations. These errors should be logged internally and not displayed to the end user. Instead, display a general message, such as `Search is unavailable at the moment, please try again`. This approach ensures that no sensitive information is leaked to the end user.

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

The API may return some errors even when data is returned. This means that some parts of the response may be missing, but you should still display the available data instead of showing an error to the user. These errors should be logged internally for future reference and troubleshooting.

## Session params <a href="#session-params" id="session-params"></a>

For features like personalised results and user segments to function effectively, the search function needs access to the user's session information from the front-end.

It's possible to get search session data using the [JS API](https://docs.nosto.com/techdocs/apis/js-apis/search):

```javascript
nostojs(api => {
    api.getSearchSessionParams().then(response => {
        console.log(response);
    });
});
```

The results of this function should be passed to search query [sessionParams](https://search.nosto.com/v1/graphql?ref=InputSearchQuery) parameter. In case search is called from backend, it should pass this data to backend (e.g. using [form data](https://developer.mozilla.org/en-US/docs/Learn/Forms/Sending\_and\_retrieving\_form\_data)).

It's also possible to save session data to [cookies](https://www.w3schools.com/js/js\_cookies.asp) on page load:

```html
<script>
    nostojs(api => {
        api.getSearchSessionParams().then(response => {
            document.cookie = `nostoSearchSessionParams=${encodeURIComponent(JSON.stringify(response))};`;
        });
    })
</script>
```

## Using variables <a href="#using-variables" id="using-variables"></a>

In the search application, you should use variables instead of hardcoded arguments to pass search data. This means that filters, sort, size, and 'from' options should be passed in the 'products' variable. For a full list of available options, please see the [reference](https://search.nosto.com/v1/graphql?ref=InputSearchProducts).

### Query

```graphql
query (
  $query: String,
  $products: InputSearchProducts,
  $sessionParams: InputSearchQuery
) {
  search(
    accountId: "YOUR_ACCOUNT_ID"
    query: $query
    products: $products,
    sessionParams: $sessionParams
  ) {
    query
    products {
      hits {
        productId
        url
        name
        imageUrl
        thumbUrl
        brand
        availability
        price
        listPrice
        customFields {
          key
          value
        }
        skus {
          name
          customFields {
            key
            value
          }
        }
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
      fuzzy
    }
  }
}
```

{% hint style="info" %}
To optimize search speed and reduce network load, select only the necessary data when performing a search query.
{% endhint %}

### Variables

Variables should encompass all dynamic query data because it is the most efficient method to pass data, and data is automatically escaped to prevent injection attacks. Avoid generating dynamic queries, as they can lead to security issues if user input is not properly escaped.

```json
{
  "query": "red",
  "products": {
    "size": 5
  },
  "sessionParams": {}
}
```

## Analytics

To analyze user behavior you need to implement tracking. This can be achieved using our [JavaScript library](../search/). You need to implement the following methods:

* [recordSearchSubmit](../search/#search-form-submit) to track search form submissions
