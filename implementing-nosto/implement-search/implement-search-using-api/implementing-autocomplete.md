# Implement Autocomplete

Autocomplete provides keyword suggestions to assist users in completing their queries, supplemented by a selection of the most relevant products with the ability to see all products on the search results page.&#x20;

<figure><img src="../../../.gitbook/assets/image (10).png" alt=""><figcaption><p>Example Autocomplete</p></figcaption></figure>

Check out [autocomplete's look & feel guidelines](https://help.nosto.com/en/articles/7169076-autocomplete-s-look-feel-guidelines).

{% hint style="info" %}
When integrating Autocomplete you have the option to directly access the API, or you can use our existing [Autocomplete JavaScript library](../implement-autocomplete-using-library/) that provides most of the required functionality out of the box.
{% endhint %}

## API Requests <a href="#autocomplete" id="autocomplete"></a>

### Example

#### Query <a href="#autocomplete" id="autocomplete"></a>

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
        _redirect
        _highlight {
          keyword
        }
      }
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
            "_redirect": "https://example.com/green.html",
            "_highlight": {
              "keyword": "<em>green</em>"
            }
          },
          {
            "keyword": "green energy",
            "_redirect": null,
            "_highlight": {
              "keyword": "<em>green</em> energy"
          }
        ]
      }
    }
  }
}
```

### Highlight

API can return highlights indicating which parts of a keyword match the search query. This HTML can be used to render and emphasize the matching sections during display.

### Redirects

Redirects are configured in the search dashboard and can be used to forward users to specific pages depending on what they type into the search field. For example, users searching for "shipping" could be directed to [https://example.com/shipping.html](https://example.com/shipping.html).

{% hint style="info" %}
API only returns redirect url, the actual browser redirect must be implemented by the merchant on keyword selection
{% endhint %}

## Analytics

### Nosto Analytics

To analyze user behavior you need to implement tracking. This can be achieved using our [JavaScript library](../../../apis/js-apis/search.md). You need to implement the following methods with `type = autocomplete`:

* [recordSearch](../../../apis/js-apis/search.md#search) to track users typing in the search field and viewing suggestions
* [recordSearchClick](../../../apis/js-apis/search.md#search-product-keyword-click) to track clicks on autocomplete suggestions

Additionally, see the [tracking instructions for search form submissions](using-the-search-api/#analytics).

### Google Analytics

Each **autocomplete product click should be tracked as a search page virtual view** **to ensure** that the Google Analytics search feature **displays the correct conversion rate**. For example, if you click any product when the typed query is `phone` it should track a virtual page view with the URL `/search?q=phone` (adjust the search path and query parameter to match your search page).

Since a product link click would redirect to a new page, to ensure that Google Analytics has time to send the tracking request, it's recommended to save the search query to local storage and track it on page load.

