# Leveraging Features

## Working with recommendations

The `load` method returns a promise which can be consumed to get the raw recommendation data. The raw recommendation data is an object containing the recommendation data.

### Reporting correct page views - load() vs update()

Recommendation results are returned in a promise by calling either `load()` or `update()`. The difference between these two methods is:

* `load()` causes the load count of the most recently tracked page view to be incremented.
* `update()` does not automatically increment the page load count.

In order to achieve an accurate reach statistic for recommendations, `load()` should be called once per page view when fetching recommendations and all proceeding recommendation requests should use `update()`.

### Reporting correct product views - requesting product recommendations outside product detail page

Normally requesting product related recommendations happen in relation to viewing a product detail page, so a typical product related recommendation request looks like this:

```
nostojs(api => {
  api.defaultSession()
    .setResponseMode('HTML')
    .viewProduct('product1') // id of product currently being viewed
    .setPlacements(api.placements.getPlacements())
    .load()
    .then(response => {
      api.placements.injectCampaigns(response.recommendations)
    })
});
```

The above will also add a product view event for the user. In some cases there are needs to request product related recommendations even when the user is not looking at a product. In these cases, to avoid reporting the user event, include a `{ trackEvents: false }` flag to the `load` function, for example:

```
nostojs(api => {
  api.defaultSession()
    .setResponseMode('HTML')
    .viewProduct('product1') // id of product currently being viewed
    .setPlacements(api.placements.getPlacements())
    .load({ trackEvents: false })
    .then(response => {
      api.placements.injectCampaigns(response.recommendations)
    })
});
```

If for implementation specific reasons there are multiple calls for product related recommendations on the product page, only the first request should track the event and subsequent requests should have the `{ trackEvents: false }` flag include.

### Handling attribution

When a recommended product is viewed, the `result_id` from the recommendation result should be used. Below is an example recommendation result (some fields have been omitted).

```json
{
    "recommendations": {
        "nosto-frontpage-1": { // requested placement id
            "result_id": "nosto-frontpage-1-fallback", // slot id that served the recommendations
            "products": [{
                "url": "https://example.com/products/product1",
                "product_id": "product1"
            }, {
                "url": "https://example.com/products/product2",
                "product_id": "product2"
            }],
            "result_type": "REAL",
            "title": "Most Popular Right Now",
            "div_id": "nosto-frontpage-1" // requested placement id
    }
}
```

The snippet of code below attributes `product1` to the result id `nosto-frontpage-1-fallback` from the recommendation result above and fetches recommendations for `nosto-productpage-1`.

```javascript
nostojs(api => {
  api.defaultSession()
   .viewProduct('product1') // id of product currently being viewed
   .setRef('product1', 'nosto-frontpage-1-fallback') // id of product and slot that resulted in the product view
   .setPlacements(['nosto-productpage-1']) // placements to request content for on the product page
   .load()
   .then(data => console.log(data))
 })
```

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

### Including Add to Cart-buttons into Recommendations

Usually adding an Add to cart button directly into the recommendation which lists multiple products is fairly straightforward, the website just needs to have an appropriate javascript function to do the operation. Nosto provides ready-made functions for multiple platforms so check the Nosto documentation specific to your e-commerce platform or if needed look into your e-commerce platform's documentation and support on how to create such a function.

Once the Add to cart button is pressed it should call that function to add the products to the cart, but it should also additionally let Nosto know that the button was pressed so Nosto can track the sales from the recommendation. This can be done with the `reportAddToCart` function as follows:

```javascript
nostojs(api => {
  api.defaultSession()
    .reportAddToCart('product-id', 'element')
    .update()
});
```

⚠️ The api call is only for informing Nosto about the attribution (that the product was added from the recommendation), `setCart` function in the Session API should be used to tell Nosto the user’s cart contents.

## Working with content

The `load` method returns a promise which can be consumed to get the raw placement data. The raw recommendation data is an object containing the placement data.

## Working with popups

Popups are handled automatically and you do not need to control the rendering of the popups. As events are dispatched, popups will show and hide automatically. If you would like to have programmatic control over the popups, see [our guide on the Popup JS API](../../js-apis/popups/).
