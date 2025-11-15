# Building your Implementation Plan

Nosto provides plugins for the most common eCommerce platforms like Shopify, Magento 2, Shopware 6, BigCommerce and Prestashop.

- The plugins have implemented the Nosto backend APIs so in most cases 90% of the product catalog sync is already handled.
  - If your client has custom use cases, you likely will have to extend the Nosto plugin, change the data structure within your platform or make use of one of the Nosto backend APIs to ensure all the needed product data is in Nosto.
- Depending on your platform, a Nosto plugin (like for Shopware or Magento 2) extends the default theme (like Dawn, Storefront, Luma or Hyvä) and implements the frontend mechanisms that are explained in the following.
  - Some platform plugins only include a very basic frontend component like adding the Nosto script and the page tagging.
  - If your client has a custom frontend (running headless or a SPA*), you will need to implement the frontend mechanisms one by one.

*This guide focuses on classic web applications that work with full page reloads. For SPAs/PWAs like React, the same principles apply but they are handled differently.
We encourage you to first read through the guiding principles and in a next step make yourself familiar with the implementation within a Single Page Application.


## Components of a stable Nosto Implementation

1. Sending/updating the Product Catalog via GraphQL API (or REST API) to Nosto regularly.
   - For custom platforms (without Nosto plugin), you must use one of the APIs and build the product sync yourself (no CSV or feed option). If you’re working with PHP, you can use the Nosto PHP-SDK.
2. Page tagging via JavaScript or hidden HTML on all pages
   - There are several page types, so please make sure they are covered in your QA checklist. (You can use ours as a blueprint.)
   - The current cart content and logged in user information need to be available on all pages.
3. Placements (empty divs) where Nosto content can be injected.
   - These placements should be added into the HTML, most Nosto plugins offer CMS blocks for easier use.
4. Injecting personalized content into the placements of the current page (banners, product recommendation carousels/grids, ...)
   - This involves a simple request/response pattern: Tell Nosto which placements are on the current page and you’ll receive the content as JSON or HTML.
5. Replacing native platform features like product listings (PLPs and SERPs) with facets, pagination and sorting options.
6. Attribution and recording of certain events when a user interacts with a Nosto module, for example:
   - Impressions: Products returned from a search query
   -  Clicks/Showing interest in a product by clicking "view more" on a PLP or SERP, from the search overlay or from a product recommendations carousel:
      - Can be a redirect to a PDP or
      - When a popup/modal (quick view) is triggered
7. A client that defines merchandising rules, fine-tunes the search functionality, creates recommendation campaigns, A/B tests etc. and does not need to interact with your code.


## Resources to evaluate and choose the Implementation Methods

Nosto provides [plugins for the most common eCommerce platforms](https://docs.nosto.com/techdocs/implementing-nosto/implement-on-your-website#choosing-the-implementation-method) like Shopify, Magento 2, Shopware 6, BigCommerce and Prestashop.

We recommend using our plugins as a base since they follow platform- and Nosto-specific best practices, save you lots of development time and have been tested in hundreds of eCommerce sites.

### Nosto Module Types and Frontend Implementation Methods

Every Nosto module falls into one of two module types which have their own set of possible implementation methods:

1. **Campaign Widgets:** Product Recommendations, Dynamic Bundles and Onsite Content Personalization (via placements, see previous section, points 3 and 4)
2. **Listings:** Search and Category Merchandising (replacing native content (Autocomplete/Search Preview, SERPs, PLPs), see previous section, point 5)

### Project Plan Checklist

All of the following questions can be answered with our general and platform-specific documentation. For custom builds and advanced implementations you find additional playbooks below. Our onboarding team is happy to assist you with the evaluation and planning.

- [ ] **Product Data Sync:** How do you send Nosto the product catalog?
- [ ] **Multi-Currency and Localization:** How do you send Nosto the localized data? How do you make sure the correct language and currency shows in the frontend?
- [ ] **Order Data Sync:** How do you send Nosto the placed orders?
- [ ] **Frontend Script:** How do you inject the Nosto script?
- [ ] **Session Tagging:** How do you tag the currently logged in user and shopping cart content on all pages?
- [ ] **Frontend Page Tagging:** How do you tag the different page types?
- [ ] **Frontend Module Injection:** Which implementation method fits your tech stack and client needs best? (Comparison table below)
  - [ ] Campaign Widgets: `Nosto-hosted` | `Client API` | `Server API`
  - [ ] Listings: `Nosto-hosted` | `Client API` | `Server API`
- [ ] **Custom Event Tracking:** How do you signal Nosto non-standard user interactions like "viewed product" or "added product to cart" events? You need this if you have features like:
  - [ ] "Add to cart" button on product cards, 
  - [ ] "Quick View"/"Quick Buy" modals in campaign widgets or listings,
  - [ ] "Mini-Cart" campaign widgets, 
  - [ ] ...
- [ ] **Testing and QA**: How do you verify:
  - [ ] Nosto receives the correct product and order data from your platform,
  - [ ] Nosto data (product recommendations and/or product listings) is personalized, segmented and rendered in the frontend,
  - [ ] Nosto-relevant user interactions are tracked by Nosto,
  - [ ] Nosto-influenced user interactions are correctly attributed to orders,
  - [ ] Nosto A/B testing functionality can be utilized for each Nosto module.
  - [ ] Nosto modules don't interfere with your architecture (e.g. on a SPA, Nosto doesn't cause page reloads)
  - [ ] Nosto Category Merchandising only: PLPs show the expected products, either:
    - [ ] Only directly assigned products to a category or 
    - [ ] Including the products of subcategories (Magento e.g. calls this an "anchored category")

### Headless Builds, SPAs (Single Page Applications) and Custom Platforms

We have prepared dedicated playbooks for advanced setups. 

In these cases you might be able to use parts of the Nosto plugins (e.g. for a Shopify headless frontend you can use our app for the data sync but need to implement the frontend components yourself).

We're happy to help you find the ideal approach for your particular tech stack. Please read the matching playbook prior to our conversation:

- Headless Frontend: [Implementation Methods](headless-frontend-implementation-methods.md)
- SPA: [Implementation Methods](spa-implementation-methods.md)
- Custom Platform/Use Case: Product Data Sync [GraphQL](../apis/graphql-an-introduction/graphql-using-mutations/graphql-updating-products.md) | [REST](../apis/rest/products/updating-products-using-the-products-api.md)
- Custom Platform/Use Case: Order Data Sync [GraphQL](../apis/graphql-an-introduction/graphql-using-mutations/working-with-orders/graphql-placing-orders.md)

### Comparison Tables for Nosto Modules and Implementation Methods

Depending on which Nosto modules your client has purchased, you need to determine how to implement each of those types. You can weigh the different methods and find the best approach for your tech stack, preferences and client needs with the resources below.

1. **Campaign Widgets:** [Product Recommendations, Dynamic Bundles and Onsite Content Personalization](../implementing-nosto/implement-psn/README.md#comparison-table)
2. **Listings:** [Search and Category Merchandising](../implementing-nosto/implement-search/README.md#compare-implementations)