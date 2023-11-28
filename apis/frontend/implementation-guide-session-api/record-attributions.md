# Record Attribution

## Summary

Nosto already offers the Session API, for recording attribution. But it's limited to `vp` events so far. In an attempt to expand this functionality to any event types, a new Session API method `recordAttribution` has been introduced. Using the JS API, this method can be utilised to update current visit with details of events for attribution.

## Usage

The Session API method `recordAttribution` accepts one event at a time, but users can chain multiple calls to `recordAttribution` and add as many events as they want.

### Method documentation

class: `session.js` name: `recordAttribution` parameters:

|  name  | field type | is required |                            description                           |
| :----: | :--------: | :---------: | :--------------------------------------------------------------: |
|  type  |   string   |     yes     |   type of event to which a placement (ref) should be attributed  |
| target |   string   |     yes     |   id of the element that's been loaded as a result of the event  |
|   ref  |   string   |      no     | id of the element that hosted the link which triggered the event |
| refSrc |   string   |      no     |     id of parent element of the link that triggered the event    |

### Event Types

Nosto supports following predefined event types

|               Type               | Description                                                                                                                                                                                                                                                                |
| :------------------------------: | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|         View Product (VP)        | An event associated with viewing a single product                                                                                                                                                                                                                          |
|        View Category (VC)        | An event associated with event a single category of items                                                                                                                                                                                                                  |
|       Internal Search (IS)       | An event associated with results of search internal to a merchant's website                                                                                                                                                                                                |
|         Add to cart (CP)         | An event associated with adding a product or bundle to a cart                                                                                                                                                                                                              |
|      External Campaign (EC)      | An event associated with campaigns, which are not part of nosto, that directed a user to a merchant website. These campaigns will contain google's UTM parameters ([UTM parameters](https://en.wikipedia.org/wiki/UTM\_parameters) for more info.) in `ev1` request URL    |
|       Custom Campaign (CC)       | An event associated with campaigns, which are not part of nosto, that directed a user to a merchant website. These campaigns will not contain google's UTM parameters in `ev1` request URL                                                                                 |
|       External Search (ES)       | An event associated with search outside of merchant's website (Google for e.g.) that brought the user to merchant's website                                                                                                                                                |
|         Give Coupon (GC)         | An event associated with a coupon code campaign popup in which the customer has acted upon                                                                                                                                                                                 |
|           Source (SRC)           | An event associated with a customer action which needs to be attributed to one of Nosto's feature (api, email, rec, imgrec, cmp). Here the information is usually passed through a pre-configured source parameters name (`nosto_source`, by default) in `ev1` request URL |
| Cart Popup Recommendations (CPR) | An event associated with a recommendation, that's shown when a product is added to cart, in which a customer has acted upon                                                                                                                                                |
|          Page Load (PL)          | An event associated with a page load merchant's website                                                                                                                                                                                                                    |
|      Content Campaign (CON)      | Event triggered when a customer performs an action inside a content campaign                                                                                                                                                                                               |

### Examples

1. Attributing a placement click to a `vp` (View Product) event

```javascript
nostojs(api => {
  api
  .defaultSession()
  .recordAttribution("vp", "12345678", "frontpage-nosto-1")
  .done()
});
```

In the above example,

* `vp` specifies the type of event and it corresponds to View Product
* `12345678` specifies the target and it corresponds to the ID of the product that's being viewed
* `frontpage-nosto-1` specifies the slot’s ID from the placement that hosted the product that’s being clicked

1. Attributing a placement click to a `cc` (Custom Campaign) event

```javascript
nostojs(api => {
  api
  .defaultSession()
  .recordAttribution("cc", "12345678", "frontpage-nosto-1")
  .done()
});
```

In the above example,

* `cc` specifies the type of event and it corresponds to Custom Campaign
* `12345678` specifies the target and it corresponds to the ID of the product that's being viewed
* `frontpage-nosto-1` specifies the slot’s ID from the placement that hosted the product that’s being clicked

1. Adding the fourth `refSrc` parameter

```javascript
nostojs(api => api
.defaultSession()
.recordAttribution("vp", "7513863258337", "productpage-nosto-3", "7513872007393")
.done()
```

In the above example,

* `vp` specifies the type of event and it corresponds to View Product
* `7513863258337` specifies the target and it corresponds to the ID of the product that's being viewed
* `productpage-nosto-3` specifies the slot’s ID from the placement that hosted the product that’s being clicked
* `7513872007393` specifies the reference source and it corresponds to the ID of the product displayed in current page (PDP), that contained the `ref` element, (`productpage-nosto-3`)

Here we are recording a `View Product` event for product 7513863258337 which was clicked from the recommendation slot `productpage-nosto-3` while on another product page `7513872007393`

1. Attributing a click inside a content campaign to a `con` (Content Campaign) event

```javascript
nostojs(api => {
  api
  .defaultSession()
  .recordAttribution("con", "6ef452da787623f2")
  .done()
});
```

In the above example,

* `con` specifies the type of event and it corresponds to Content Campaign
* `6ef452da787623f2` specifies the target and it corresponds to the ID of the content campaign inside which a customer performs an action

In similar way, we will be able to record attribution for all the event types listed under `Event Types` section of this documentation.
