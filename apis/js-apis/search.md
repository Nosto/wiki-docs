# Search

## Searching

It is possible to call search API directly from javascript library to fetch search results directly on frontend.

For most basic search `fields` parameter should be provided to specify what product fields should be returned. For all parameters, see the [reference](https://search.nosto.com/v1/graphql?ref=InputSearchQuery).

```javascript
nostojs(function(api) {
    api.search({
        query: 'my search',
        products: { fields: ["name"] }
    }).then(function(response) {
        console.log(response);
    });
});
```

Search function also accepts the following options:

| Option     | Default | Description                                         |
| ---------- | ------- | --------------------------------------------------- |
| `redirect` | `false` | Automatically redirect if search returns a redirect |
| `track`    | `null`  | Track search query by provided type                 |

Function automatically loads session parameters required for personalisation & segments in the background.

### Search page

For search page in most cases `facets` parameter should be provided.

Also `redirect` & `track` should be enabled to automatically track searches to Nosto analytics & redirect if API returns a redirect request.

<pre class="language-javascript"><code class="lang-javascript"><strong>nostojs(function(api) {
</strong>    api.search({
        query: 'my search',
        products: {
            facets: ['*'],
            fields: ['name'],
            size: 10
        }
    }, {
        redirect: true,
        track: 'serp'
    }).then(function(response) {
        console.log(response);
    });
});
</code></pre>

### Autocomplete

`track` should be enabled to automatically track searches to Nosto analytics.

```javascript
nostojs(function(api) {
    api.search({
        query: 'my search',
        products: {
            fields: ['name'],
            size: 10
        }
    }, {
        track: 'autocomplete'
    }).then(function(response) {
        console.log(response);
    });
});
```

### Category page

For category page in most cases `facets` parameter should be provided. And either [categoryId](https://search.nosto.com/v1/graphql?ref=InputSearchProducts) or [categoryPath](https://search.nosto.com/v1/graphql?ref=InputSearchProducts) should be also provided.

Furthermore `redirect` & `track` should be enabled to automatically track searches to Nosto analytics & redirect if API returns a redirect request.

```javascript
nostojs(function(api) {
    api.search({
        products: {
            categoryId: '12345',
            fields: ['name'],
            size: 10
        }
    }, {
        track: 'category',
        redirect: true
    }).then(function(response) {
        console.log(response);
    });
});
```

## Session params

For some of the search features to work properly, such as personalised results and segments, the search function needs to be able to access information about the user's session from the front-end.

{% hint style="info" %}
Search function on JS API already includes session state automatically.
{% endhint %}

To get all session data the following snippet can be used:

```javascript
nostojs(function(api) {
    api.getSearchSessionParams().then(function(response) {
        console.log(response);
    });
});
```

The results of this function should be passed to search query [sessionParams](https://search.nosto.com/v1/graphql?ref=InputSearchQuery) parameter. In case search is called from backend, it should pass this data to backend (e.g. using [form data](https://developer.mozilla.org/en-US/docs/Learn/Forms/Sending\_and\_retrieving\_form\_data)).

The function accepts the following options:

| Option                 | Default | Description                    |
| ---------------------- | ------- | ------------------------------ |
| `maxWait`              | `2000`  | Maximum execution time in `MS` |
| `cacheRefreshInterval` | `60000` | Maximum cache time             |
