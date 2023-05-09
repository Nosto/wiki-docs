# Shopify Markets Workaround

{% hint style="info" %}
This is a temporary workaround for loading recommendations based on Shopify Markets. Our team is currently working on a complete end-to-end support solution, which will be released later this year.

The workaround has been designed to minimise needed work in order to render products in correct price, currency and language, depending on the Market the end-customer is browsing in.
{% endhint %}

## Shopify Markets - JS API Workaround

### How it works

The workaround is based on a new JS API `migrateToShopifyMarket.` Using this API, you can load recommendations with following details updated for the current market, automatically.

1. Product URL - updates Product URL for every product in the recommendation to match the URL of the store's market. At the same time, the URL will retain all the attribution parameters from the original URL
2. Product Title - updates Product Title, for every product in the recommendation, to reflect the language of the current market
3. Product Description - updates Product description, for every product in the recommendation, to reflect the language of the current market
4. Product Category - updates Product category, for every product in the recommendation, to reflect the language of the current market. Please note that this only affects the main category (fetched from Shopify's "Product Type" field).
5. Product Variant Options - For those recommendations containing variant selectors, this API translates all the variant options and title to reflect the language of the current market.&#x20;

### Functional Limitations

* The workaround does not take Market-specific availability into account
* The workaround does not allow using Nosto's Search Product in any other language than the main account language
* The workaround does not work in Email Products
* You cannot use pricing filters in Nosto campaigns or Placements for any other currency than the main account currency - they will only look into the main currency price
* You will not have Analytics, Segments or Orders on a per-Market basis, but for the whole Nosto account and all Markets covered

{% hint style="info" %}
All of the above will be possible once we launched the end-to-end integration for Shopify Markets
{% endhint %}

### How to integrate

For more technical information, needed changes, available parameters & code examples please also visit our [Technical Details (how to use it)](technical-details-how-to-use-it.md) page.
