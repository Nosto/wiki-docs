# Implement Personalized Campaign Widgets

How to implement Product Recommendations, Dynamic Bundles and Onsite Content Personalization.

**If you have custom requirements like customer group pricing/visibility or a highly complex product card**, *we recommend using one of the Nosto APIs to only retrieve the core product data via JSON and get prices and visibility from your platform instead of sending it to Nosto.*

If you only have a complex product card but are using a Shopify theme, you can consider using our [dynamic product cards](../../implementing-nosto/template-customization/product-cards.md)

## Prerequisites

- [ ] Nosto account with working product sync (promotable products)
- [ ] Nosto script in the frontend (Nosto Debug Toolbar is loading)
- [ ] Knowledge of running a SPA or classic web application
- [ ] General understanding of [how Nosto works](../../getting-started/what-nosto-does-and-how-it-works.md) and [what components make a stable Nosto implementation](../../getting-started/building-your-implementation-plan.md#components-of-a-stable-nosto-implementation.md)
- [ ] One or both Nosto modules enabled in your account:
  - [ ] OCP: Onsite Content Personalization (like banners or text)
  - [ ] Product Recommendations/Dynamic Bundles: Product Recommendations (like "You might be interested in" or "Complete the look")


## Good to know before you start

1. Every Nosto account comes with a set of default product recommendation campaigns and "placements" (empty `<div/>` elements). 
   - You can see the mapping of campaigns and placements in the Nosto Admin UI: Product Experience Cloud -> Recommendations.
   - These are sorted into the different page types like homepage (e.g. `#nosto-frontpage-1`), PLP, PDP, SERP, 404 page as well as general layout areas like the mini-cart drawer or search overlay/autocomplete.
2. Placements need to be set up in your store templates. On Shopify and Shopware, you can use our content blocks. For assistance in setting up placements on your store, please reach out to your Nosto POC or Nosto's support team. 
  - Nosto campaigns need to be injected into the placements - automatically or manually, depending on your tech stack and implementation method.
  - Nosto offers you several helper methods to inject campaign into the DOM, you'll find details at the end of this page.
3. Templates for Recommendation campaigns can be hosted and maintained in Nosto or built within your own code base (API approach, recommended for headless and SPAs when using a custom code setup).
4. Depending on your implementation method and tech stack, different options to attribute clicks from Nosto campaigns are available (it might need a few lines of custom code, you'll find details below per implementation method).
   - *Please note:* If you have the Nosto preview enabled, attribution/references will not show in the Nosto debug toolbar. You can do QA via the `ev1` request in your network tab (details below).
5. OCP and Recommendation campaigns are always associated with exactly one placement.
   - The placements are also used for A/B testing, e.g. testing campaign A vs. campaign B inside of placement `#nosto-productpage-1`.
   - Requesting campaigns via GraphQL is limited (no A/B testing, no dynamic filtering).


## How it works

1. After the Nosto script is loaded on a page, you can send a request to Nosto and will receive the campaigns for this specific page.
2. Your request to Nosto needs to include certain data like the current page type, what's on the page (e.g. product data, the current category or search term), what's in the cart, customer details if the customer is logged in and what placements are on the page.
   - You'll see [this `ev1` request](https://nosto.github.io/nosto-js/interfaces/client.EventRequestMessageV1.html) in your network tab and will monitor it extensively while implementing Nosto.
3. The [Nosto response](https://nosto.github.io/nosto-js/interfaces/client.EventResponseMessage.html) includes mostly the [recommendation campaigns](https://nosto.github.io/nosto-js/interfaces/client.EventResponseMessage.html#recommendations) but also meta data like the [number of pages visited](https://nosto.github.io/nosto-js/interfaces/client.EventResponseMessage.html#pv) in the current session, a session ID, a customer ID and more.
   - OCP campaigns are always returned as raw [HTML](https://nosto.github.io/nosto-js/interfaces/client.AttributedCampaignResult.html).
   - Product Recommendation campaigns can be returned as raw HTML or JSON ([JSONResult](https://nosto.github.io/nosto-js/interfaces/client.JSONResult.html) with [[JSONProduct](https://nosto.github.io/nosto-js/interfaces/client.JSONProduct.html)]).
4. Depending on your tech stack and templating method, the campaigns get automatically injected into the page (conventional) or need to be explicitly rendered (advanced).
5. Interactions with Nosto campaigns (like clicking on a product, selecting a variant/color swatch or clicking on a banner) need a certain attribution that always follows the same pattern: *"This event X (page/product/variant/… has been viewed/selected) after an interaction with the campaign Y (on page Z (optional))."*:
    - Product ID 8 was viewed after a click in Recommendation campaign `nosto-pdp-top` on the PDP with product ID 4.
    - Product ID 6 was viewed after a click (quick view modal or PDP redirect) in Recommendation campaign `nosto-frontpage-mid`.

Example [Nosto response](https://nosto.github.io/nosto-js/interfaces/client.EventResponseMessage.html): 

```json
{ 
    "campaigns": {
        "content": {
            "frontpage-banner": {
                "div_id": "frontpage-banner",
                "result_id": "5fc6390c60b2ecd3cc0c2d4f",
                "html": "< Campaign html content >",
                "params": ...
            }
        },
        "recommendations": {
            "frontpage-center-1": {
                "div_id": "frontpage-center-1",
                "result_id": "frontpage-center-1-fallback",
                "products": [ /* Array of products, typed as JSONProduct */ ],
                "params": ...
            }
        }
    }
}
```


## Implementation Methods

Since OCP campaigns always return HTML content, this guide only compares product recommendations (Recommendations and Bundles). (Example response and type reference above.)

### Client: Automatic Injection with Nosto Autoloading

This method is the fastest and works best for conventional builds where the templates are built within the Nosto backend with [Apache Velocity](https://help.nosto.com/en/articles/588949-templating-language-reference).
**This approach is not suitable for SPAs** since interactions trigger a full page load.

- By default, the Nosto autoloader is enabled and content will be automatically injected into the templates on the page.
- Attribution is automatically handled by Nosto as it knows which HTML template was used in what campaign.
- If you are on Shopify, we recommend to evaluate our [dynamic product cards](https://docs.nosto.com/shopify/styling-options-dynamic-product-cards) which allow you to re-use your existing product cards.
- You can make use of several [Nosto-variables](https://help.nosto.com/en/articles/2002516-available-variables-and-attributes-for-nosto-campaigns) inside of your template.
  - You will find a full reference and examples in the Nosto backend when you're building your template.

### Client: JS API: `createRecommendationRequest()`

In case you want more control about the campaign loading, you can disable autoloading and request the campaigns yourself. This approach is also suitable if you only want to request the product data in JSON per campaign and build the templates yourself.
This approach is **not suitable for SPAs or headless frontends**, please see `Session API: defaultSession()` below.

- The campaigns can return `HTML` (default, for templates hosted at Nosto) or `JSON`, depending on how you [build the request](https://nosto.github.io/nosto-js/interfaces/client.API.html#createrecommendationrequest).
- By default, the request does not know anything about the current page, placements on the current page, logged in customer, cart content etc. and you have two options of passing that data:
  - Including the HTML tagging with `{includeTagging: true}`
  - Setting the data manually via JS, e.g. `setPageType("product").setProducts([product_id: "4"])`
    - In most cases it will be sufficient to only include the page tagging since it reads the current product, cart content etc. and add a `.setPlacements(api.placements.getPlacements())` call.
    - Advanced cases where you need to explicitly set data occur when e.g. a variant has been selected on a PDP or if products should be filtered by a certain tag (e.g. for cannabis state-specific regulation or for vehicle-specific parts).
    - You can overwrite parts of the page tagging and filter products in a recommendation request (can include one or more placements) using [dynamic filtering](../../apis/js-apis/recommendations/setting-up-dynamic-filtering.md).
- Attribution is automatically handled by Nosto when using the default `response mode HTML`.
  - If you use the `JSON response mode`, you can [simplify DOM injection and click attribution](#dom-injection-and-click-attribution) with Nosto's helper methods.
- This approach is recommended for custom frontend builds, we recommend looking into the [Nosto Open Source packages](../../apis/frontend/oss/README.md).

### Client: Session API: `defaultSession()`

In case you are running a **SPA or headless frontend**, you want more control about the campaign loading and need to disable autoloading to request the campaigns yourself.

You can also find a [video for the Session API](https://partners.nostoacademy.com/catalog/courses/3870391) in the Nosto Partner Academy, just reach out to your Nosto contact if you don't have access yet. We also recommend to bookmark the [video to debug the Session API](https://partners.nostoacademy.com/catalog/courses/3870394).

- The campaigns return `JSON` by default, but in comparison to the JS API, you incorporate requesting campaigns with your page tagging/tracking via `defaultSession()` ([reference](https://nosto.github.io/nosto-js/interfaces/client.API.html#defaultsession)).
    - *Page tagging* refers to the JS API ([taggingProvider](https://nosto.github.io/nosto-js/interfaces/client.API.html#settaggingprovider)) and uses HTML and **MUST NOT** be mixed with the Session API.
    - *Page tracking* is similar to the page tagging but uses JavaScript calls to let Nosto know what is on the current page. For example:
    ```javascript
    nostojs(api => {
      api.defaultSession()
        .viewProduct("product-123")
        .setPlacements(api.placements.getPlacements())
        .load()
    });
    ```
- The page tracking sends the same [`ev1` request](https://nosto.github.io/nosto-js/interfaces/client.EventRequestMessageV1.html) which responds with the same campaign data and the same principles as the JS API.
- Since requesting campaigns is tied to the `defaultSession()` for page tracking, you can run a very similar code block on the different page types ([examples here](../../apis/frontend/implementation-guide-session-api/spa-basics-tracking-events.md) and in the [API reference](https://nosto.github.io/nosto-js/interfaces/client.Session.html)).
  - The methods like `viewFrontPage()` or `viewProduct("4")` are the main indicator that campaigns will be returned.
  - Adding `setPlacements(api.placements.getPlacements())` or passing the placement IDs explicitly as an array determines from where campaigns will be requested. We recommend the first approach, getting all campaigns for all placements instead of requesting them one by one.
  - Nosto provides you with ways to [easily inject campaigns into placements and sets up event listeners for click attribution](#dom-injection-and-click-attribution).
- Calling `load()` sends the request to Nosto, the returned Promise can be handled async or by chaining a `then()` to the request.
- Since there is no page tagging, you need to use the ["Visitor" tab in the Nosto Debug Toolbar](../checking-your-setup.md) and the [`ev1` request](https://nosto.github.io/nosto-js/interfaces/client.EventRequestMessageV1.html) in your network tab for verification and QA.
- There are several advanced cases to keep in mind and cover:
  - Using the category path that's in the Nosto backend (use the "Preview" feature and [Nosto Debug Toolbar](../checking-your-setup.md#view-category)), not the URL slug.
  - [Sending an additional event when a specific SKU has been selected (either on a PDP or via a "quick view" modal.](../../apis/frontend/implementation-guide-session-api/spa-basics-leveraging-features.md#handling-attribution)
  - You can filter products in a recommendation using [`viewCustomField()`](https://nosto.github.io/nosto-js/interfaces/client.Session.html#viewcustomfield) which is the equivalent to [dynamic filtering via the JS API](../../apis/js-apis/recommendations/setting-up-dynamic-filtering.md) (you MUST NOT mix these APIs).
  - If you are using the `setRef()` method ([reference](https://nosto.github.io/nosto-js/interfaces/client.Action.html#setref-1)), pay close attention - **the second parameter is the recommendation slot id** (`result_id` of the response), *not the placement div id*.
  - [Using `load()` only on the first request on the current page](../../apis/frontend/implementation-guide-session-api/spa-basics-leveraging-features.md#reporting-correct-page-views-load-vs-update) because it increments the page view counter (`pv` in the ["ev1" response](https://nosto.github.io/nosto-js/interfaces/client.EventResponseMessage.html#pv)). On subsequent requests on the same page you must send the request with `update()` or [pass a recommendation request flag like `.load{skipPageViews: true}`](https://nosto.github.io/nosto-js/interfaces/client.Action.html#load) ([details here](../../apis/frontend/implementation-guide-session-api/spa-basics-leveraging-features.md#reporting-correct-page-views-load-vs-update)).
- You can still `setResponseMode("HTML")` and request the Nosto-hosted templates if you're *not* running a SPA. The click attribution and template injection can be automated by calling `enableCampaignInjection()` ([example](../../implementing-nosto/implement-on-your-website/advanced-implementation/parameterless-attribution.md#session-api-based-usage)).

### Server: GraphQL API: `updateSession()`

In case you don't want follow one of the client-based approaches, you can manage the Nosto session and campaign rendering via GraphQL.

**Please beware of the following limitations:**
- You request the campaigns for a specific product ID or category (without placements) and will receive the Recommendation campaign IDs directly.
  - Therefore, you **can't use Nosto built-in A/B testing** for campaign widgets.
  - *You need an alternative, full page A/B testing like Omniconvert* in this case.
- **[Dynamic filtering](../../apis/js-apis/recommendations/setting-up-dynamic-filtering.md) is not possible via GraphQL.** We highly recommend to go with the Session API and use [`viewCustomField`](https://nosto.github.io/nosto-js/interfaces/client.Session.html#viewcustomfield)
- **Nosto OCP cannot be retrieved via GraphQL** (personalized banners or other HTML content).
- Adding explicit affinity signals manually like with the [JS API](https://nosto.github.io/nosto-js/interfaces/client.API.html#addaffinitysignals) is not supported. [Personalization via variant/SKU affinity](../../apis/graphql-an-introduction/graphql-using-mutations/graphql-onsite-sessions/README.md#tracking-product-variant-views) as well as [multi currency or customer group pricing](../../apis/graphql-an-introduction/graphql-using-mutations/graphql-onsite-sessions/README.md#customer-group-pricing-and-multi-currency) are supported.

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
    - The `ref` parameter must match the `resultId` (= Nosto recommendation campaign slot ID) from the response ([example response above](#how-it-works)).
  - Set the correct page type (`PageRequestEntity` in the API reference) under `pages` to specify the context from which you want to receive Nosto campaign data.
- Parse the [Nosto response](../../apis/graphql-an-introduction/graphql-using-mutations/graphql-onsite-sessions/README.md#attribution-of-recommendation-results) and render your template.
  - Make sure you save the `resultId` and pass it to your next `updateSession(params: { event: { type: VIEWED_PRODUCT } } )` as `ref` for attribution when a shopper clicks on one of the products inside of your template.
  - Since you don't use JavaScript, there is no event listener or helper function Nosto can provide. You will build the process yourself that is outlined in the [parameterless attribution documentation](../implement-on-your-website/advanced-implementation/parameterless-attribution.md) (detailled example below).


## DOM Injection and Click Attribution

Typical use case, options and adjustment depending on your implementation method:

* A shopper is on a PDP (e.g. product ID 42), sees a Nosto product recommendation and clicks on a shown product (ID 200). You don't explicitly react to the click, you instead make sure the click listener for parameterless attribution is firing. See the [parameterless attribution documentation](../implement-on-your-website/advanced-implementation/parameterless-attribution.md) for more details.
  * If you're using Nosto-hosted templates with autoloading, you don't need to do anything. Parameterless attribution is enabled and set up by default.
  * If you're manually requesting Nosto-hosted HTML templates via the JS API, you have two options:
    * Let Nosto inject the campaigns into the DOM via `injectCampaigns()`, this is recommended, attribution is set up automatically.
    * Inject the campaigns into the DOM yourself and set up attribution manually via [attributeProductClicksInCampaign](https://nosto.github.io/nosto-js/interfaces/client.API.html#attributeproductclicksincampaign) after rendering the campaign ([example here](../implement-on-your-website/advanced-implementation/parameterless-attribution.md#json-rendering-attribution)).
  * If you're manually requesting Nosto-hosted HTML templates via the Session API, we recommend to add `enableCampaignInjection()` to your `defaultSession()`. Nosto will automatically inject the campaigns into the DOM, parameterless attribution is enabled and set up by default.
  * If you're manually requesting only the product data via JSON from a Nosto campaign via the Session API, you have two options after you've built the HTML for your campaigns in your code base:
    * Let Nosto inject the campaigns into the DOM via `injectCampaigns()`, this is recommended, attribution is set up automatically.
    * Inject the campaigns into the DOM yourself and set up attribution manually via [attributeProductClicksInCampaign](https://nosto.github.io/nosto-js/interfaces/client.API.html#attributeproductclicksincampaign) after rendering the campaign.
* When a user then clicks on a product link within a Nosto recommendation (e.g. slot ID "productpage-nosto-2-fallback"), Nosto's event listener (previously set up via `enableCampaignInjection()` for HTML templates and `injectCampaigns()` or `attributeProductClicksInCampaign()` for JSON-based templates) captures and stores the current URL and reference of the `result_id` in the shopper's browser local storage.
* The browser then navigates to the product URL.
  * The Nosto client script reads the previous URL and reference from the local storage and sets it as `ref` parameter for the ev1 request, which results in the attribution of the current product view to the previously shown recommendation campaign.


## Comparison Table

| Feature                                            | Automatic Injection with Nosto Autoloading             | JS API: `createRecommendationRequest()`                                                                                                                                         | Session API: `defaultSession()`                                                                                                                | Nosto Content via GraphQL                                                                                   |
| :------------------------------------------------- | :----------------------------------------------------- | :-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------  | :--------------------------------------------------------------------------------------------------------------------------------------------- | :---------------------------------------------------------------------------------------------------------- |
| **Best For**                                       | Conventional builds                                    | Custom frontend builds (non-SPA) with e.g. customer group pricing/visibility (every platform) or highly complex product cards (non-Shopify)                                     | SPAs and Headless frontends                                                                                                                    | Mobile apps or server-side rendered builds (when Nosto A/B testing, dynamic filtering and OCP isn't needed) |
| **How it Works**                                   | Content is automatically injected into page templates. | Manually request campaigns after disabling autoloading.                                                                                                                         | Request campaigns as part of the page tracking/tagging flow.                                                                                   | Request campaigns as part of the page tracking/tagging flow.                                                |
| **Campaign Response Type**                         | HTML (for Nosto-hosted templates)                      | HTML (default) or JSON                                                                                                                                                          | JSON (default), but can be set to HTML                                                                                                         | Recommendation campaign slot IDs (no placements, **OCP campaigns are *not* available**)                               |
| **Campaign Injection and Click Attribution**       | Handled automatically by Nosto.                        | Automatic for HTML mode with `enableCampaignInjection()`. For JSON mode, use `injectCampaigns()` or inject campaigns yourself and add `api.attributeProductClicksInCampaign()`. | For JSON mode, use `injectCampaigns()` or inject campaigns yourself and add `api.attributeProductClicksInCampaign()`. Automatic for HTML mode. | Manual. Requires careful use of the `event` params in `updateSession()` mutation.                           |
| **SPA Suitable**                                   | No (triggers a full page load)                         | No (recommended for custom builds, but not SPAs)                                                                                                                                | Yes (designed for SPAs and Headless)                                                                                                           | Yes                                                                                                         |
| **Headless compatible**                            | No                                                     | No                                                                                                                                                                              | Yes                                                                                                                                            | Yes                                                                                                         |
| **Fully customizable frontend**                    | Yes                                                    | Yes                                                                                                                                                                             | Yes                                                                                                                                            | Yes                                                                                                         |
| **Suitable for complex use cases**                 | Sometimes                                              | Yes                                                                                                                                                                             | Yes                                                                                                                                            | Yes                                                                                                         |
| **Customized and managed only in Nosto dashboard** | Yes (templates are in Nosto backend)                   | Depends on (can build templates in Nosto or own code base, injection needs to be triggered manually via JS)                                                                     | Unlikely (you *can* build templates in Nosto but will likely build them in the own code base, injection needs to be triggered manually via JS) | No (can build templates in own code base via server-side rendering)                                         |
| **A/B Testing**                                    | Yes (via placements)                                   | Yes (via placements)                                                                                                                                                            | Yes (via placements)                                                                                                                           | No (Nosto built-in A/B testing is not available, needs full page testing like OmniConvert)                  |
| **Drawbacks**                                      | Not suitable for SPAs or Headless.                     | Not suitable for SPAs or Headless.                                                                                                                                              | Requires a bit more planning because page tracking and campaign injection are combined                                                         | **No Nosto A/B testing.**<br>**No Dynamic Filtering.**<br>**No OCP (HTML content/banners).**                |