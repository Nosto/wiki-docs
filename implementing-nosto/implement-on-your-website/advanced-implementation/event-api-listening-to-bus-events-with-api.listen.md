# Event API: Listening to Bus Events with api.listen

## Overview

Registers a listener for Nosto JS API events. Use this to react to specific lifecycle or user events dispatched by the Nosto client.

```javascript
api.listen(event: BusEvent, callback: (...args) => void)
```

Check out the API documentation for [listen](https://nosto.github.io/nosto-js/interfaces/client.API.html#listen)

### Example Usage

```javascript
nostojs(api => {
  api.listen('taggingsent', (response) => {
    // 'response' from recommendation request 
    // consume response if necessary
    console.log('Tagging data was sent to Nosto');
  });
});
```

#### Supported `BusEvent` Types

The following table lists all event types supported by the `listen` API. See [this](https://nosto.github.io/nosto-js/interfaces/client.EventMapping.html) for API documentation on each of these event types and it's associated payload

#### Lifecycle Events

{% hint style="info" %}
A Nosto recommendation request is sent to the `/ev1` endpoint and returns the product recommendation(s) for placements injected on the page.

A recommendation response is the response returned from the `/ev1` endpoint.
{% endhint %}

<table><thead><tr><th width="228.82421875">Event Name</th><th>Description</th></tr></thead><tbody><tr><td>prerequest</td><td>Before a recommendation request is sent to Nosto. Payload is the data that's sent in the recommendation request. Refer to our API documentation on <a href="https://nosto.github.io/nosto-js/interfaces/client.EventMapping.html#prerequest">prerequest</a>.</td></tr><tr><td>prerender</td><td>After receiving response from the recommendation request but before recommendations are rendered. Refer to our API documentation on <a href="https://nosto.github.io/nosto-js/interfaces/client.Prerender.html">prerender</a>.</td></tr><tr><td>postrender</td><td><p>**Only For HTML response mode.</p><p>After recommendations are rendered/injected on the page. Refer to our API documentation on <a href="https://nosto.github.io/nosto-js/interfaces/client.Postrender.html">postrender</a>.</p></td></tr><tr><td>taggingsent</td><td>After receiving recommendation response and the recommendations are rendered. Payload is the response data from the recommendation request, all placement - campaign markup mapping that will be injected (unFilledElements) and all placement -campaign markup mapping that's injected (filledElements). Refer to our API documentation on <a href="https://nosto.github.io/nosto-js/interfaces/client.EventMapping.html#taggingsent">taggingsent</a>.</td></tr><tr><td>taggingresent</td><td>When tagging data is resent. Associated with <a href="https://nosto.github.io/nosto-js/interfaces/client.API.html#sendtagging">sendTagging</a> (a.k.a resendAllTagging) API. Payload is the tagging data from the store page. Refer to our API documentation on <a href="https://nosto.github.io/nosto-js/interfaces/client.EventMapping.html#taggingresent">taggingresent</a>.</td></tr><tr><td>carttaggingresent</td><td>When cart contents are resent to Nosto either using the <a href="https://nosto.github.io/nosto-js/interfaces/client.API.html#resendcarttagging">resendCartTagging</a> or <a href="https://nosto.github.io/nosto-js/interfaces/client.API.html#resendcartcontent">resendCartContent</a> API. Payload is the cart items extracted from tagging (resendCartTagging) or the cart items from the supplied cart object (resendCartContent). Refer to our API documentation on <a href="https://nosto.github.io/nosto-js/interfaces/client.EventMapping.html#carttaggingresent">carttaggingresent</a>.</td></tr><tr><td>customertaggingresent</td><td>When customer info is resent to Nosto from page tagging. Associated with the <a href="https://nosto.github.io/nosto-js/interfaces/client.API.html#resendcustomertagging">resendCustomerTagging</a> API. Payload is the customer info extracted from the page tagging. Refer to our API documentation on <a href="https://nosto.github.io/nosto-js/interfaces/client.EventMapping.html#customertaggingresent">customertaggingresent</a>.</td></tr><tr><td>emailgiven</td><td>When customer info is sent to Nosto either using the <a href="https://nosto.github.io/nosto-js/interfaces/client.API.html#customer">customer</a> API or using the discount popup. Payload is the customer object which is being sent to Nosto. Refer to our API documentation on <a href="https://nosto.github.io/nosto-js/interfaces/client.EventMapping.html#emailgiven">emailgiven</a>.</td></tr></tbody></table>

#### Popup Events

<table><thead><tr><th width="254.6640625">Event Name</th><th>Description</th></tr></thead><tbody><tr><td>popupopened</td><td>When a discount popup is opened and displayed on the store page. Payload is the campaign ID associated with the popup and the trigger that caused the popup to display. Refer to our API documentation on <a href="https://nosto.github.io/nosto-js/interfaces/client.EventMapping.html#popupopened">popupopened</a>.</td></tr><tr><td>popupmaximized</td><td>When a popup is maximized from ribbon mode. Payload is the campaign ID associated with the popup. Refer to our API documentation on <a href="https://nosto.github.io/nosto-js/interfaces/client.EventMapping.html#popupmaximized">popupmaximized</a>.</td></tr><tr><td>popupminimized</td><td>When a popup is minimized into a ribbon. Payload is the campaign ID associated with the popup. Refer to our API documentation on <a href="https://nosto.github.io/nosto-js/interfaces/client.EventMapping.html#popupminimized">popupminimized</a>.</td></tr><tr><td>popupclosed</td><td>When a popup is closed in the page. Refer to our API documentation on <a href="https://nosto.github.io/nosto-js/interfaces/client.EventMapping.html#popupclosed">popupclosed</a>.</td></tr><tr><td>popupribbonshown</td><td>When a popup ribbon is activated on page load or when popup is minimized. Refer to our API documentation on <a href="https://nosto.github.io/nosto-js/interfaces/client.EventMapping.html#popupribbonshown">popupribbonshown</a>.</td></tr></tbody></table>

#### Tagging Events

<table><thead><tr><th width="256.66796875">Event Name</th><th>Description</th></tr></thead><tbody><tr><td>cartupdated</td><td><p>**Only for Shopify merchants.</p><p>Whenever the a product is added or removed from cart. Refer to our API documentation on <a href="https://nosto.github.io/nosto-js/interfaces/client.EventMapping.html#cartupdated">cartupdated</a>.</p></td></tr></tbody></table>

### Unregistering listener

Use the `unlisten` method to remove a previously registered event handler for a specific event type, using the `listen` API method. Check out the API documentation for [unlisten](https://nosto.github.io/nosto-js/interfaces/client.API.html#unlisten).

#### Example usage

```javascript
// Register a listener
api.listen('taggingsent', onTaggingSent);

// Unregister the listener
api.unlisten('taggingsent', onTaggingSent);

function onTaggingSent(response) {
  console.log('Tagging sent:', response);
}
```

**Note:**\
If the callback was not previously registered for the event, calling `unlisten` has no effect.
