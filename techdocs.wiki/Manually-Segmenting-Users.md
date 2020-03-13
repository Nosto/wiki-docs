While Nosto leverages customer attributes and behavioral signals to automatically segment users, there may be scenarios where you may want to explicitly segment users. You can do so by affixing a segment-code to the current user and then leverage Nosto's segmentation tool to segment users with that specified attribute.

```js
nostojs(function(api) {
  api.addSegmentCodeToVisit('women');
});
```

The example above does not trigger a reload of all the placements. Once you have placed the snippet into your page, all Nosto requests will automatically leverage the given segment-code. You must specify the segment code on every page load.

Once you have done so, read our guide on [segmenting users via custom events](https://help.nosto.com/articles/2398016-visit-custom-event).