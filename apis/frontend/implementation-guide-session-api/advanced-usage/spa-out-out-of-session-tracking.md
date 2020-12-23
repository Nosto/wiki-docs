# Opt-Out and Do-Not-Track

Nosto uses a cookie to set to identify browsers between their visits to the site. The purpose and usage of this cookie is very similar to how web analytics tools, such as Google Analytics, work.

Nosto supports opting out of session tracking via the "Do Not Track" feature. If you leverage Nosto's "Do Not Track", Nosto will avoid setting a session identifier.

{% hint style="info" %}
This feature does not disable cookies entirely. It only disables the `2c.cId` cookie that is used for identifying browsers between their visits to the site.
{% endhint %}``

## Opting out of session tracking

You can opt-out of tracking by using the `setDoNotTrack` method. You must do so before making any requests to Nosto. 

```js
nostojs(api => api.visit.setDoNotTrack(true));
```

Having done so, you can also veriify that you have opted out by using the `isDoNotTrack()` method

```js
nostojs(api => console.log(api.visit.isDoNotTrack()));
```
