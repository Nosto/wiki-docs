# Feature Scope & Limitations

This page describes how the Nosto Shopware plugin interacts with native Shopware features and highlights areas where behavior may differ or where only partial support is available. Understanding these behaviors helps merchants and developers correctly scope their storefront implementation and avoid unexpected results.

### Debugging with Custom Themes

When troubleshooting issues related to the Nosto Shopware plugin, we strongly recommend testing the behavior using the default Shopware storefront theme.

Custom themes and storefront overrides may alter template logic, routes, or data handling in ways that affect how Nosto functionality behaves.

Before reporting a bug to Nosto Support, please verify that the issue can also be reproduced when using the default Shopware theme without additional customizations.

If the issue only occurs with a custom theme, the problem is most likely caused by the theme implementation and should be investigated by the merchant or their development team.

### Feature Support Overview

The table below provides an overview of how the Nosto Shopware plugin interacts with selected Shopware features.

| Shopware Feature             | Plugin Support | Notes                                                                                       |
| ---------------------------- | -------------- | ------------------------------------------------------------------------------------------- |
| Multi-currency               | Partial        | Only the default currency price is exported to Nosto.                                       |
| Customer-specific pricing    | Partial        | Advanced Pricing is partially supported, but not all Shopware pricing rules are applied.    |
| Special prices               | Partial        | Prices are evaluated during product synchronization rather than during search requests.     |
| Product reviews              | Supported      | Review count and average rating can be synchronized if enabled in the plugin configuration. |
| External review systems      | Not supported  | Only the native Shopware review system is supported.                                        |
| Customer tagging             | Supported      | Custom tagging can be configured via `customerDataToNosto`.                                 |
| Alternate image exports      | Supported      | Multiple product images can be exported if enabled in the plugin configuration.             |
| Cart recommendation template | Supported      | Supports adding products to cart, cart reload, and restore-cart links.                      |

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
