# Search

## Searching

It is possible to call the Search API directly from the JS library to fetch search results directly on frontend.

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

The api.search function also accepts the following options:

<table><thead><tr><th width="148">Option</th><th width="94">Default</th><th>Description</th></tr></thead><tbody><tr><td><code>redirect</code></td><td><code>false</code></td><td>Automatically redirect if search returns a redirect</td></tr><tr><td><code>track</code></td><td><code>null</code></td><td>Track search query by provided type</td></tr></tbody></table>

The function automatically loads session parameters required for personalization & segments in the background.

### Search page

For a search page in most cases the `facets` parameter should be provided.

Also `redirect` & `track` should be enabled to automatically track searches to Nosto analytics & redirect if API returns a redirect request.

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
        track: 'serp'
    }).then(response => {
        console.log(response);
    });
});
```

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

For category pages in most cases the `facets` parameter should be provided. And either [categoryId](https://search.nosto.com/v1/graphql?ref=InputSearchProducts) or [categoryPath](https://search.nosto.com/v1/graphql?ref=InputSearchProducts) should be also provided.

Furthermore `redirect` & `track` should be enabled to automatically track searches to Nosto analytics & redirect if API returns a redirect request.

```javascript
nostojs(api => {
    api.search({
        products: {
            categoryId: '12345',
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

For some of the search features to work properly, such as personalised results and segments, the search function needs to be able to access information about the user's session from the front-end.

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

The results of this function should be passed to search query [sessionParams](https://search.nosto.com/v1/graphql?ref=InputSearchQuery) parameter. In case search is called from backend, it should pass this data to backend (e.g. using [form data](https://developer.mozilla.org/en-US/docs/Learn/Forms/Sending\_and\_retrieving\_form\_data)).

The function accepts the following options:

<table><thead><tr><th width="252">Option</th><th width="94">Default</th><th>Description</th></tr></thead><tbody><tr><td><code>maxWait</code></td><td><code>2000</code></td><td>Maximum execution time in <code>MS</code></td></tr><tr><td><code>cacheRefreshInterval</code></td><td><code>60000</code></td><td>Maximum cache time</td></tr></tbody></table>

## Analytics

Tracking search events to analytics can be divided into three parts: `search`, `search submit`, `search product/keyword click`. These are user behaviours that should be tracked:

* search submit (`type = serp`)
* faceting, paginating, sorting (`type = serp/category`)
* autocomplete input (`type = autocomplete)`
* search results product click `(type = serp/autocomplete/category)`
* autocomplete keyword click (`type = autocomplete)`
* category merchandising results `(type = category)`

JS API library provides tracking helpers for all of these cases.

### Search

User actions that lead to search results should be tracked with `api.recordSearch()` after search request is done:

* search submit (`type = serp`) - user submits search query in autocomplete component and is landed to SERP
* faceting, paginating, sorting (`type = serp/category`) - user adjusts current search results by filtering (e.g. brand), selecting search page, sorting results
* autocomplete input (`type = autocomplete)` - user sees partial results while typing in search input
* category merchandising results `(type = category)` - user sees specific category results when category is selected (category merchandising must be implemented)

{% hint style="danger" %}
You don't need to execute `api.recordSearch()`if you call `api.search(query, { track: 'serp'|'autocomplete'|'category'})` function from JS API, because`api.search()`already calls `api.recordSearch()` when `track` option is provided.
{% endhint %}

```javascript
nostojs(api => {
    api.recordSearch(
        type,
        query,
        response
      )
})
```

<table><thead><tr><th>Parameter</th><th>Description</th><th data-hidden></th></tr></thead><tbody><tr><td>type</td><td>Search type: <code>serp</code>, <code>autocomplete</code>, <code>category</code></td><td></td></tr><tr><td>query</td><td>Partial search API query containing: <code>query</code>, <code>products.sort</code>, <code>products.filter</code></td><td></td></tr><tr><td>response</td><td>Partial search API response containing: <code>products.hits.productId[]</code>, <code>products.fuzzy</code>, <code>products.total</code>, <code>products.size</code>, <code>products.from</code></td><td></td></tr></tbody></table>

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
    }
)
```

### Search form submit

Search queries are categorised into two groups: organic and non-organic searches.\
\
In order to mark a search query as an organic search you need to call `api.recordSearchSubmit()`. You should call it on search input submit only, before search request is sent.

```javascript
nostojs(api => {
    api.recordSearchSubmit(query)
})
```

{% hint style="info" %}
Organic search - is a search query submitted through search input and which lead to SERP (search engine results page). Following faceting, paginating, sorting queries on organic query is also counted as organic.&#x20;
{% endhint %}

### Search product/keyword click

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



{% hint style="warning" %}
Product and keyword clicks must be tracked separately by binding to two separate click events.
{% endhint %}

Keyword clicks should be tracked in autocomplete component with `api.recordSearchClick()` by providing `type = autocomplete` and keyword hit object:

```javascript
nostojs(api => {
    api.recordSearchClick(
        'autocomplete',
        { keyword: 'green' }
    )
})
```