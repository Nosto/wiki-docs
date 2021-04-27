# Supporting opt-out and do-not-track

you can disable Nosto for a given customer if they don't give consent for data processing or if legal reasons such as [COPPA](https://en.wikipedia.org/wiki/Children%27s_Online_Privacy_Protection_Act) requires to disable Nosto for an individual. 

This method disables the initialization of Nosto entirely, meaning that all associated Nosto features on your web store for that user are also disabled.

You will need to wrap the Nosto script in a conditional that activates only if the customer has accepted to activate Nosto. Below is an example that simply checks for the existence of a cookie called `accepts_marketing` but you should change the conditional to match your consent management implementation.

```text
<script type="text/javascript">
if (document.cookie.indexOf('accepts_marketing') >= 0) {
  var head = document.getElementsByTagName('head')[0];
  var js = document.createElement("script");
  js.type = "text/javascript";
  js.src = "//connect.nosto.com/include/$accountID";
  head.appendChild(js);
}
</script>
```

## Opting out of session tracking

Normally Nosto uses a cookie to identify browsers between their visits to the site. The purpose and usage of this cookie is very similar to how web analytics tools, such as Google Analytics, work.

You can use opting out of session tracking in case you want to enable Nosto's features in a limited manner where Nosto doesn't track browser sessions. When users opt out of session tracking, Nosto treats each request like it would be a new session that is then immediately discarded. In this mode, Nosto can still serve generic trend based recommendations, but any feature that relies on users actions over multiple requests will never activate.

{% hint style="info" %}
Any information sent to Nosto with the requests activated by the implementation will still be processed normally by Nosto's service even when users have opted out of session tracking.
{% endhint %}

Nosto supports opting out of session tracking via the "Do Not Track" feature. If you leverage Nosto's "Do Not Track", Nosto will avoid setting a session identifier.

{% hint style="info" %}
This feature does not disable cookies entirely. It only disables the `2c.cId` cookie that is used for identifying browsers between their visits to the site.
{% endhint %}

You can opt-out of tracking by using the `setDoNotTrack` method. You must do so before making any requests to Nosto.

```javascript
nostojs(api => api.visit.setDoNotTrack(true));
```

Having done so, you can also verify that you have opted out by using the `isDoNotTrack()` method

```javascript
nostojs(api => console.log(api.visit.isDoNotTrack()));
```

