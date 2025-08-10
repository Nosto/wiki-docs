## Refreshing Recommendations

When you need to reload the recommendations on the page, you can use this call to reload the recommendations:

```javascript
nostojs(api => {
  api.loadRecommendations();
});
```

The call accepts an optional parameter, but in most cases, that parameter is not required. Add the optional parameter only if the reason to reload new recommendations is the result of clicking an existing recommendation and some new product was loaded. A typical use case where this would be the case is when a recommendation has a quick view button that dynamically loads a new product into a CSS overlay with ajax.

```javascript
nostojs(api => {
  api.loadRecommendations({markNostoElementClicked: "nosto­categorypg­1"});
});
```

# Conditional loading

By default, Nosto loads the recommendations as soon as the site’s DOM is loaded. In some use cases, Nosto needs to be loaded manually. This is done by disabling autoload directly after the nosto script:

```javascript
nostojs(api => {
  api.setAutoLoad(false)
});
```

and conditionally later to trigger loading

```javascript
nostojs(api => {
  api.load();
});
```

This pattern should not be used to load Nosto faster, but as a pattern to conditionally load Nosto. If `api.load()` is called before this site's DOM is loaded tagging data won't be fully available and Nosto will not function correctly.
