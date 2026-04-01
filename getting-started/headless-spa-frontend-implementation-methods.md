# Headless and SPA Frontend: Implementation Methods
The following gives a quick overview of page tagging/event tracking (coupled with handling Nosto product recommendations and banners) for headless and SPA (Single Page Application) builds. You can find more details in the [personalization implementation guide](../implementing-nosto/implement-psn/README.md), but this page will already give you a general understanding of the concept.

Search and Category Merchandising is separate from the personalization guide and covered at the end of this page.

{% hint style="info" %}
If you are using Shopify Hydrogen or Magento Hyvä, you can use Nosto's dedicated [React Component Library for Shopify Hydrogen](https://docs.nosto.com/shopify/features/shopify-hydrogen) and the [built-in support for Hyvä in the Nosto Magento plugin](https://docs.nosto.com/magento-2/hyva-theme).
{% endhint %}


## Page Tagging and Event Tracking + Requesting Nosto Content for rendering with your Templates
On every page visit, you need to send a request to Nosto using our [Session API](https://nosto.github.io/nosto-js/interfaces/client.Session.html) about the page type the user is browsing and what exactly they're looking at (e.g. type = product, ID = 123).

You can [find the different page types here](../apis/frontend/implementation-guide-session-api/spa-basics-tracking-events.md), the concept is the same every time.

Nosto then returns a response with two types of content:
- Product Recommendations -> [JSONResult](https://nosto.github.io/nosto-js/interfaces/client.JSONResult.html) with an array of [JSONProduct](https://nosto.github.io/nosto-js/interfaces/client.JSONProduct.html)
- Onsite Content Personalization (OCP, e.g. banners or text) -> [HTML](https://nosto.github.io/nosto-js/interfaces/client.AttributedCampaignResult.html)

### Nosto Content via Session API
You take the response and pass it to your rendering function, building the HTML template and injecting it into your theme.

This is done via ["placements"](https://help.nosto.com/en/articles/1883767-placements-general-article) (empty divs on every page that can be populated from the backend, e.g. pdp-top, pdp-mid, home-1, home-2, ...) and you pass all the placement-IDs that are on the current page to Nosto. Nosto then returns the data of the campaigns that are inside of those placements.

Using placements gives the eCom-team a high degree of flexibility since they can control what to show where and they can run A/B tests within Nosto.

Nosto offers you several helper functions to simplify injecting your campaigns and setting up click attribution. If you want to read more on DOM injection and click attribution [read this](../implementing-nosto/implement-psn/README.md#dom-injection-and-click-attribution).

### Nosto Content via GraphQL
The event tracking can also be done via GraphQL.

The concept is the same: [specify data about the session (cart and customer)](../apis/graphql-an-introduction/graphql-using-mutations/README.md) and [request product recommendations for a given page type](../apis/graphql-an-introduction/graphql-using-mutations/graphql-onsite-sessions/README.md).

**Please beware of the following drawbacks:**
1. You request the campaigns for a specific product ID or category (without placements) and will receive the Recommendation campaign IDs directly and therefore can't use Nosto built-in A/B testing. You need an alternative, full page A/B testing like Omniconvert in this case.
2. [Dynamic filtering](../apis/frontend/js-apis/recommendations/setting-up-dynamic-filtering.md) is not possible via GraphQL. We highly recommend to go with the Session API and use [`viewCustomField`](https://nosto.github.io/nosto-js/interfaces/client.Session.html#viewcustomfield).
3. Nosto OCP (like personalized banners or other HTML content) can not be retrieved via GraphQL.
4. Adding explicit affinity signals manually like with the [JS API](https://nosto.github.io/nosto-js/interfaces/client.API.html#addaffinitysignals) is not supported. [Personalization via variant/SKU affinity](../../apis/graphql-an-introduction/graphql-using-mutations/graphql-onsite-sessions/README.md#tracking-product-variant-views) as well as [multi currency or customer group pricing](../../apis/graphql-an-introduction/graphql-using-mutations/graphql-onsite-sessions/README.md#customer-group-pricing-and-multi-currency) are supported.

### Choosing the right Implementation Method
The Nosto team is happy to support you finding the method that matches your tech stack, requirements and preferences. We highly recommend reading our [personalization implementation guide](../implementing-nosto/implement-psn/README.md), but if you're in a hurry, take a look at our [comparison table](../implementing-nosto/implement-psn/README.md#comparison-table).


## Implementation Methods for Nosto Search/Category Merchandising (CM)
Here you can find an [overview of all implementation methods](../implementing-nosto/implement-search/README.md#compare-implementations).

The differences between the GraphQL API and JS Library (wrapping the GraphQL API) are:

- Queries done with the JS Library are automatically tracked, [only clicks need to be tracked](../implementing-nosto/implement-search/search/README.md#search-product-click) (when a user clicks on a product that was returned by a Nosto-powered search overlay, SERP or PLP)
- Nosto A/B testing is automatically included with the JS Library and [needs to be handled explicitly when using GraphQL](../implementing-nosto/implement-search/implement-search-using-api/analytics-ab-testing.md) 
- [Personalized and segmented results with GraphQL need an addition of the JS Library](../implementing-nosto/implement-search/implement-search-using-api/using-the-search-api.md#session-params) to get the current session params from the browser and pass it to the GraphQL request

The endpoints and requests are very similar, you either pass a search query or a category and Nosto returns all products and associated facets. Here are [several examples for pagination, sorting, faceting etc.](../implementing-nosto/implement-search/implement-search-using-api/implementing-search-page.md).
