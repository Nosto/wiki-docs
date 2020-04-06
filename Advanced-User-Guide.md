This guide is intended for advanced users and outlines and documents common questions and pain points encountered by large retailers with complex and performance-critical Magento deployments.

## Images

Nosto thumbnails and serves all images from our Cloudfront-backed CDN. After the original image is fetched, all recommendations are served by Nosto. There is no additional load your instance.

All images are thumbnailed into flattened JPEG images.

**Why do the images lose the transparency?**

As Nosto thumbnails all images into JPEGs, the alpha-channel (transparency) - as found on PNG images - are discarded. No PNG support exists on our roadmap. If it is necessary that the recommendations use an alternate version, you will need to customise our extension.

→ Learn more about [Overriding Product Data](Overriding-Product-Data.md)

## Multi-Currency

If your site uses multiple currencies, the extension works out of the box. Nosto's multi-currency offers spot-on real-time currency conversions by using the exchange-rates provided by your Magento installation to convert all the product prices on the fly. Nosto keeps track of your Magento's exchange-rates and does the conversion while serving recommendations. All conversions happen on Nosto and in no way does this feature impact your site's performance.

All that needs to be done is for multi-currency to be enabled. Multi-currency can be enabled by navigating to System >> Configuration >>  Nosto (under Services) and switching the option "Multi Currency Method" to "Exchange Rates".

![Multi-Currency Configuration](https://user-images.githubusercontent.com/327432/27001754-f181e500-4dd9-11e7-955a-9a3ed57115c8.png)

##### Third-Party Currency Middleware

If your site uses third-party currency middleware to that enables customers to purchase from multiple currencies, the extensions' built-in multi-currency feature may not function as intended. Some examples of third-party currency middleware are Global-E and UPS.

In order to support third-party currency middleware, there are two options:

1. Override the extension's exchange rate handling to fetch the rates from the third-party provider.
2. Only show recommendations in the base currency. For example, if you are an American retailer, it is likely that your base currency is USD and the bulk of your traffic comes from the US. In a scenario, like the one aforementioned, it is recommended that you only serve recommendations in the base-currency.

## Product Updates

The extension relies on observers to keep the product catalog in sync with Nosto. Every modification to the product in Magento, leads to the observer making an API to call to Nosto to update the product.

While, Nosto uses CloudFront to ensure that you request is likely to hit the closest edge node to your location, observer invocations may add 300-500ms to the product observer.

##### Disabling Observers

While not recommended, if you would like to disable observers, you may do this by navigating to System >> Configuration >>  Nosto (under Services) and toggling off "Real-time product updates to Nosto"

![Product Updates Configuration](https://user-images.githubusercontent.com/327432/27001753-f17f074a-4dd9-11e7-8055-c594fd96b51a.png)

This will disable all product-update API calls.

#### Bulk Updates / Cron Jobs

If you are synchronising products from your ERP/PIM system, it is necessary that you run a cron to bulk update products to Nosto.

→ Learn more about [Bulk Product Updates](Bulk-Product-Updates.md)

## Order Confirmations

Nosto relies on embedded semantic markup on the order-confirmation page to track orders. As a fallback, all orders are also sent to Nosto via an API call.

Our order-confirmation APIs allow for enhanced conversion tracking for payment-providers that may not show the standard Magento order-confirmation page.

Incorrect conversion tracking has a multitude of side effects:

1. Incorrect post-purchase ad retargeting. Example, a customer who purchased a shoe may still be retargeted on Facebook to purchase the same shoe.
2. Incorrect abandoned-cart emails. For example, a customer who purchased a shoe may be sent an abandoned-cart email.
3. Incorrect conversion analytics. For example, underperforming recommendations statistics in the dashboard.

If your site always shows an order-confirmation page, you may disable our order-confirmation API.

→ For more information [Contact our support](mailto:support@nosto.com)