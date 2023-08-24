# Implementing Autocomplete

Autocomplete should provide keyword suggestions to assist users in completing their queries, supplemented by a selection of the most relevant products with ability to see all products in search results page.&#x20;

<figure><img src="../../../.gitbook/assets/image (10).png" alt=""><figcaption><p>Example Autocomplete</p></figcaption></figure>

Check out [autocomplete's look & feel guidelines](https://help.nosto.com/en/articles/7169076-autocomplete-s-look-feel-guidelines).

## API Requests <a href="#autocomplete" id="autocomplete"></a>

### Query <a href="#autocomplete" id="autocomplete"></a>

```graphql
query {
  search(
    accountId: "YOUR_ACCOUNT_ID"
    query: "green"
    products: { size: 5 },
    keywords: { size: 5 }
  ) {
    products {
      hits {
        productId
        name
      }
      total
    }
    keywords {
      hits {
        keyword
        _highlight {
          keyword
        }
      }
    }
    query
  }
}
```

### Response

```json
{
  "data": {
    "search": {
      "query": "green",
      "products": {
        "hits": [
          {
            "productId": "1",
            "name": "My product"
          }
        ]
        "total": 1
      },
      "keywords": {
        "hits": [
          {
            "keyword": "green",
            "_highlight": {
              "keyword": "<em>green</em>"
            }
          }
        ]
      }
    }
  }
}
```

### Redirects

Redirects can be used to forward users to special pages depending on what they type into the search field. For example, users searching for shipping could be forwarded to https://example.com/shipping.html.

{% hint style="warning" %}
For API integrations GraphQL can only return the target URL. The actual browser redirect must be implemented by the merchant.
{% endhint %}

#### Query

```graphql
{
  search(
    accountId: "YOUR_ACCOUNT_ID"
    query: "ship"
    keywords: {}
    products: {}
  ) {
    keywords {
      hits {
        keyword
        _redirect
      }
    }
    products {
      hits {
        productId
        name
      }
    }
  }
}
```

[GraphQL playground example](https://search.nosto.com/v1/graphql?query=%7B%0A%20%20search%28%0A%20%20%20%20accountId%3A%20%22YOUR_ACCOUNT_ID%22%0A%20%20%20%20query%3A%20%22ship%22%0A%20%20%20%20keywords%3A%20%7B%7D%0A%20%20%20%20products%3A%20%7B%7D%0A%20%20%29%20%7B%0A%20%20%20%20keywords%20%7B%0A%20%20%20%20%20%20hits%20%7B%0A%20%20%20%20%20%20%20%20keyword%0A%20%20%20%20%20%20%20%20_redirect%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%20%20%20%20products%20%7B%0A%20%20%20%20%20%20hits%20%7B%0A%20%20%20%20%20%20%20%20productId%0A%20%20%20%20%20%20%20%20name%0A%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%20%20%7D%0A%7D)

#### Response

```json
{
  "data": {
    "search": {
      "keywords": {
        "hits": [
          {
            "keyword": "shipping",
            "_redirect": "https://example.com/shipping.html"
          }
        ]
      },
      "products": {}
    }
  }
}
```


## Analytics

### Nosto Analytics

Autocomplete input is considered as search event and it should be tracked using [JS API library helper](../../../apis/js-apis/search.md#search) where `type = autocomplete`.

Product clicks in autocomplete should be tracked with [click helper](../../../apis/js-apis/search.md#search-product-click) where `type = autocomplete`.

When query is submitted, it should be tracked with [submit helper](../../../apis/js-apis/search.md#search-form-submit). Additionally, results of submitted query should be tracked with [search helper](../../../apis/js-apis/search.md#search) where `type = serp`.&#x20;

### Google Analytics

Each **autocomplete product click should be tracked as a search page virtual view** **to ensure** that the Google Analytics search feature **displays the correct conversion rate**. For example, if you click any product when the typed query is `phone` it should track a virtual page view with the URL `/search?q=phone` (adjust the search path and query parameter to match your search page).

Since a product link click would redirect to a new page, to ensure that Google Analytics has time to send the tracking request, it's recommended to save the search query to local storage and track it on page load.

