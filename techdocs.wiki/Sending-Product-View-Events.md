When you need to explicity send an event to Nosto to tell it that the user has viewed a certain product, you must do so manually. A need for something like this arises when your website has a quick-view function on the category or search pages that enables the customer to preview the product in an overlay without actually navigating to the product page.


```js
nostojs(function(api) {
  api.createRecommendationRequest()
    .addEvent('vp', "product-id-101")
    .loadRecommendations();
});
```

**Note:** The example above creates a new request, adds view product event for productId3 and sends the event to Nosto. Since the request did not specify any recommendation slots, this request only submits view event to Nosto. Below is a bit more complex example which makes use of other aspects of the Nosto request to achieve different behaviour.

In the event that the product was viewed due to a click on a Nosto placement, you must send an additional parameter that denotes the identifier of the placement that was clicked.

```js
nostojs(function(api){
   var request = api.createRecommendationRequest()
     .addEvent('vp', "product-id", "placement-id")
     .loadRecommendations();
});
```

**Note:** Failure to do will result in incorrect attribution statistics.