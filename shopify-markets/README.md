# Shopify Markets

## Overview <a href="#user-content-overview" id="user-content-overview"></a>

Nosto supports Shopify Markets for selling across countries and regions, so each of your Markets gets the right Nosto experience: tailored to one Market, shared across several, or a mix of both. Language, currency and product availability follow your Shopify setup, and new Submarkets are picked up automatically.

Prices show in each Market's currency, using the exchange rates and rounding rules you set in Shopify. If you set prices per Market, for example through Managed Markets, a small template adjustment keeps the displayed prices right.&#x20;

### How Nosto maps your Markets <a href="#user-content-how-nosto-maps-your-markets" id="user-content-how-nosto-maps-your-markets"></a>

* Master Account: created automatically when you install Nosto. It represents your main Market and language, and serves every Market that isn't linked elsewhere.
* Market Accounts: additional Nosto accounts for Markets or languages that need their own catalog, pricing, language or campaigns.
* Linked Markets: Markets without a Nosto account of their own. They use the setup of the Market Account they're linked to.

## Preparation <a href="#user-content-preparation" id="user-content-preparation"></a>

Before you begin setting up your Shopify Markets personalization with Nosto, please have a chat with your Nosto contact person. The integration has been designed to flexibly adjust to your needs, so planning this ahead of time allows integrating in a way that is tailored to you.

For your planning, please think about the following questions:

Languages

* How many languages do you support across your Markets? Write them down, as every language gets its own Nosto account for easier campaign management.

Market Availability

* Do you have Markets with different product sets? Identify them beforehand, so that these exceptions can be respected.

Dedicated Targeting

* Do you have Key-Markets that are handled by different teams, have specific needs in merchandising or require dedicated strategy? Let us know, and we will make it happen!

### Implementation Process <a href="#user-content-implementation-process" id="user-content-implementation-process"></a>

Once you shared the above list with your TSM or CSM, the team will set up your accounts for you. Once done, you can find everything in your Nosto account under: Settings → Integrations → Shopify "Manage" → Shopify Markets.

Here you can:

Link and unlink Markets

* All your Markets will be linked to your Nosto accounts for you - but of course you have full flexibility, in case your strategy changes down the line. You will be able to change the linking at any point, or ask the Nosto team to do it for you. New Submarkets are linked automatically, see Automatic linking of Submarkets below.

Product Syncing Process

* Click the "Full Product Sync" button within Nosto to start synchronizing products for all your Markets and languages, including exchange rates. Expect this to take up to two hours.

General Setup

* Set up your campaigns, templates, rulesets, and other details within your Market Accounts to precisely target individual Markets. Linked Markets use the campaigns of the Market Account they're linked to automatically. New Submarkets are linked to their parent's account automatically where possible. Unlinked Markets fall back to your Master Account.
* If you don't wish to tailor campaigns for each Market Account specifically, you can reach out to the Nosto team, who can copy the configurations to your accounts.

## Automatic linking of Submarkets <a href="#user-content-automatic-linking-of-submarkets" id="user-content-automatic-linking-of-submarkets"></a>

Shopify lets you nest Markets inside each other. A Baltics Market can contain a Latvia Submarket, for example. Shopify can also create Submarkets on its own, for instance when your store is upgraded to shipping options by market and your shipping zones are more detailed than your Markets.

Nosto keeps these Submarkets on the experience you already set up. Whenever a Market is created, updated or deleted in Shopify, Nosto checks your Market structure and links a new Submarket to the Nosto account that already serves its parent Market in the same language. If the parent Market is itself linked to another account, the Submarket follows it there.

Example: your Baltics Market is linked to your Europe account. When a Latvia Submarket appears under Baltics, Nosto links Latvia to your Europe account as well, so shoppers in Latvia get the same Nosto experience as the rest of Baltics.

A link doesn't create a new Nosto account or a new product feed. The Submarket uses the account it's linked to, including its catalog and campaigns.

Nosto links a Submarket automatically when the right account is clear:

* The Submarket is active in Shopify.
* Its country belongs to the parent Market.
* The parent Market, or the account it's linked to, has a Nosto account in the same language.
* Exactly one account fits.

If one of these is missing, Nosto leaves the Submarket unlinked, and your shoppers see your Master Account's experience, like any other unlinked Market. You can still link it manually at any time, or ask your Nosto contact to do it for you.

Good to know:

