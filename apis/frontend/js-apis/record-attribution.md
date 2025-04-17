# Record Attribution

The `recordAttribution` method provides a low-level standalone API to submit events to Nosto for processing in the backend.
It is to be used for standalone attribution submission that are not covered by the alternatives:
* Parameterless attribution
* Usage of attribution url parameters
* Session API
* Request API (api.createRecommendationRequest)

```typescript
nostojs(api => api.recordAttribution({ 
  type: "vp",
  target: "6829460553806", 
  ref: "191666803", 
  refType: "cmp" })
  .done() 
)
```

The API method `recordAttribution` accepts one event at a time, but users can chain multiple calls to `recordAttribution` and add as many events as they want.

## Parameters

<table><thead><tr><th width="271" align="center">name</th><th align="center">field type</th><th align="center">is required</th><th align="center">description</th></tr></thead><tbody><tr><td align="center">type</td><td align="center">string</td><td align="center">yes</td><td align="center">type of event to which a placement (ref) should be attributed. Refer <a data-mention href="record-attribution.md#event-types">#event-types</a></td></tr><tr><td align="center">target</td><td align="center">string</td><td align="center">yes</td><td align="center">id of the element that's been loaded as a result of the event</td></tr><tr><td align="center">ref</td><td align="center">string</td><td align="center">no</td><td align="center">id of the element that hosted the link which triggered the event</td></tr><tr><td align="center">refSrc</td><td align="center">string</td><td align="center">no</td><td align="center">id of parent element of the link that triggered the event</td></tr><tr><td align="center">targetFragment</td><td align="center">string</td><td align="center">no</td><td align="center">the <code>skuId</code> in case of `<code>vp`</code>events </td></tr><tr><td align="center">refType</td><td align="center">string</td><td align="center">no</td><td align="center">Refer <a data-mention href="record-attribution.md#event-ref-types">#event-ref-types</a></td></tr></tbody></table>

## Event Types

Nosto supports following predefined event types

|               Type               | Description                                                                                                                                                                                                                                                                |
| :------------------------------: | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
|         View Product (VP)        | An event associated with viewing a single product                                                                                                                                                                                                                          |
|        View Category (VC)        | An event associated with event a single category of items                                                                                                                                                                                                                  |
|       Internal Search (IS)       | An event associated with results of search internal to a merchant's website                                                                                                                                                                                                |
|         Add to cart (CP)         | An event associated with adding a product or bundle to a cart                                                                                                                                                                                                              |
|      External Campaign (EC)      | An event associated with campaigns, which are not part of nosto, that directed a user to a merchant website. These campaigns will contain google's UTM parameters ([UTM parameters](https://en.wikipedia.org/wiki/UTM_parameters) for more info.) in `ev1` request URL     |
|       Custom Campaign (CC)       | An event associated with campaigns, which are not part of nosto, that directed a user to a merchant website. These campaigns will not contain google's UTM parameters in `ev1` request URL                                                                                 |
|       External Search (ES)       | An event associated with search outside of merchant's website (Google for e.g.) that brought the user to merchant's website                                                                                                                                                |
|         Give Coupon (GC)         | An event associated with a coupon code campaign popup in which the customer has acted upon                                                                                                                                                                                 |
|           Source (SRC)           | An event associated with a customer action which needs to be attributed to one of Nosto's feature (api, email, rec, imgrec, cmp). Here the information is usually passed through a pre-configured source parameters name (`nosto_source`, by default) in `ev1` request URL |
| Cart Popup Recommendations (CPR) | An event associated with a recommendation, that's shown when a product is added to cart, in which a customer has acted upon                                                                                                                                                |
|          Page Load (PL)          | An event associated with a page load merchant's website                                                                                                                                                                                                                    |
|      Content Campaign (CON)      | Event triggered when a customer performs an action inside a content campaign                                                                                                                                                                                               |

## Event Ref Types

The `refType` (reference types) parameter is introduced as a replacement for Nosto's legacy `src`event. It's specifies the type of source (Nosto feature) that contributed to the attribution. The table below lists all possible reference types

| Ref Type | Description                                           |
| -------- | ----------------------------------------------------- |
| email    | Triggered email                                       |
| imgrec   | Email widgets                                         |
| rec      | Onsite recommendation (Nosto recommendation template) |
| api      | API recommendations (Session/JS API)                  |
| oc       | Onsite campaigns                                      |
| cmp      | Category merchandising                                |
| os       | Onsite search                                         |

## Examples

### Attributing a placement click to a `vp` (View Product) event

```javascript
nostojs(api => {
  api
  .recordAttribution({ type: "vp", target: "12345678", ref: "frontpage-nosto-1" })
  .done()
});
```

In the above example,

* `vp` specifies the type of event and it corresponds to View Product
* `12345678` specifies the target and it corresponds to the ID of the product that's being viewed
* `frontpage-nosto-1` specifies the slot’s ID from the placement that hosted the product that’s being clicked

### Attributing a placement click to a `cc` (Custom Campaign) event

```javascript
nostojs(api => {
  api
  .recordAttribution({ type: "cc", target: "12345678", ref: "frontpage-nosto-1" })
  .done()
})
```

In the above example,

* `cc` specifies the type of event and it corresponds to Custom Campaign
* `12345678` specifies the target and it corresponds to the ID of the product that's being viewed
* `frontpage-nosto-1` specifies the slot’s ID from the placement that hosted the product that’s being clicked

### Adding the fourth `refSrc` parameter

```javascript
nostojs(api => api
  .recordAttribution({ type: "vp", target: "7513863258337", ref: "productpage-nosto-3", refSrc: "7513872007393" })
  .done()
})
```

In the above example,

* `vp` specifies the type of event and it corresponds to View Product
* `7513863258337` specifies the target and it corresponds to the ID of the product that's being viewed
* `productpage-nosto-3` specifies the slot’s ID from the placement that hosted the product that’s being clicked
* `7513872007393` specifies the reference source and it corresponds to the ID of the product displayed in current page (PDP), that contained the `ref` element, (`productpage-nosto-3`)

Here we are recording a `View Product` event for product 7513863258337 which was clicked from the recommendation slot `productpage-nosto-3` while on another product page `7513872007393`

### Attributing a click inside a content campaign to a `con` (Content Campaign) event

```javascript
nostojs(api => {
  api
    .recordAttribution({ type: "con", target: "6ef452da787623f2" })
    .done()
})
```

In the above example,

* `con` specifies the type of event and it corresponds to Content Campaign
* `6ef452da787623f2` specifies the target and it corresponds to the ID of the content campaign inside which a customer performs an action

In similar way, we will be able to record attribution for all the event types listed under `Event Types` section of this documentation.
