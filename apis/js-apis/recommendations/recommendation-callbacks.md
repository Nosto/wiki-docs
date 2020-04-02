# Recommendation Callbacks

The JS API can be used to register callbacks to hook into the recommendation events. To register a listener for a callback, use `api.listen(callbackId, callbackFunction)` function.

## Post Render Callback

Called when Nosto has responded and finished recommendation placement.

```javascript
nostojs(function(api) {
  api.listen('postrender', function(event) {
    console.log(event.filledElements);
    console.log(event.unFilledElements); 
  });
});
```

### Fields

| Field | Type | Reason |
| :--- | :--- | :--- |
| filledElements | Array | Contains a list of recommendation slots that contain recommendations |
| unFilledElements | Array | Contains a list of recommendations slots that did not have get recommendations. |

