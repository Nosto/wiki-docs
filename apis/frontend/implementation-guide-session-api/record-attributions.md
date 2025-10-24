# Record Attribution

## Summary

Nosto already offers the Session API, for recording attribution. But it's limited to `vp` events so far. In an attempt to expand this functionality to any event types, a new Session API method `recordAttribution` has been introduced. This method can be utilized to update current visit with details of events for attribution.

{% hint style="warning" %}
Deprecated

The `recordAttribution` usage with Session API is deprecated and no longer recommended. The API will continue to work without any issues but we recommend using the JS API version. For more info on JS API, please refer [JS API/Record Attribution](https://docs.nosto.com/techdocs/apis/frontend/js-apis/record-attribution)
{% endhint %}

## Usage

The Session API method `recordAttribution` accepts one event at a time, but users can chain multiple calls to `recordAttribution` and add as many events as they want.

### Parameters

<table><thead><tr><th width="271" align="center">name</th><th align="center">field type</th><th align="center">is required</th><th align="center">description</th></tr></thead><tbody><tr><td align="center">type</td><td align="center">string</td><td align="center">yes</td><td align="center">type of event to which a placement (ref) should be attributed</td></tr><tr><td align="center">target</td><td align="center">string</td><td align="center">yes</td><td align="center">id of the element that's been loaded as a result of the event</td></tr><tr><td align="center">ref</td><td align="center">string</td><td align="center">no</td><td align="center">recommendation id of the result that contained the link which triggered the event</td></tr><tr><td align="center">refSrc</td><td align="center">string</td><td align="center">no</td><td align="center">product id of the parent element of the link that triggered the event</td></tr></tbody></table>

### Event Types

Nosto supports following predefined event types

|               Type               | Description                                                                                                                                                                                                                                                                |
| :------------------------------: | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|         View Product (VP)        | An event associated with viewing a single product                                                                                                                                                                                                                          |
|        View Category (VC)        | An event associated with event a single category of items                                                                                                                                                                                                                  |
|       Internal Search (IS)       | An event associated with results of search internal to a merchant's website                                                                                                                                                                                                |
|         Add to cart (CP)         | An event associated with adding a product or bundle to a cart                                                                                                                                                                                                              |
|      External Campaign (EC)      | An event associated with campaigns, which are not part of nosto, that directed a user to a merchant website. These campaigns will contain google's UTM parameters ([UTM parameters](https://en.wikipedia.org/wiki/UTM_parameters) for more info.) in the request URL     |
|       Custom Campaign (CC)       | An event associated with campaigns, which are not part of nosto, that directed a user to a merchant website. These campaigns will not contain google's UTM parameters in the request URL                                                                                 |
|       External Search (ES)       | An event associated with search outside of merchant's website (Google for e.g.) that brought the user to merchant's website                                                                                                                                                |
|         Give Coupon (GC)         | An event associated with a coupon code campaign popup in which the customer has acted upon                                                                                                                                                                                 |
|           Source (SRC)           | An event associated with a customer action which needs to be attributed to one of Nosto's feature (api, email, rec, imgrec, cmp). Here the information is usually passed through a pre-configured source parameters name (`nosto_source`, by default) in the request URL |
| Cart Popup Recommendations (CPR) | An event associated with a recommendation, that's shown when a product is added to cart, in which a customer has acted upon                                                                                                                                                |
|          Page Load (PL)          | An event associated with a page load merchant's website                                                                                                                                                                                                                    |
|      Content Campaign (CON)      | Event triggered when a customer performs an action inside a content campaign                                                                                                                                                                                               |
