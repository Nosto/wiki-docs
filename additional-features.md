# Additional Features

This documentation page provides detailed information on the additional features available through the plugin, including how these features interact with Shopware's capabilities and are implemented in Nosto state. Understanding these features will help you tailor your storefront to meet your specific business needs.

### Multi-Currency

#### Overview

Shopware allows the use of multiple currencies, with exchange rates defined manually under **Shopware 6 > Settings > Currencies**. Each sales channel can support multiple currencies, with one set as the default. When editing a product's price, the prices for all configured currencies are updated according to the set exchange rate.

#### Plugin Implementation

* The plugin exports only the price of the configured default currency.

### Customer Group Pricing

#### Overview

Shopware supports customer-specific pricing for its paid plans, accessible via **Shopware 6 > Extensions > Customer specific pricing**. The Advanced Pricing feature may be used for this purpose, although implementation details may vary based on local or demo shop configurations.

#### Plugin Implementation

* Utilizes Advanced Pricing partially, allowing Shopware to calculate the relevant price based on the current context. For full product synchronization, a context is built using account/plugin configurations and some default values (e.g., the default customer group). However, not all Shopware pricing rules are applied, and customer-specific pricing is not used.

### Special Prices

#### Overview

Special prices can be managed within Shopware's Advanced Pricing feature.

#### Plugin Implementation

* Timed prices are considered according to the synchronization timing, not the search timing.

### Rating / Review System

#### Overview

Shopware includes an integrated review system, found under **Shopware 6 > Products > Reviews**.

#### Plugin Implementation

* When activated within the plugin configuration, the review count and average are synchronized. Reviews from other providers are not supported unless they overwrite the default Shopware reviews.

### Customer Tagging

The plugin includes a `customerDataToNosto` configuration option for customization of customer tagging.

More Information can be found under the following pages:

[Marketing permission and GDPR compatibility](https://docs.nosto.com/shopware/features/marketing-permission-and-gdpr-compatibility)

[Overriding Customer Data](https://docs.nosto.com/shopware/guides/overriding-or-extending-functionalities/overriding-customer-data)

### Alternate Image URLs

Support for multiple image exports can be enabled within the plugin configuration.

### Cart Recommendation Template

The Plugin supports functionalities such as adding products (individually or in multiples) to the cart from recommendation templates, reloading the Nosto cart from the Shopware cart, and generating a "restore to cart" link for email widgets - the link is generated and added to tagging.&#x20;
