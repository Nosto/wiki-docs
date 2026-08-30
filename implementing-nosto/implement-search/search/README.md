# Using the JavaScript Library

## Search <a href="#search" id="search"></a>

For client-side/frontend integrations, Nosto's JavaScript library can be used to simplify the integration. It provides a programming interface to access the Search & Categories API.

For the most basic search the `fields` parameter should be provided to specify what product/keyword fields should be returned. Both `products` and `keywords` can be used separately and together, depending on the use case. For all parameters, see the [reference](https://search.nosto.com/v1/graphql?ref=InputSearchQuery).

```javascript
nostojs(api => {
    api.search({
        query: 'my search',
        products: { fields: ["name"] },
        keywords: { fields: ["keyword"] }
    }).then(response => {
        console.log(response);
    });
});
```

> **Note:** The first parameter of `api.search` generally corresponds to the GraphQL schema accepted by the search backend. The second parameter includes more frontend-specific logic like tracking or following redirects.

The second parameter of the `api.search` function also accepts the following optional fields:​

<table><thead><tr><th width="128.140625">Option</th><th width="174.92578125">Accepted values</th><th width="154.78515625">Default</th><th>Description</th></tr></thead><tbody><tr><td><code>redirect</code></td><td><p><code>true</code></p><p><code>false</code></p></td><td><code>false</code><br>(Ignore redirects)</td><td>Automatically follow page redirects if instructed by the backend response</td></tr><tr><td><code>track</code></td><td><p><code>"autocomplete"</code></p><p><code>"category"</code></p><p><code>"serp"</code></p><p><code>undefined</code></p></td><td><code>undefined</code><br>(No tracking)</td><td>Track the search query as coming from the provided page type</td></tr><tr><td><code>isKeyword</code></td><td><code>true</code><br><code>false</code></td><td><code>false</code><br>(Not a keyword)</td><td>Indicates that the search is triggered by a keyword click in autocomplete</td></tr></tbody></table>

The function automatically loads session parameters required for personalization & segments in the background.

In order to request custom fields, add the entries `"customFields.key"` and `"customFields.value"` to the requested product fields. This changes the example above like this

```javascript
// ...
        products: { fields: ["name", "customFields.key", "customFields.value"] },
// ...
```

### Search page <a href="#search-page" id="search-page"></a>

For a search page, the `facets` parameter should generally be provided. In many cases, `*` is sufficient as a wildcard to include all facets.

In order to automatically track search request to Nosto analytics, `track` parameter should be provided with the correct page type.

`isKeyword` should be set to `true` if search is triggered by selecting a keyword suggested in the autocomplete.&#x20;

```javascript
nostojs(api => {
    api.search({
        query: 'my search',
        products: {
            facets: ['*'],
            fields: ['name'],
            size: 10
        }
    }, {
        redirect: true,
        track: 'serp',
        isKeyword: true
    }).then(response => {
        console.log(response);
    });
});
```

#### Redirects

The `redirect` parameter, if set to true, causes the JS library to automatically follow any redirects returned by the backend. These are triggered by the merchant's configuration, redirecting certain user queries to specific pages. For example, query "summer" may get redirected to the "summer sale" collection.

The default redirection mechanism simply updates the window location:

```
window.location.href = response.redirect
```

> **Note:** Do mix up `response.redirect` with the query's `redirect` property. The former contains the redirected target URL, while the latter is a boolean parameter on the query.

You may want to set `query.redirect` to `false` when this redirection mechanism is insufficient and you want to use another method, such as your framework's routing library. You may obtain the redirect target from `response.redirect` field and act accordingly.

### Autocomplete

`track` should be enabled to automatically track searches to Nosto analytics.

```javascript
nostojs(api => {
    api.search({
        query: 'my search',
        products: {
            fields: ['name'],
            size: 10
        },
        keywords: {
            fields: ['keyword'],
            size: 5
        }
    }, {
        track: 'autocomplete'
    }).then(response => {
        console.log(response);
    });
});
```

### Category page

For category pages in most cases the `facets` parameter should be provided. Additionally [categoryId](https://search.nosto.com/v1/graphql?ref=InputSearchProducts) (for Shopify) and [categoryPath](https://search.nosto.com/v1/graphql?ref=InputSearchProducts) should be also provided.

Furthermore `redirect` & `track` should be enabled to automatically track searches to Nosto analytics & redirect if API returns a redirect request.

```javascript
nostojs(api => {
    api.search({
        products: {
            categoryId: '12345',
            categoryPath: 'Pants',
            fields: ['name'],
            size: 10
        }
    }, {
        track: 'category',
        redirect: true
    }).then(response => {
        console.log(response);
    });
});
```

## Session parameters

For some of the search features to work properly, such as personalized results and segments, the search function needs to be able to access information about the user's session from the front-end.

{% hint style="info" %}
The search function of the JS API already includes session state automatically.
{% endhint %}

To get all session data the following snippet can be used:

```javascript
nostojs(api => {
    api.getSearchSessionParams(options).then(response => {
        console.log(response);
    });
});
```

The results of this function should be passed to search query [sessionParams](https://search.nosto.com/v1/graphql?ref=InputSearchQuery) parameter. In case search is called from backend, it should pass this data to backend (e.g. using [form data](https://developer.mozilla.org/en-US/docs/Learn/Forms/Sending_and_retrieving_form_data)).

The function accepts the following options:

<table><thead><tr><th width="252">Option</th><th width="94">Default</th><th>Description</th></tr></thead><tbody><tr><td><code>maxWait</code></td><td><code>2000</code></td><td>Maximum execution time in <code>MS</code></td></tr><tr><td><code>cacheRefreshInterval</code></td><td><code>60000</code></td><td>Maximum cache time</td></tr></tbody></table>

## Analytics

Tracking search events to analytics can be divided into three parts: `search`, `search submit`, `search product click`. These are user behaviors that should be tracked:

* search submit (`type = serp`)
* faceting, paginating, sorting (`type = serp`) or (`type = category`)
* autocomplete input (`type = autocomplete`)
* search results product click (`type = serp`), (`type = autocomplete`) or (`type = category`)
* autocomplete keyword click (`type = autocomplete`)
* category merchandising results (`type = category`)

JS API library provides tracking helpers for all of these cases.

### Error Handling

On errors from `api.search` calls the error object contains a status field that has the HTTP response status which can be used to determine the cause of the error in addition to the error message. The ranges of the status codes used for errors are 400-499 and 500-599. For details on these error codes, go to [HTTP response status codes](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status#client_error_responses)

### Search

User actions that lead to search results should be tracked with `api.recordSearch()` after search request is done:

* search submit (`type = serp`) - user submits search query in autocomplete component and is landed to SERP
* faceting, paginating, sorting (`type = serp`) or (`type = category`) - user adjusts current search results by filtering (e.g. brand), selecting search page, sorting results
* autocomplete input (`type = autocomplete`) - user sees partial results while typing in search input
* category merchandising results (`type = category`) - user sees specific category results when category is selected (category merchandising must be implemented)

{% hint style="danger" %}
You don't need to execute `api.recordSearch()`if you call `api.search(query, { track: 'serp'|'autocomplete'|'category'})` function from JS API, because`api.search()`already calls `api.recordSearch()` when `track` option is provided.
{% endhint %}

```javascript
nostojs(async (api) => {

    const searchRequest = {
      query: "shoes",
      products: {
        sort: [{ field: "price", order: "asc" }],
        filter: [{ "field": "brand", "value": "Nike" }]
      }
    }
    const response = await sendGraphQlRequest(searchRequest)
    const searchResult = response.data.search

    api.recordSearch(
        type,
        searchRequest,
        searchResult,
        options
      )
})
```

<table><thead><tr><th>Parameter</th><th>Description</th><th data-hidden></th></tr></thead><tbody><tr><td>type</td><td>Search type: <code>serp</code>, <code>autocomplete</code>, <code>category</code></td><td></td></tr><tr><td>request</td><td><a href="https://search.nosto.com/v1/graphql?ref=Query">Full search API request</a></td><td></td></tr><tr><td>result</td><td><a href="https://search.nosto.com/v1/graphql?ref=SearchResult">Full search API result</a></td><td></td></tr><tr><td>options (optional)</td><td><p>Record search options. Currently is accepts:<br></p><p><code>isKeyword: boolean</code> - should be set when keyword in autocomplete is clicked (search is submitted via keyword)</p></td><td></td></tr></tbody></table>

Example:

```javascript
api.recordSearch(
    "serp",
    {
        query: "shoes",
        products: {
            sort: [{ field: "price", order: "asc" }],
            filter: [{ "field": "brand", "value": "Nike" }]
        }
    },
    {
        products: {
            hits: [{ productId: "123" }, { productId: "124" }],
            fuzzy: true,
            total: 2,
            size: 2,
            from: 0
        }
    },
    {
        isKeyword: false
    }
)
```

#### Keyword click tracking

Example:

```javascript
api.recordSearch(
    'autocomplete', 
    searchRequest,
    searchResult,
    { 
        isKeyword: true
    } 
)
```

#### Category results tracking

Example:

```javascript
api.recordSearch(
    'category',
    {
        products: {
            categoryId: "123456",
            categoryPath: "Pants",
            sort: [{ field: "price", order: "asc" }]
            size: 24,
            from: 0,
        }
    },
    {
        products: {
            categoryId: "123456",
            categoryPath: "Pants",
            hits: [{ productId: "123" }, { productId: "124" }],
            fuzzy: true,
            total: 18,
            size: 24,
            from: 0
        }
    },
    {
        isKeyword: false
    }
)
```

The tracking metadata is primarily taken from the third parameter. The [full request](https://search.nosto.com/v1/graphql?ref=Query) and [full result](https://search.nosto.com/v1/graphql?ref=SearchResult) objects must be provided in the `api.recordSearch` call instead of partials.

### Search form submit

Search queries are categorised into two groups: organic and non-organic searches.\
\
In order to mark a search query as an organic search you need to call `api.recordSearchSubmit(query: string)`. You should call it on search input submit only, before search request is sent.

```javascript
nostojs(api => {
    const query = 'shoes'
    api.recordSearchSubmit(query)
})
```

{% hint style="info" %}
Organic search - is a search query submitted through search input and which lead to SERP (search engine results page). Following faceting, paginating, sorting queries on organic query is also counted as organic.
{% endhint %}

### Search product click

Product clicks should be tracked in autocomplete component, SERP, category page with `api.recordSearchClick()` by providing component (type), where click occurred, and clicked product data:

```javascript
nostojs(api => {
    api.recordSearchClick(type, hit)
})
```

<table><thead><tr><th>Parameter</th><th></th><th data-hidden></th></tr></thead><tbody><tr><td>type</td><td>Search type: <code>serp</code>, <code>autocomplete</code>, <code>category</code></td><td></td></tr><tr><td>hit</td><td>Object containing <code>productId</code> and <code>url</code>, collected from clicked product</td><td></td></tr></tbody></table>

Example:

```javascript
api.recordSearchClick(
    "autocomplete", 
    { productId: "123", url: "https://myshop.com/product123" }
)
```

### Search add to cart additions

When shopping cart additions happen directly as part of the **search**, **category** or **autocomplete** results **without a navigation to the product page**, the `api.recordSearchAddToCart()` method should be called with the component (type), where addition occurred and the product data:

```javascript
nostojs(api => {
    api.recordSearchAddToCart(type, hit)
})
```

<table><thead><tr><th>Parameter</th><th></th><th data-hidden></th></tr></thead><tbody><tr><td>type</td><td>Search type: <code>serp</code>, <code>autocomplete</code>, <code>category</code></td><td></td></tr><tr><td>hit</td><td>Object containing <code>productId</code> and <code>url</code>, collected from product</td><td></td></tr></tbody></table>

Example:

```javascript
api.recordSearchAddToCart(
    "autocomplete", 
    { productId: "123", url: "https://myshop.com/product123" }
)
```

***

### :warning: Event Tracking Requirements :warning:

When tracking events, adherence to the following criteria is essential for capturing detailed and valid data:

* **`query` parameter**:
  * The `query` string is an essential component for event tracking.
  * If present, include `products.sort` to track sorting behavior.
  * If applicable, incorporate `products.filter`.
* **`searchResults` parameter**:
  * `products.hits` array containing objects with a `productId` is mandatory.
  * `products.total` number to identify if the search has results.
  * For accurate pagination tracking, `products.from` and `product.size` must be included.
  * For identifying if the request was autocorrected include `products.fuzzy`.
  * For category requests, either `products.categoryId` or `products.categoryPath`is mandatory.

> :bulb: **Tip:** In case of API integration, use this example GraphQL partial query to integrate with the API and retrieve the necessary response data for precise event tracking.

```graphql
query {
  search(
    accountId: "YOUR_ACCOUNT_ID"
    query: "green"
    products: { size: 10, from: 10 }
  ) {
    products {
      hits {
        productId
      }
      total
      size
      from
      fuzzy
      categoryId
      categoryPath
    }
  }
}
```

#### :triangular\_flag\_on\_post: Search Form Submit

Bear in mind that search queries are split between **organic** and **non-organic searches**. To classify a search as organic, it is crucial to invoke `api.recordSearchSubmit()` upon the search input submission, _before_ the actual search request is dispatched. This step is pivotal in ensuring the seamless tracking of organic searches through to the SERP.

#### :mag: Accurate Click Tracking

Tracking product clicks is fundamental for understanding user interaction. Use `api.recordSearchClick()` to monitor this actions correctly, specifying the `type` and relevant hit data.

In case of an SPA based integration the `api.recordSearchClick` calls should be complemented with Session API or `api.createRecommendationRequest()` usage to couple the search analytics events to generic Nosto events for accurate attribution.

## Fallback mechanism

For JS API integrations, there is no built-in fallback functionality. Merchants need to build this themselves to ensure a robust search experience.

### When to implement fallbacks

You should implement fallback mechanisms in the following scenarios:

* **Search query errors**: When the search API returns an error response
* **Timeout scenarios**: JS API based Search requests timeout after 5 seconds, so implement fallbacks for cases when requests take longer than expected
* **Network connectivity issues**: When there are network problems preventing API calls

### Implementation considerations

* **Error handling**: Always wrap Nosto search calls in try-catch blocks
* **User experience**: Ensure seamless transition to fallback without visible errors
* **Analytics**: Track fallback usage to monitor search performance

***
