# Sending Product-View Events

When you need to explicitly send an event to Nosto to tell it that the user has viewed a certain product, you must do so manually. A need for something like this arises when your website has a quick-view function on the category or search pages that enables the customer to preview the product in an overlay without actually navigating to the product page.

```javascript
nostojs(function(api) {
  api.createRecommendationRequest()
    .addEvent('vp', "product-id-101")
    .load();
});
```

**Note:** The example above creates a new request, adds view product event for productId3 and sends the event to Nosto. Since the request did not specify any recommendation slots, this request only submits view event to Nosto. 

You can either create an empty request as in the above example and add only the wanted parts to it, or you can create a request based on the tagging on the page by using the `includeTagging` field and then modify it. Here is an example that creates a request using the tagging on the page and then overrides the current tag to be custom colour to update the recommendation with id `productpage-nosto-2` to show only products with the same colour:

```javascript
nostojs(api => {
  api.createRecommendationRequest({includeTagging:true})
    .setCurrentTags(["color:red"])
    .setElements(["productpage-nosto-2"])
    .load()
    .then(response => {
      console.log(response);
    });
});
```

In addition to the `currentTags` information, we will be able to send-in `current custom field` data in recommendation request in order to create a more robust and dynamic recommendations using custom fields. `customFields` is of object type. For e.g. { key1: "value1", key2: "value2" }

```javascript
nostojs(api => {
  api.createRecommendationRequest({includeTagging:true})
    .setCurrentTags(["color:red"])
    .addCurrentCustomFields({gender: "male", material: "cotton"})
    .setElements(["productpage-nosto-2"])
    .load()
    .then(response => {
      console.log(response);
    });
```

In many cases leveraging the existing tagging on the page and then overriding the specific parts is simpler and more robust.

In case the request is built completely manually, it needs to include also the attribution information for click tracking. Below is an example which shows how that can be done for product views that result from clicking on a Nosto recommendation slot.

In the event that the product was viewed due to a click on a Nosto recommendation slot, you must send an additional parameter that denotes the identifier of the recommendation slot that was clicked. If this is done inside of recommendation template identifier of recommendation slot can be get from property `$!product.attributionKey`

```javascript
nostojs(function(api){
   var request = api.createRecommendationRequest()
     .addEvent('vp', "product-id", "recommendation-id")
     .load();
});
```

**Note:** Failure to do will result in incorrect attribution statistics.

