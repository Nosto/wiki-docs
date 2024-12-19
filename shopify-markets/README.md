# Shopify International

## **Overview**

Nosto fully supports the complete functionality of Shopify International (formerly called Shopify Markets), allowing you to precisely target specific markets individually, unify the experience across all markets, or even employ a combination of both approaches. All of this is accomplished while ensuring language, pricing, currency, and product availability are respected.

## Preperation

Before you begin setting up your Shopify Markets personalization with Nosto, Please have a chat with your Nosto contact person. The integration has been designed to flexibly adjust to your needs, so planning this ahead of time allows integrating in a way that is tailored to you.&#x20;

For your planning, please think about the following questions:

**Languages**

* How many languages do you support accross your Markets? Write them down, as every language will get it's own Instance for easier campaign management.&#x20;

**Market Availability**

* Do you have Markets with different product sets? Identify them beforehand, so that these exceptions can be respected.&#x20;

**Dedicated Targeting**

* Do you have Key-Markets that are handled by different teams, have specific needs in merchandising or require dedicated strategy? Let us know, and we will make it happen!

### Implementation Process

Once you shared the above list with your TSM or CSM, the team will set up your instance-strategy for you. Once done, you can find everything in your Nosto account under: Settings -> Integrations -> Shopify "Manage" -> Shopify Markets.&#x20;

Here you can:&#x20;

**Link and unlink Markets**

* All your Markets will be linked to your Nosto instances for you - but of course you have full flexibility, in case your strategy changes down the line. You will be able to change the linking at any point, or ask the Nosto team to do it for you.&#x20;

**Product Syncing Process:**

* Click the "_Full Product Sync_" button within Nosto to start synchronizing products for all your Markets and languages, including exchange rates. Expect this to take up to two hours.

**General Setup:**

* Set up your campaigns, templates, rulesets, and other details within your Nosto Market accounts to precisely target individual Markets. All linked Markets will utilise the Main Accounts campaigns automatically\
  \--> Unlinked Markets will fall back to your Master Account&#x20;
* If you don't wish to tailor campaigns for each Main Account specifically, you can reach out to the Nosto team, who can copy the configurations to your accounts.

## Fulfillable Inventory

Nosto supports Fulfillable Inventory for international sales channels. To activate, please reach out to us directly, and share the IDs of your inventory locations per Market. There IDs you can find when navigating to your location in Shopify - the ID will be at the end of your URL in the browser. \
\
The Nosto team will enable the functionality for you, and assign the locations to you Market accounts. After this, a full Product Sync will be started. Please note that the initial sync will take more time than usual, with around 250 products per hour (seperately for your Markets). The process will speed up while it's running, and every sync afterwards will be within normal product sync expectations.&#x20;

## Global-E & Markets Pro

In some cases you might encounter incorrect prices showing up in the Front End (e.g. in Recommendations). This can happen when Nosto grabs exchange rates for multiple currencies from Shopify, but in fact prices are set through Global-E or in Markets Pro). In that case it can be needed to add an additional functionality to your templates. in order to ensure prices in the FE are always showing correctly. Please check here, or reach out to you Nosto contact and ask for help.&#x20;

## Technical Details

* **Product Sync:** Once Markets are synced, Nosto uses Shopify's webhooks & GraphQL endpoints to sync product data specifically tailored for each of your Markets and languages set in Shopify. Your product data will appear in your respective Market accounts swiftly.
* **Product Updates:** Whenever you update your products or add new ones in Shopify, the changes are sent to Nosto via Shopify's webhooks & GraphQL endpoints, allowing us to update your product catalog accordingly.
* **Handling Currencies:** Every account created in Nosto will contain product prices in the respective Markets currency, and on top, respect exchange rates and rounding rules as set in Shopify.
* **Managing Orders:** After a customer places an order in your store, Nosto loads the appropriate Market script. It assigns the order and attribution to the correct instance in the Nosto backend (based on Market & language a customer has set). This process ensures accurate analytics and behavioral data in Nosto.
* **Managing Themes:** Activating a Market in Nosto will load the appropriate script depending on the user's location or language. Because all Markets in Shopify use the same theme, they'll automatically display if Nosto is added to the main theme through your main Nosto account. However, the content displayed is dependent on the configurations you've established within Nosto for each specific Market.
* **Campaign Rendering:** Similar to other Shopify accounts in Nosto, you have the ability to render Nosto campaigns through various placements. These can be either static (for example, through Shopify Sections) or dynamic. Consequently, Nosto will always display the campaign linked to a specific placement in the account associated with the Market/Language the customer is browsing. In the absence of a designated campaign, Nosto will not display anything. If a Market account is inactive, Nosto will revert to the configurations of your main account.

## Considerations & Data Preparation

There are some limitations to the data that Nosto can receive

* In Shopify, it's not possible to translate Tags or ProductType; therefore, Tags will only appear in your main market language in Nosto
* For localizing data points that can be used within Nosto, we highly suggest making these available as metafields in Shopify. \
  Usage examples include Filters on Search- & Category pages and badges in Product Recommendations, among others.
* To allow Nosto to pull your metafields, those need to be accessible through the storefront. See also: [Shopify documentation](https://help.shopify.com/en/manual/custom-data/access-options)
* The integration is only compatible with the new version of Category Merchandising (through [code editor](https://docs.nosto.com/techdocs/implementing-nosto/implement-search/implement-search-using-code-editor/implementing-category-pages) or [API](https://docs.nosto.com/techdocs/implementing-nosto/implement-search/implement-search-using-api/implementing-category-pages))

