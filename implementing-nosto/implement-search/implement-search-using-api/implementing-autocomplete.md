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



## Analytics

### Nosto Analytics

Autocomplete input is considered as search event and it should be tracked using [JS API library helper](../../../apis/js-apis/search.md#search) where `type = autocomplete`.

Product clicks in autocomplete should be tracked with [click helper](../../../apis/js-apis/search.md#search-product-click) where `type = autocomplete`.

When query is submitted, it should be tracked with [submit helper](../../../apis/js-apis/search.md#search-form-submit). Additionally, results of submitted query should be tracked with [search helper](../../../apis/js-apis/search.md#search) where `type = serp`.&#x20;

### Google Analytics

Each **autocomplete product click should be tracked as a search page virtual view** **to ensure** that the Google Analytics search feature **displays the correct conversion rate**. For example, if you click any product when the typed query is `phone` it should track a virtual page view with the URL `/search?q=phone` (adjust the search path and query parameter to match your search page).

Since a product link click would redirect to a new page, to ensure that Google Analytics has time to send the tracking request, it's recommended to save the search query to local storage and track it on page load.

