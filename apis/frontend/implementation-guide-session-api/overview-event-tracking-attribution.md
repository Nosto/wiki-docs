# Overview: Event Tracking and Attribution

Please make sure to [read the introduction on Nosto personalization](../../../implementing-nosto/implement-psn/README.md) (or [read our summary](../../../getting-started/headless-frontend-implementation-methods.md#page-tagging-and-event-tracking-requesting-nosto-content-for-rendering-with-your-templates)) and the previous links in the navigation before diving into this topic.


## Event Tracking via Page Type

Instead of using HTML tagging or tagging providers, you will implement the same pattern for event tracking on all page types. Nosto needs to know what is happening during a user's session, mainly page views (like PDPs or PLPs), "add to cart" events and conversions.

Regardless if an event was influenced by a Nosto personalization module (product recommendation/bundle or on-site content like a personalized banner) or not, **the fundamental call you make is always the same and varies on the page type** (it will make sense in a second).

When a shopper visits the homepage, you will call `viewFrontPage()`, when a search was made for "black shoes" you will call `.viewSearch("black shoes")` and so on. [All page types are listed with examples here](../apis/frontend/implementation-guide-session-api/spa-basics-tracking-events.md).


### Attribution via Reference

You will do the same event tracking as above (mostly on a page view) and add `setRef(...)` to the call for Nosto attribution.

If the product view was caused by a Nosto module (e.g. a recommendation campaign on the homepage), you will add the Nosto campaign slot ID as a reference via `setRef("frontpage-nosto-1")`.
You will receive this reference as `result_id` when you load the campaigns on the current page (homepage in this example). You MUST use the `result_id`, **do not** use the placement or `div_id` ([example response](spa-basics-leveraging-features.md#handling-attribution)).

Another common example is when a shopper is on a PDP (e.g. product ID 42), see a Nosto product recommendation and clicks on a shown product (ID 200). The user opens the PDP and you will track:

- Shopper is now viewing product ID 200
- Shopper saw and clicked on this product ID 200 from the Nosto campaign "productpage-nosto-2".
- (You do not have to track that the Nosto campaign was shown on the PDP for product ID 42.)

```javascript
nostojs(api => {
  api.defaultSession()
    .viewProduct("200")
    .setRef("200", "productpage-nosto-2")
    .setPlacements(api.placements.getPlacements)
    .load()
    .then(response => {
      // ...
    })
});
```

You can store the reference "productpage-nosto-2" of the Nosto campaign in the shopper's browser session storage when the click occurs (still on PDP product ID 42) and retrieve this data when the target PDP (ID 200) is loaded to then pass it to `setRef(...)`.

### Attribution via Click

As alternative to storing a reference and retrieving it for `setRef(...)` you can also let Nosto listen for clicks for direct attribution.

* If you're building the templates in the Nosto backend (HTML response mode), follow the [Session API based usage](../../../implementing-nosto/implement-on-your-website/advanced-implementation/parameterless-attribution.md#session-api-based-usage).
* If you're retrieving only the product data (JSON response mode) and are building the templates in your code base, follow the JSON rendering attribution example below.

Use Case: A shopper is on a PDP (e.g. product ID 42), sees a Nosto product recommendation and clicks on a shown product (ID 200).
You don't explicitly react to the click, you instead set up the click listener with [attributeProductClicksInCampaign](https://nosto.github.io/nosto-js/interfaces/client.API.html#attributeproductclicksincampaign) after rendering the campaign.

```js
nostojs(api => {
  api.defaultSession()
    .viewProduct("42")
    .setPlacements(api.placements.getPlacements)
    .load()
    .then(response => {
      const recommendation = response.recommendations[placementId]
      const container = document.getElementById(placementId)
      if (recommendation && container) {
        // TODO: Define your own method to render products
        renderProductsToContainer(containerElement, recommendation)
        api.attributeProductClicksInCampaign(container, recommendation)
      }
    })
});
```

Here is an example [ev1 response](https://nosto.github.io/nosto-js/interfaces/client.EventResponseMessage.html) (some fields have been omitted):

```json
{
    "recommendations": {
        "productpage-nosto-2": { // requested placement id
            "result_id": "productpage-nosto-2-fallback", // slot id that served the recommendations
            "products": [{
                "url": "https://example.com/products/product200",
                "product_id": "200"
            }, {
                "url": "https://example.com/products/product300",
                "product_id": "300"
            }],
            "result_type": "REAL",
            "title": "Most Popular Right Now",
            "div_id": "productpage-nosto-2" // requested placement id
        }
    }
}
```


## Advanced Cases and Examples

Please [continue reading](spa-basics-leveraging-features.md) to prevent multiple page view events, easy campaign injection and attribute additional events like a "quick view" modal for products or "add to cart" buttons.
