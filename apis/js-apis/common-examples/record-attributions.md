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
* View Product (VP)
* Like Product (LP)
* Dislike Product (DP)
* Remove Product (RP)
* Bought Product (BP)
* View Category (VC)
* Order (OR)
* Internal Search (IS)
* Add to cart (CP)
* External Campaign (EC)
* External Search (ES)
* Give Coupon (GC)
* Source (SRC)
* Cart Popup Recommendations (CPR)
* Page Load (PL)
* Custom Campaign (CC)
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
- `frontpage-nosto-1` specifies the reference and it corresponds to the placement that hosted the product that's being viewed


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
- `frontpage-nosto-1` specifies tge reference and it corresponds to the placement that hosted the product that's being clicked

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
- `7513872007393` specifies the reference source and it corresponds to the ID of element, of the current page, that contained the `ref` element, `productpage-nosto-3` in this case

Here we are recording a `View Product` event for product 7513863258337 which was clicked from the recommendation slot `productpage-nosto-3` while on another product page `7513872007393`

In similar way, we will be able to record attribution for all the event types listed under `Event Types` section of this documentation.
