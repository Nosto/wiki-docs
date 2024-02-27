# Shopify Markets v1

{% hint style="info" %}
Our Shopify Markets integration v1 release is specifically designed to handle all Market related part on the Front End. This minimizes effort in setting Nosto up with Shopify Markets, but also limits your possibility of customise your setup per Market. For full controll please use our [end-to-end integration](https://docs.nosto.com/shopify/shopify-markets).


{% endhint %}

## Shopify Markets - JS API version

### How it works

The current version is based on a new JS API `migrateToShopifyMarket.` Using this API, you can load recommendations with following details updated for the current market, automatically.

1. Product URL - updates Product URL for every product in the recommendation to match the URL of the store's market. At the same time, the URL will retain all the attribution parameters from the original URL
2. Product Title - updates Product Title, for every product in the recommendation, to reflect the language of the current market
3. Product Description - updates Product description, for every product in the recommendation, to reflect the language of the current market
4. Product Category - updates Product category, for every product in the recommendation, to reflect the language of the current market. Please note that this only affects the main category (fetched from Shopify's "Product Type" field).
5. Product Variant Options - For those recommendations containing variant selectors, this API translates all the variant options and title to reflect the language of the current market.&#x20;
6. Product Price - updates Price & List-Price showing to match currency and price of customers location.

### Considerations

* At this point the integration supports the main account language within Nosto. This means, that some features might be limited, if you're utilising several languages in one Market. Still, using the API you can display correct language in your Front End f.e. in Product Recommendations.
* As v1 is a pure Front End solution, analytics will appear in the main account currency and may not be accurate to actual sales data
* To personalise the user experience for different locations/markets, you can use our Segmentation & Insights feature

### How to integrate

For more technical information, needed changes, available parameters & code examples please also visit our [Technical Details (how to use it)](technical-details-how-to-use-it.md) page.
