# Submit search

When submitting Search results through Autocomplete, `submit` callback is called on these events:

* **`Enter` key press.**
* **`Submit button` click.**
* **`Keyword` click.**

By default `submit` checks if query/keyword length satisfies `minQueryLength`, sends `Search Submit event` to Nosto Analytics, and sends Search request to the Nosto Search API.

In the usual scenario, you want to render Search Results on submit, so you should override `submit` function:

```js
submit: async (query, config) => {
    if (
        query.length >= config.minQueryLength
    ) {
        const response = await search(
            {
                query,
            },
            {
                redirect: true,
                track: config.nostoAnalytics ? "serp" : undefined,
            }
        )
        // Do something with response. For example, update Search Engine Results Page products state.
    }
},
```

To disable `submit` pass `undefined` value.\
\


#### 📊 Nosto Analytics (enabled by default)

Setting `nostoAnalytics: true` will enable Nosto Analytics tracking. Tracking results can be seen in the Nosto Dashboard under Search & Categories -> Analytics page.

❗Note: you should additionally add click events on your search results page according to [Nosto Tech Docs](https://docs.nosto.com/techdocs/apis/frontend/js-apis/search#search-product-keyword-click) with `type: serp || category` according to the results page type.❗\
\


#### 📈 Google Analytics (enabled by default)

By default we send `pageview` events to existing GA tag, found in shop site. To send `pageview` events with correct search information, a minimal configuration is needed in `googleAnalytics` property.

* **`serpPath`** - Search query url parameter name
* **`queryParamName`** - Search query url parameter name
* **`enabled`** - Enable Google Analytics

For example, if search results URL is https://examplenostoshop.com/search-results?query=shoes, then configuration should be:

```js
googleAnalytics: {
    serpPath: "search-results",
    queryParamName: "query",
    enabled: true
}
```

To disable Google Analytics, set `googleAnalytics: false`.
