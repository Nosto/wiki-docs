# Implement Autocomplete

Autocomplete provides keyword suggestions to assist users in completing their queries, supplemented by a selection of the most relevant products with the ability to see all products on the search results page. The feature also supports category and popular search suggestions. Please contact Nosto Support to have them enabled for your account.

<figure><img src="../../../.gitbook/assets/image (9) (1).png" alt=""><figcaption><p>Example Autocomplete</p></figcaption></figure>

Check out [autocomplete's look & feel guidelines](https://help.nosto.com/en/articles/7169076-autocomplete-s-look-feel-guidelines).

{% hint style="info" %}
When integrating Autocomplete you have the option to directly access the API, or you can use our existing [Autocomplete JavaScript library](../search/implement-autocomplete-using-library/) that provides most of the required functionality out of the box.
{% endhint %}

## Requirements

Some autocomplete features are available conditionally.

* **Keyword suggestions** require searchable fields to be marked for autocomplete.
* **Popular searches** require a function Nosto tracking integration for searches.
* **Category suggestions** depend on available categories (including URLs)
  being [sent to Nosto](../../../apis/graphql-an-introduction/graphql-using-mutations/updating-categories).
  * The Shopify integration sends categories to Nosto out-of-the-box, with no extra work required.
  * The Shopware 6 plugin automatically sends categories to Nosto without requiring extra work from version 5.1.4.

## API Requests <a href="#autocomplete" id="autocomplete"></a>

### Example

#### Query <a href="#autocomplete" id="autocomplete"></a>

```graphql
query {
  search(
    accountId: "YOUR_ACCOUNT_ID"
    query: "green"
    products: { size: 5 },
    keywords: { size: 5 },
    categories: { size: 5 },
    popularSearches: { 
      size: 5,
      emptyQueryMatchesAll: true
    }
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
    categories {
      hits {
        name
        fullName
        externalId
        parentExternalId
        url
        urlPath
      }
      total
    }
    popularSearches {
      hits {
        query
        total
      }
      total
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
        ],
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
          }
        ]
      },
      "categories": {
        "hits": [
          {
            "name": "Home and Garden > Plants > Green Plants",
            "fullName": "Home and Garden > Plants > Green Plants",
            "externalId": "1234",
            "parentExternalId": "5678",
            "url": "https://www.example.com/category/home-and-garden",
            "urlPath": "home-and-garden"
          },
          {
            "name": "Fashion > Jackets > Green Jackets",
            "fullName": "Fashion > Jackets > Green Jackets",
            "externalId": "4321",
            "parentExternalId": "8765",
            "url": "https://www.example.com/category/fashion",
            "urlPath": "fashion"
          }
        ],
        "total": 86
      },
      "popularSearches": {
        "hits": [
          {
            "query": "green pants",
            "total": 3024
          },
          {
            "query": "green shirt",
            "total": 480
          }
        ],
        "total": 2
      }
    }
  }
}
```
#### Empty query

To retrieve results for an empty query, you must explicitly set `emptyQueryMatchesAll: true` in your request.
By default, `emptyQueryMatchesAll` is `false` and the API does not return any results when the query is empty.
Setting it to `true` enables the API to return default suggestions.
This behavior applies to all suggestion types — keywords, categories, and popular searches.

For more details please check the [Search request schema](https://search.nosto.com/v1/graphql?ref=Query)

#### Example query for default popular search suggestions
The query below returns the most popular search terms for the specified account.

```graphql
query {
    search(
        accountId: "YOUR_ACCOUNT_ID"
        query: ""
        popularSearches: {size: 5, emptyQueryMatchesAll: true}
    ) {
        popularSearches {
            hits {
                query
                total
            }
            total
        }
        query
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

{% include "../../../.gitbook/includes/analytics-hint.md" %}

### Google Analytics

Each **autocomplete product click should be tracked as a search page virtual view** **to ensure** that the Google Analytics search feature **displays the correct conversion rate**. For example, if you click any product when the typed query is `phone` it should track a virtual page view with the URL `/search?q=phone` (adjust the search path and query parameter to match your search page).

Since a product link click would redirect to a new page, to ensure that Google Analytics has time to send the tracking request, it's recommended to save the search query to local storage and track it on page load.
