# Initializing Nosto

Nosto is initialized automatically once the script has loaded. Unlike the legacy script, you do not need to manually initialize Nosto.

## Disabling Autoloading

In the event that you would like to initialize Nosto but not have it automatically load the recommendations, you can set `setAutoLoad` to false.

```javascript
  nostojs(api => api.setAutoLoad(false));
```

_Note:_ You will need to manually load the recommendations when the autoloading is disabled. A recommendation request needs to be sent in order to track user events. So, if autoloading is disabled and you wish to track an order or cart changes, you need to send a recommendation request even if you don't wish to display recommendations on the page.
