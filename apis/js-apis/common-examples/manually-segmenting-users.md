# Manually segmenting users

While Nosto leverages customer attributes and behavioral signals to automatically segment users, there may be scenarios where you may want to explicitly segment users. You can do so by affixing a segment-code to the current user and then leverage Nosto's segmentation tool to segment users with that specified attribute.

```javascript
nostojs(api => {
  api.addSegmentCodeToVisit('women');
});
```

The example above does not trigger a reload of all the placements. If you would like to reload all onsite content to reflect the new segment, you must explicitly reload it.

```javascript
nostojs(api => {
  api.loadRecommendations();
});
```

Once you have done so, read our guide on [segmenting users via custom events](#user-content-fn-1)[^1].

[^1]: 
