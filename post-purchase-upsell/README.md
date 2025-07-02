---
description: >-
  Introduction to Nosto’s Post Purchase Upsell extension for Shopify - what it
  does, how it works, and what you need to enable it.
---

# Post-Purchase Upsell

## What is Post Purchase Upsell?

Nosto’s Post Purchase Upsell lets you present one last personalized product offer right after a customer completes payment but before they reach the Shopify Thank You page. This is done using Shopify’s Post Purchase Extensions framework, ensuring a seamless checkout flow with no redirects or popups.

The extension allows merchants to:

* Increase average order value with zero friction.
* Control the offer logic and experience via Nosto.
* Rely on Shopify to handle order editing and charging the payment method.

## How It Works in Shopify

1. Customer completes payment → Shopify creates the order and sets it to **On hold** status.
2. Nosto’s Post Purchase Upsell offer page is displayed inside the Shopify Checkout.
3. Customer accepts or declines the offer:
   * **If accepted**: Shopify edits the order and charges the payment method.
   * **If declined**: order continues to fulfillment.
4. Once the step completes, Shopify releases the order from **On hold** status.
