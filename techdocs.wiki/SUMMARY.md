* Implement Nosto
  * On a regular site
    * [Manual Tagging - Essentials](Manual-implementation)
      * [Setting up your account](Setting-up-your-account)
      * [Adding the Nosto Script](Add-Nosto-script)
      * [Adding the Cart Tagging](Cart-Tagging)
      * [Adding the Customer information](Adding-the-customer-information)
      * [Adding the Product Tagging](Product-Tagging)
        * [Default Product Tagging](https://github.com/Nosto/techdocs/wiki/Basic:-Default-Product-Tagging)
        * [Basic Tagging](https://github.com/Nosto/techdocs/wiki/Basic:-Minimum-Product-Tagging)
      * [Adding the Category/Brand Tagging](Category-&-Brand-tagging)
      * [Adding the Search Tagging](Search-Tagging)
      * [Adding the Order Tagging](Order-Tagging)
      * [Defining Nosto placements](Defining-Nosto-placements)
      * [Tagging your page types](Tag-your-page-types)
    * [Advanced Usage](Advanced-implementation)
      * [Extending tagging with SKUs](Extending-tagging-with-SKUs)
      * [Adding support for multi-currency](Adding-support-for-multi-currency)
      * [Adding support for customer group pricing](Adding-support-for-customer-group-pricing)
    * [FAQ](https://github.com/Nosto/techdocs/wiki/Basic:-FAQ)
  * [On a Single-Page App (Session API)](https://github.com/Nosto/techdocs/wiki/SPA:-Implementation-Guide-(Session-API))
      * [Terminology](https://github.com/Nosto/techdocs/wiki/Session-API---Terminology)
      * [Setting up your account](https://github.com/Nosto/techdocs/wiki/SPA:-Basics#Setting-up-your-account)
      * [Setting up the catalog sync](https://github.com/Nosto/techdocs/wiki/SPA:-Basics#setting-up-the-catalog-sync)
      * [Adding the Nosto Script](https://github.com/Nosto/techdocs/wiki/SPA:-Basics#Add-Nosto-script)
      * Managing Sessions
        * [Setting the cart
](https://github.com/Nosto/techdocs/wiki/SPA:-Basics#setting-the-cart)
        * [Setting the customer
](https://github.com/Nosto/techdocs/wiki/SPA:-Basics#setting-the-customer)
       * Tracking Events
         * [Upon viewing the homepage](https://github.com/Nosto/techdocs/wiki/SPA:-Basics#upon-viewing-the-homepage) 
         * [Upon viewing a product
](https://github.com/Nosto/techdocs/wiki/SPA:-Basics#upon-viewing-a-product)
         * [Upon viewing a collection
](https://github.com/Nosto/techdocs/wiki/SPA:-Basics#upon-viewing-a-collection)
         * [Upon doing a search
](https://github.com/Nosto/techdocs/wiki/SPA:-Basics#upon-doing-a-search)
         * [Upon starting a checkout
](https://github.com/Nosto/techdocs/wiki/SPA:-Basics#upon-starting-a-checkout)
         * [Upon placing an order](https://github.com/Nosto/techdocs/wiki/SPA:-Basics#upon-placing-an-order)
         * [Upon a not found page](https://github.com/Nosto/techdocs/wiki/SPA:-Basics#upon-viewing-a-page-that-was-not-found-404)
    * Leveraging Features
      * [Working with recommendations](https://github.com/Nosto/techdocs/wiki/SPA:-Basics#working-with-recommendations)
      * [Working with content](https://github.com/Nosto/techdocs/wiki/SPA:-Basics#working-with-content)
      * [Working with popups](https://github.com/Nosto/techdocs/wiki/SPA:-Basics#working-with-popups)
    * Advanced Usage
      * Extending tagging with SKUs
      * [Adding support for multi-currency](https://github.com/Nosto/techdocs/wiki/SPA:-Adding-support-for-multi-currency)
      * [Adding support for customer group pricing](https://github.com/Nosto/techdocs/wiki/SPA:-Adding-support-for-customer-group-pricing)
    * [FAQ](https://github.com/Nosto/techdocs/wiki/SPA:-FAQ)
* [3rd party data integrations](3rd-party-data-integrations)
  * [Tracking Nosto with Google Analytics Enhanced Ecommerce](Tracking-Nosto-with-Google-Analytics)
* [Physical Retail](Physical-Retail)
  * [Sending Offline Orders](Sending-Offline-Orders)
* [Native Mobile](Native-Mobile)
  * [Implementing on iOS and Android](Implementing-on-iOS-and-Android)
* [JS API](JS-APIs)
  * [Initializing Nosto](Initializing-Nosto)
  * [Recommendations](Recommendations)
    * [Loading Recommendations](Loading-Recommendations)
    * [Recommendation Callbacks](Recommendation-Callbacks)
    * [Setting up dynamic filtering](Setting-up-dynamic-filtering)
  * [Popups](Popups)
    * [Listing Popup Campaigns](Listing-Popup-Campaigns)
    * [Opening a Popup](Opening-a-Popup)
    * [Enabling & Disabling Popups](Enabling-&-Disabling-Popups)
    * [Popup Callbacks](Popup-Callbacks)
  * Common Examples
    * [Sending Product-View Events](Sending-Product-View-Events)
    * [Sending Add-To-Cart Events](Sending-Add-To-Cart-Events)
    * [Sending Customer Information](Sending-customer-information)
    * [Sending email addresses to Nosto](Sending-email-addresses-to-Nosto)
    * [Manually segmenting users](Manually-Segmenting-Users)
* [APIs](APIs)
  * GDPR
    * [Redacting customer data](Sanitizing-customer-data-using-the-Redaction-API)
    * [Initiating data takeouts](Initiating-data-takeouts-via-the-Takeout-APIs)
  * Customers
    * [Blacklisting Customers](Blacklisting-customers-using-the-Blacklist-API)
    * [Toggling marketing consent](Toggling-email-opt-in-using-the-Consent-API)
  * Products
    * [Updating Products](Updating-products-using-the-Products-API)
    * [Discontinuing Products](Discontinuing-Products)
    * [Recrawling Products](Recrawling-products-using-the-Recrawl-API)
  * Other
    * [Updating Rates](Updating-Rates-using-the-Rates-API)
* [GraphQL](https://github.com/Nosto/docs-nosto-com/wiki/GraphQL:-An-Introduction)
    * [The Playground](https://github.com/Nosto/docs-nosto-com/wiki/GraphQL:-The-Playground)
    * [Using the API](https://github.com/Nosto/docs-nosto-com/wiki/GraphQL:-Using-the-API)
    * [Testing and Debugging](https://github.com/Nosto/docs-nosto-com/wiki/GraphQL:-Testing-&-Debugging)
    * [Using Mutations](https://github.com/Nosto/docs-nosto-com/wiki/GraphQL:-Using-Mutations)
        * [Updating Products](https://github.com/Nosto/docs-nosto-com/wiki/GraphQL:-Updating-Products)
        * [Updating Identities](https://github.com/Nosto/docs-nosto-com/wiki/GraphQL:-Updating-Identities)
        * [GraphQL: Onsite Sessions
](https://github.com/Nosto/docs-nosto-com/wiki/GraphQL:-Onsite-Sessions)
        * Working with Orders
          * [GraphQL: Placing Orders
](https://github.com/Nosto/docs-nosto-com/wiki/GraphQL:-Placing-Orders)
          * [GraphQL: Updating Order Statuses
](https://github.com/Nosto/techdocs/wiki/GraphQL:-Updating-Order-Statuses)
    * [Using Queries](https://github.com/Nosto/docs-nosto-com/wiki/GraphQL:-Using-Queries)
        * [Querying Products](https://github.com/Nosto/docs-nosto-com/wiki/GraphQL:-Querying-Products)
        * [Querying Identities](https://github.com/Nosto/docs-nosto-com/wiki/GraphQL:-Querying-Identities)
        * [Querying Orders](https://github.com/Nosto/docs-nosto-com/wiki/GraphQL:-Querying-Orders)
        * [Querying Recommendations](https://github.com/Nosto/docs-nosto-com/wiki/GraphQL:-Querying-Recommendations)
        * [Querying Segments](https://github.com/Nosto/techdocs/wiki/GraphQL:-Querying-Segments)
        * [Querying Category Merchandising Products](https://github.com/Nosto/techdocs/wiki/GraphQL:-Querying-Category-Merchandising-Products)
    * [For iOS & Android](https://github.com/Nosto/docs-nosto-com/wiki/GraphQL:-For-iOS-&-Android)
    * [For Headless](https://github.com/Nosto/docs-nosto-com/wiki/GraphQL:-For-Headless)
    * [FAQ](https://github.com/Nosto/docs-nosto-com/wiki/GraphQL:-FAQ)