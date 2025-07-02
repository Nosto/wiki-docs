---
description: >-
  A summary of Shopify’s current limitations related to Post Purchase
  Extensions, including key considerations for implementation and merchant
  setup.
---

# Known Shopify Limitations

## Known Shopify Limitations

Shopify’s Post Purchase Extension framework introduces a number of platform-level limitations that merchants and developers should be aware of when implementing Nosto’s Post Purchase Upsell. These limitations are imposed by Shopify and apply universally, regardless of which app or provider is used.

This section outlines the most relevant ones, particularly for merchants with advanced setups.

## Payment Provider Restrictions

Not all payment providers are compatible with Shopify’s Post Purchase step. Specifically, **Shopify does not support payment gateways that require CVV/CVN re-entry** after the initial checkout step.

Known **unsupported payment gateways** include:

* Braintree
* PayPal Payments Pro
* Payflow Pro
* Eway

Additionally, **wallet-based and installment payment methods** are not supported. If the original purchase is made using any of the following, **Shopify will skip the post-purchase page entirely**:

* Apple Pay
* Google Pay
* Amazon Pay
* Klarna
* Affirm
* Afterpay

**For more information:**\
[Shopify's official documentation](https://shopify.dev/docs/apps/build/checkout/product-offers#limitations-and-considerations)

## Currency Limitation

Post Purchase Upsell offers **can only be shown in the** **store’s primary currency**.

This is a Shopify platform limitation. If a customer checks out in a different curren, the upsell step will not be shown.

## Order Value Threshold

Shopify only allows upsell products to be added if they have a value of **at least 1 unit of the store’s currency**.

Offers below this threshold (e.g. $0.50) will not be processed.
