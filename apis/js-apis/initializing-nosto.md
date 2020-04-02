# Initializing Nosto

Nosto is initialized automatically once the script has loaded. Unlike the legacy script, you do not need to manually initialize Nosto.

## Disabling Autoloading

In the event that you would like to initialize Nosto but not have it automatically load the recommendations, you can set `setAutoLoad` to false.

```javascript
  nostojs(api => api.setAutoLoad(false));
```

_Note:_ you will need to manually load the recommendations when the autoloading is disabled.

