# Leveraging Features

## Working with recommendations

The `load` method returns a promise which can be consumed to get the raw recommendation data. The raw recommendation data is an object containing the recommendation data.

### Handling attribution

When a recommendation placement is clicked, you must redirect to the next page using the `nosto` query parameter and the value must be the `result_id` from the response.

### Letting Nosto render the HTML

If you wish Nosto to render the recommendations and returns HTML for the recommendations you can set the response mode to HTML by adding `.setResponseMode('HTML')` into your call. A full example for 404 page would look like this

```javascript
nostojs(api => {
  api.defaultSession()
    .setResponseMode('HTML')
    .viewNotFound()
    .setPlacements(['notfound-nosto-1', 'bestseller-recs'])
    .load()
    .then(data => {
      // Here you would inject the recommendations to DOM
      console.log(data.recommendations);
    })
});
```

Please note that Nosto will not inject the returned HTML into the DOM even if the response mode is set to HTML. The logic for injecting the recommendations must be implemented in the application side.

## Working with content

The `load` method returns a promise which can be consumed to get the raw placement data. The raw recommendation data is an object containing the placement data.

### Handling attribution

When a content placement is clicked, you must redirect to the next page using the `nosto` query parameter and the value must be the `result_id` from the response.

**Example** If a product route is `/product/nike-sneakers-1` with `result_id` having value _nosto-frontpage-1_ the route would be `/product/nike-sneakers-1/?nosto=nosto-frontpage-1`.

**Important** In order for the attribution to work you must perform an [action](session-api-terminology.md#action) after the product route change. Most likely you would be fetching [product related recommendations](spa-basics-tracking-events.md#upon-viewing-a-product).

## Working with popups

Popups are handled automatically and you do not need to control the rendering of the popups. As events are dispatched, popups will show and hide automatically. If you would like to have programmatic control over the popups, see [our guide on the Popup JS API](../../apis/js-apis/popups/).

