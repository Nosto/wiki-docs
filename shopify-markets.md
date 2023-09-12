# Shopify Markets

## **Overview**

Nosto fully supports the complete functionality of Shopify Markets, allowing you to precisely target specific markets individually, unify the experience across all markets, or even employ a combination of both approaches. All of this is accomplished while ensuring language, pricing, currency, and product availability are respected.

## Before you start

Before you begin setting up your Shopify Markets personalization with Nosto follow these initial steps:&#x20;

{% hint style="warning" %}
If you already have Nosto installed, please upgrade the app to grant needed Markets permissions through your Shopify admin portal.
{% endhint %}

1. **Get Access to Shopify Markets through Nosto Team:** Reach out to your Nosto contact person or to [Nosto Support](mailto:support@nosto.com). Please give them your nosto account ID, and wait for confirmation that Shopify Markets has been enabled for you.
2. **Enable Shopify Markets** by pressing the similarly named button in your Nosto account (via Settings -> Integrations -> Shopify "Manage"). This step might take a couple of minutes.&#x20;

{% hint style="info" %}
In order to allow Nosto to pull Market information and products, a Market must be activated in Shopify. It doesn't have to be visible though, so you can consider to activate but hide the Market in order to bring information to Nosto.&#x20;
{% endhint %}

## Implementation Process

Once you completed all steps outlined in our "before you start" section, you can continue by following these step-by-step instructions, starting in Nosto via Settings -> Integrations -> Shopify "Manage":

**Product Syncing Process:**

* Click the "_Full Product Sync_" button within Nosto to start synchronizing products for all your Markets and languages, including exchange rates. Expect this to take up to two hours.

**General Setup:**

* **For New Clients or Tailored Markets:** Set up your campaigns, templates, rulesets, and other details within your Nosto Market accounts to precisely target individual Markets.
* **For Existing Clients:** If you don't wish to tailor campaigns for each Market, you can reach out to the Nosto team, who can copy the configurations to your Market accounts.

{% hint style="info" %}
If you have used our v1 Integration of Shopify Markets, please reach out to your main contact person, as there potentially is need for some adjustment in your templates.
{% endhint %}

**Verification:**&#x20;

* Utilize the "[Debug Toolbar](https://help.nosto.com/en/articles/1441625-how-to-use-the-nosto-debug-toolbar)" or the Preview-link in your Markets overview to ensure that everything is correctly configured.
* Verify exchange rates and translations.
* Make final adjustments as needed before launching Nosto for your Market.

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
* Products included/excluded to a specific Market after initial sync do not get synced automatically. When a product gets excluded from a specific Market, please trigger a product update from your Nosto dashboard.

## Deactivating Shopify Markets

If you choose not to target a specific Market, or if you encounter any issues, you have the option to deactivate a Market account in Nosto. You can do this from the overview within your main account by navigating to Settings -> Integrations, and then clicking on "Manage" next to the Shopify logo. Doing so will halt the loading of the Market-specific script, and instead, the script for your main account will be loaded.

{% hint style="info" %}
Be aware, that instead your main account will load in your store, which will fully rely on pricing & language information of your main market.
{% endhint %}