* A new link can take a few minutes to appear after Shopify creates a Submarket.
* Removing an automatic link by hand isn't permanent. A later update from Shopify can link the Submarket again, as long as it still meets the rules above.
* A Submarket that needs its own catalog or campaigns can still get its own Nosto account. Your Nosto contact sets it up with you.
* Deleting a Submarket in Shopify removes only its link. Your other accounts, links and product feeds stay as they are.

## Market Logs <a href="#user-content-market-logs" id="user-content-market-logs"></a>

For more visibility, we log your Markets events received from Shopify, as well as what they triggered in Nosto. In your Shopify Settings area in Nosto, you find every Market change Nosto received from Shopify and every automatic link it made, each with a timestamp. For example:

* "Received new market creation for France (ID: 67890)."
* "Market France (ID: 67890-fr) was linked to parent Market Europe (ID: 12345)."

## Fulfillable Inventory <a href="#user-content-fulfillable-inventory" id="user-content-fulfillable-inventory"></a>

Nosto supports Fulfillable Inventory for international sales channels. To activate, please reach out to us directly, and share the IDs of your inventory locations per Market. You can find these IDs when navigating to your location in Shopify - the ID will be at the end of your URL in the browser. The Nosto team will enable the functionality for you, and assign the locations to your Market Accounts. After this, a full Product Sync will be started. Please note that the initial sync will take more time than usual, with around 250 products per hour (separately for your Markets). The process will speed up while it's running, and every sync afterwards will be within normal product sync expectations.

## Managed Markets & Global-E <a href="#user-content-global-e--managed-markets" id="user-content-global-e--managed-markets"></a>

In some cases you might encounter incorrect prices showing up in the Front End (e.g. in Recommendations). This can happen when Nosto grabs exchange rates for multiple currencies from Shopify, but in fact prices are set through in Managed Markets or Global-E. In that case it can be needed to add an additional functionality to your templates in order to ensure prices in the Front End are always showing correctly. Please check [here](technical-details-how-to-use-it.md), or reach out to your Nosto contact and ask for help.

## Technical Details <a href="#user-content-technical-details" id="user-content-technical-details"></a>

* Product Sync: Once Markets are synced, Nosto uses Shopify's webhooks & GraphQL endpoints to sync product data specifically tailored for each of your Markets and languages set in Shopify. Your product data will appear in your respective Market Accounts swiftly.
* Product Updates: Whenever you update your products or add new ones in Shopify, the changes are sent to Nosto via Shopify's webhooks & GraphQL endpoints, allowing us to update your product catalog accordingly.
* Handling Currencies: Every account created in Nosto will contain product prices in the respective Market's currency, and on top, respect exchange rates and rounding rules as set in Shopify.
* Managing Orders: After a customer places an order in your store, Nosto loads the appropriate Market script. It assigns the order and attribution to the correct account in the Nosto backend (based on the Market & language a customer has set). Orders from a linked Submarket are assigned to the account it's linked to. This process ensures accurate analytics and behavioral data in Nosto.
* Managing Themes: Activating a Market in Nosto will load the appropriate script depending on the user's location or language. Because all Markets in Shopify use the same theme, they'll automatically display if Nosto is added to the main theme through your Master Account. However, the content displayed is dependent on the configurations you've established within Nosto for each specific Market.
* Campaign Rendering: Similar to other Shopify accounts in Nosto, you have the ability to render Nosto campaigns through various placements. These can be either static (for example, through Shopify Sections) or dynamic. Consequently, Nosto will always display the campaign linked to a specific placement in the account associated with the Market/Language the customer is browsing. In the absence of a designated campaign, Nosto will not display anything. If a Market Account is inactive, Nosto will revert to the configurations of your Master Account.

## Considerations & Data Preparation <a href="#user-content-considerations--data-preparation" id="user-content-considerations--data-preparation"></a>

There are some limitations to the data that Nosto can receive:

* In Shopify, it's not possible to translate Tags or ProductType; therefore, Tags will only appear in your main market language in Nosto.
* For localizing data points that can be used within Nosto, we highly suggest making these available as metafields in Shopify. Usage examples include Filters on Search- & Category pages and badges in Product Recommendations, among others.
* To allow Nosto to pull your metafields, those need to be accessible through the storefront. See also: [Shopify documentation](https://help.shopify.com/en/manual/custom-data/options)
* The integration is only compatible with Category Merchandising (Universal).
