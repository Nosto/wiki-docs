##### Is it possible to omit the Nosto parameter?

The `nosto` parameter allows Nosto to attribute clicks on content and recommendations. In the event, you'd like to omit the parameter, you'll need to manually send the product-view event. You can do so by executing the following snippet.

```js
nostojs(function(api){
   var request = api.createRecommendationRequest()
     .addEvent('vp', "product-id", "placement-id")
     .loadRecommendations();
});
```

How the attribution is tracked will be entirely dependent upon your implementation. 