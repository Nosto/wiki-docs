# Implement Personalized Campaign Widgets

How to implement Product Recommendations, Dynamic Bundles and Onsite Content Personalization.

## Prerequisites

- [ ] Nosto account with working product sync (promotable products)
- [ ] Nosto script in the frontend (Nosto Debug Toolbar is loading)
- [ ] Knowledge of running a SPA or classic web application
- [ ] General understanding of [how Nosto works](../../getting-started/README.md) and [what components make a stable Nosto implementation](../../getting-started/building-your-implementation-plan.md#components-of-a-stable-nosto-implementation.md)
- [ ] One or multiple Nosto modules enabled in your account:
  - [ ] OCP: Onsite Content Personalization (like banners or text)
  - [ ] RECs/Dynamic Bundles: Product Recommendations (like "You might be interested in" or "Complete the look")


## Good to know before you start

Every Nosto account comes with a set of default product recommendation campaigns and "placements" (empty `<div/>` elements). These are sorted into the different page types like homepage (e.g. `#nosto-frontpage-1`), PLP, PDP, SERP, 404 page as well as general layout areas like the mini-cart drawer or search overlay/autocomplete.

Your client will tell you which placements to put where inside your templates (or has already defined those within a design file). Nosto campaigns need to be injected into the placements - automatically or manually, depending on your tech stack and implementation method.

Templates for RECs campaigns can be hosted and maintained in Nosto or built within your own code base (API approach, recommended for headless and SPAs).

Depending on your implementation method and tech stack, different options to attribute clicks from Nosto campaigns are available (it might need a few lines of custom code, you'll find details below per implementation method).

OCP and RECs campaigns are always associated with exactly one placement. The placements are also used for A/B testing, e.g. testing campaign A vs. campaign B inside of placement `#nosto-productpage-1`.


## How it works

1. After the Nosto script is loaded on a page, you can send a request to Nosto and will receive the campaigns for this specific page.
2. Your request to Nosto needs to include certain data like the current page type, what's on the page (e.g. product data, the current category or search term), what's in the cart, if the customer is logged in and what placements are on the page.
   - You'll see [this "ev1" request](https://nosto.github.io/nosto-js/interfaces/client.EventRequestMessageV1.html) in your network tab and will monitor it extensively while implementing Nosto.
3. The Nosto response includes mostly the campaigns but also meta data like the number of pages visited in the current session, a session ID, a customer ID and more.
   - OCP campaigns are always returned as raw HTML.
   - RECs/Bundles campaigns can be returned as raw HTML or JSON.
4. Depending on your tech stack and templating method, the campaigns get automatically injected into the page (conventional) or need to be explicitly rendered (advanced).
5. Interactions with Nosto campaigns (like clicking on a product, selecting a variant/color swatch or clicking on a banner) need a certain attribution that always follows the same pattern: *"This event X (page/product/variant/… has been viewed/selected) after an interaction with the campaign Y (on page Z (optional))."*:
    - Product ID 8 was viewed after a click in RECs campaign `nosto-pdp-top` on the PDP with product ID 4.
    - Product ID 6 was viewed after a click (quick view modal or PDP redirect) in RECs campaign `nosto-frontpage-mid`.


## Implementation Methods

Since OCP campaigns always return HTML content, this guide only compares product recommendations (RECs and Bundles).

### Client: Automatic Injection with Nosto Autoloading

This method is the fastest and works best for conventional builds where the templates are built within the Nosto backend with [Apache Velocity](https://help.nosto.com/en/articles/588949-templating-language-reference).

- By default, the Nosto autoloader is enabled and content will be automatically injected into the templates on the page.
- Attribution is automatically handled by Nosto as it knows which HTML template was used in what RECs campaign.
- If your client is on Shopify, we recommend to evaluate our [dynamic product cards](https://docs.nosto.com/shopify/styling-options-dynamic-product-cards) which allow you to re-use your existing product cards.
- You can make use of several [Nosto-variables](https://help.nosto.com/en/articles/2002516-available-variables-and-attributes-for-nosto-campaigns) inside of your template (mostly applicable for clients not using the dynamic product cards).
- **This approach is not suitable for SPAs** since interactions trigger a full page load.

### Client: JS API: `createRecommendationRequest()`

In case you want more control about the campaign loading, you can disable autoloading and request the campaigns yourself.

- The campaigns can return `HTML` (default, for Nosto-hosted templates) or `JSON`, depending on how you [build the request](https://nosto.github.io/nosto-js/interfaces/client.API.html#createrecommendationrequest).
- By default, the request does not know anything about the current page, placements on the current page, logged in customer, cart content etc. and you have two options of passing that data:
  - Including the HTML tagging with `{includeTagging: true}`
Setting the data manually via JS, e.g. `setPageType("product").setProducts([product_id: "4"])`
    - In most cases it will be sufficient to only include the page tagging since it reads the current product, cart content etc. and add a `.setPlacements(api.placements.getPlacements())` call.
    - Advanced cases where you need to explicitly set data occur when e.g. a variant has been selected on a PDP or if products should be filtered by a certain tag (e.g. for cannabis state-specific regulation or for vehicle-specific parts).
- Attribution is automatically handled by Nosto when using the default `response mode HTML`. 
  - If you use the `JSON response mode`, you can [simplify attribution by creating a custom HTML element](../../apis/js-apis/recommendations/sending-product-view-events) and using the `api.attributeProductClicksInCampaign()` method from Nosto ([reference](https://nosto.github.io/nosto-js/interfaces/client.API.html#attributeproductclicksincampaign)).
- This approach is recommended for custom frontend builds, we recommend looking into the [Nosto Open Source packages](../../apis/frontend/oss/README.md).
This approach is **not suitable for SPAs or headless frontends**, please see `Session API: defaultSession()` below.

### Client: Session API: `defaultSession()`

In case you are running a **SPA or headless frontend**, you want more control about the campaign loading and need to disable autoloading to request the campaigns yourself.

- The campaigns return `JSON` by default, but in comparison to the JS API, you incorporate requesting campaigns with your page tagging/tracking via `defaultSession()` ([reference](https://nosto.github.io/nosto-js/interfaces/client.API.html#defaultsession)).
    - *Page tagging* refers to the JS API ([taggingProvider](https://nosto.github.io/nosto-js/interfaces/client.API.html#settaggingprovider)) and uses HTML and **MUST NOT** be mixed with the Session API.
    - *Page tracking* is similar to the page tagging but uses JavaScript calls to let Nosto know what is on the current page.
- The page tracking sends the same ["ev1" request](https://nosto.github.io/nosto-js/interfaces/client.EventRequestMessageV1.html) which responds with the same campaign data and the same principles for the JS API apply.
- Since requesting campaigns is tied to the `defaultSession()` for page tracking, you can run a very similar code block on the different page types ([examples here](../../apis/frontend/implementation-guide-session-api/spa-basics-tracking-events.md) and in the [API reference](https://nosto.github.io/nosto-js/interfaces/client.Session.html)).
  - The methods like `viewFrontPage()` or `viewProduct("4")` are the main indicator that campaigns will be returned.
  - Adding `setPlacements(api.placements.getPlacements())` or passing the placement IDs explicitly as an array determines from where campaigns will be requested. We recommend the first approach, getting all campaigns for all placements instead of requesting them one by one.
- Calling `load()` sends the request to Nosto, the returned Promise can be handled async or by chaining a `then()` to the request.
- Since there is no page tagging, you need to use the ["Visitor" tab in the Nosto Debug Toolbar](../checking-your-setup.md) and the "ev1" request in your network tab for verification and QA.
- There are several advanced cases to keep in mind and cover:
  - Using the category path that's in Nosto, not the URL slug.
  - [Sending an additional event when a specific SKU has been selected (either on a PDP or via a "quick view" modal.](../../apis/frontend/implementation-guide-session-api/spa-basics-leveraging-features#handling-attribution)
  - Pay close attention to the `setRef()` method ([reference](https://nosto.github.io/nosto-js/interfaces/client.Action.html#setref-1)) - **the second parameter is the recommendation slot id** (`result_id` of the response), *not the placement div id*.
- [Using `load()` only on the first request on the current page](../../apis/frontend/implementation-guide-session-api/spa-basics-leveraging-features#reporting-correct-page-views-load-vs-update) because it increments the page view counter (`pv` in the ["ev1" response](https://nosto.github.io/nosto-js/interfaces/client.EventResponseMessage.html#pv)). On subsequent requests on the same page you must send the request with `update()` or [pass a recommendation request flag like `.load{skipPageViews: true}`](https://nosto.github.io/nosto-js/interfaces/client.Action.html#load) ([details here](../../apis/frontend/implementation-guide-session-api/spa-basics-leveraging-features#reporting-correct-page-views-load-vs-update)).
- You can still `setResponseMode("HTML")` and request the Nosto-hosted templates if you're not running a SPA. The click attribution and template injection can be automated by calling `enableCampaignInjection()` ([example](../../implementing-nosto/implement-on-your-website/advanced-implementation/parameterless-attribution#session-api-based-usage)).


### Server: GraphQL API: `updateSession()`

In case you don't want follow one of the client-based approaches, you can manage the Nosto session and campaign rendering via GraphQL.

**Please beware of the following limitations:**
- You request the campaigns for a specific product ID or category (without placements) and will receive the RECs campaign IDs directly.
  - Therefore, you **can't use Nosto built-in A/B testing** for campaign widgets.
  - *You need an alternative, full page A/B testing like Omniconvert* in this case.
- **Nosto OCP cannot be retrieved via GraphQL** (personalized banners or other HTML content).

- The page tagging/event tracking (current customer data and shopping cart) can also be done via [a GraphQL mutation that returns the session ID](../../apis/graphql-an-introduction/graphql-using-mutations/README.md), example:
```graphql
mutation {
    updateSession(id: "ad8f0d0e-1156-4df2-b385-10e03f8f8a44",
    params: {
      customer: {
        firstName: "John"
        lastName: "Doe"
        marketingPermission: true
        customerReference: "319330"
      }
      event: {
        type: VIEWED_PRODUCT
        target: "400"
      }
      cart: {
        items: [
          {
            productId: "100",
            skuId: "100-1",
            name: "Product 100",
            unitPrice: 199,
            priceCurrencyCode: "EUR",
            quantity: 1
          }
        ]
      }
    }) {
      id
    }
}
```

- The request/response concept is the same as with the Session API: specify data about the session (cart and customer, see above), [request product recommendations for a given page type](../../apis/graphql-an-introduction/graphql-using-mutations/graphql-onsite-sessions/README.md#working-with-recommendations) and render the template while keeping attribution in mind. You will need to:
  - Set the `params.event` to match the current page type and specific e.g. the product ID, category path or search term for correct tracking and attribution.
    - The `ref` parameter must match the `resultId` (= Nosto recommendation campaign slot ID) from the response (example below).
  - Set the correct page type (`PageRequestEntity` in the API reference) under `pages` to specify the context from which you want to receive Nosto campaign data.
- Parse the [Nosto response](../../apis/graphql-an-introduction/graphql-using-mutations/graphql-onsite-sessions/README.md#attribution-of-recommendation-results) and render your template.
  - Make sure you save the `resultId` and pass it to your next `updateSession(params: { event: { type: VIEWED_PRODUCT } } )` as `ref` for attribution when a shopper clicks on one of the products inside of your template.


## Comparison Table

| Feature                                            | Automatic Injection with Nosto Autoloading             | JS API: createRecommendationRequest()                                                 | Session API: defaultSession()                                                           | Nosto Content via GraphQL                                                                  |
| :------------------------------------------------- | :----------------------------------------------------- | :------------------------------------------------------------------------------------ | :-------------------------------------------------------------------------------------- | :----------------------------------------------------------------------------------------- |
| **Best For**                                       | Conventional builds                                    | Custom frontend builds (non-SPA)                                                      | SPAs and Headless frontends                                                             | Mobile apps or server-side rendered builds (when Nosto A/B testing isn't needed)           |
| **How it Works**                                   | Content is automatically injected into page templates. | Manually request campaigns after disabling autoloading.                               | Request campaigns as part of the page tracking/tagging flow.                            | Request campaigns as part of the page tracking/tagging flow.                               |
| **Campaign Response Type**                         | HTML (for Nosto-hosted templates)                      | HTML (default) or JSON (per placement)                                                | JSON (default), but can be set to HTML (per placement)                                  | RECs campaign slot IDs (no placements, **OCP campaigns are *not* available**)              |
| **Attribution**                                    | Handled automatically by Nosto.                        | Automatic for HTML mode. For JSON mode, use `api.attributeProductClicksInCampaign()`. | Manual. Requires careful use of `setRef()` with the recommendation slot id (result_id). | Manual. Requires careful use of the `event` params in `updateSession()` mutation.          |
| **SPA Suitable**                                   | No (triggers a full page load)                         | No (recommended for custom builds, but not SPAs)                                      | Yes (designed for SPAs and Headless)                                                    | Yes                                                                                        |
| **Headless compatible**                            | No                                                     | No                                                                                    | Yes                                                                                     | Yes                                                                                        |
| **Fully customizable frontend**                    | Yes                                                    | Yes                                                                                   | Yes                                                                                     | Yes                                                                                        |
| **Suitable for complex use cases**                 | Sometimes                                              | Yes                                                                                   | Yes                                                                                     | Yes                                                                                        |
| **Customized and managed only in Nosto dashboard** | Yes (templates are in Nosto backend)                   | No (can build templates in own code base via JS)                                      | No (can build templates in own code base via JS)                                        | No (can build templates in own code base via server-side rendering)                        |
| **A/B Testing**                                    | Yes (via placements)                                   | Yes (via placements)                                                                  | Yes (via placements)                                                                    | No (Nosto built-in A/B testing is not available, needs full page testing like OmniConvert) |
| **Drawbacks**                                      | Not suitable for SPAs or Headless.                     | Not suitable for SPAs or Headless.                                                    | Complex event tracking and attribution                                                  | **No Nosto A/B testing.**<br>**No OCP (HTML content/banners).**                            |