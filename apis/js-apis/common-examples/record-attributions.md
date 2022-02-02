# Record Attribution

## Summary

Nosto already offers session API, for recording attribution. But it's limited to `vp` events so far. In an attempt to expand this functonality to any event types, a new session API method `recordAttribution` has been introduced. Using JS API, this method can be utilised to update current visit with details of events for attribution.

## Usage

The session API mthod `recordAttribution` accepts one event at a time, but use case chain multiple calls to `recordAttribution` and add as many events as they want.

### Method documentation

class: session.js name: recordAttribution parameters:

|  name  | field type | is required |                            description                           |
| :----: | :--------: | :---------: | :--------------------------------------------------------------: |
|  type  |   string   |     yes     |   type of event to which a placement (ref) should be attributed  |
| target |   string   |     yes     |   id of the element that's been loaded as a result of the event  |
|   ref  |   string   |      no     | id of the element that hosted the link which triggered the event |
| refSrc |   string   |      no     |     id of parent element of the link that triggered the event    |

### Event Types
Nosto supports following pre-defined event types
|  Type  | Description | 
| :----: | -------- |
| View Product (VP) | An event associated with viewing a single product |
| Like Product (LP) | An event associated with liking a single product |
| Dislike Product (DP) | An event associated with disliking a product |
| Bought Product (BP) | An event assiated with every product that belonged to a completed order. Basically, a product that has been purchased | 
| View Category (VC) | An event associated with event a single category of items |
| Order (OR) | An event associated with a single order |
| Internal Search (IS) | An event associated with results of search internal to a merchant's website |
| Add to cart (CP) | An event associated with adding a product or bundle to a cart |
| External Campaign (EC) | An event associated with campaigns, which are not part of nosto, that directed a user to a merchant website. These campaigns will contain google's UTM parameters ([UTM parameters](https://en.wikipedia.org/wiki/UTM_parameters) for more info.) in  `ev1` request URL |
| Custom Campaign (CC) | An event associated with campaigns, which are not part of nosto, that directed a user to a merchant website. These campaigns will not contain google's UTM parameters in `ev1` request URL |
| External Search (ES) | An event associated with search outside of merchant's website (Google for e.g.) that brought the user to        merchant's website |
| Give Coupon (GC) | An event associated with a coupon code campaign popup in which the customer has acted upon |
| Source (SRC) | An event associated with a customer action which needs to be attributed to one of Nosto's feature (api, email, rec, imgrec, cmp). Here the information is usually passed through a pre-configured source parameters name (`nosto_source`, by default) in `ev1` request URL |
| Cart Popup Recommendations (CPR) | An event associated with a recommendation, that's shown when a product is added to cart, in which a customer has acted upon |
| Page Load (PL) | An event associated with a page load merchant's website |

### Examples

1. Attributing a placement click to a `vp`  (View Product) event

```javascript
nostojs(function(api) {
  api
  .defaultSession()
  .recordAttribution("vp", "12345678", "frontpage-nosto-1")
  .done()
});
```
In the above example,
- `vp` specifies the type of event and it corresponds to View Product
- `12345678` specifies the target and it corresponds to the ID of the product that's being viewed
- `frontpage-nosto-1` specifies tge reference and it corresponds to the placement that hosted the product that's being clicked


2. Attributing a placement click to a `cc`  (Custom Campaign) event

```javascript
nostojs(function(api) {
  api
  .defaultSession()
  .recordAttribution("cc", "12345678", "frontpage-nosto-1")
  .done()
});
```
In the above example,
- `cc` specifies the type of event and it corresponds to Custom Campaign
- `12345678` specifies the target and it corresponds to the ID of the product that's being viewed
- `frontpage-nosto-1` specifies the reference and it corresponds to the placement that hosted the product that's being viewed

3.  Adding the fourth `refSrc` parameter

```javascript
nostojs(api => api
.defaultSession()
.recordAttribution("vp", "7513863258337", "productpage-nosto-3", "7513872007393")
.done()
```

In the above example,
- `vp` specifies the type of event and it corresponds to View Product
- `7513863258337` specifies the target and it corresponds to the ID of the product that's being viewed
- `productpage-nosto-3` specifies the reference and it corresponds to the placement that hosted the product that's being clicked
- `7513872007393` specifies the reference source and it corresponds to the ID of the product displayed in current page (PDP), that contained the `ref` element, (`productpage-nosto-3`)

Here we are recording a `View Product` event for product 7513863258337 which was clicked from the recommendation slot `productpage-nosto-3` while on another product page `7513872007393`

In similar way, we will be able to record attribution for all the event types listed under `Event Types` section of this documentation.
